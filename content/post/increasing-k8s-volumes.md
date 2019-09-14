---
title: "And if you can't resize your persistent volume in Kubernetes?"
date: 2019-09-14
url: /post/increasing-k8s-volumes
---

One of the first steps when you work with data which must be stored is to estimate the disk size. This is really hard, especially when you are application is new.

Working in a cloud environment gives us the flexibility of using the resources that we need in *almost* any moment without wasting our money in resources that we will use in the future, if everything goes right.

If your application is working in a cloud virtual machine and it needs to store some data, the approach would be to attach a new disk for this data, instead of using the default disk of the virtual machine. This new disk could be used by other virtual machine if the current one suffers any problem, without loosing the data.

Attaching new disks to our cloud virtual machines is very common but, what would happen if we work with containers?

Some new concepts are introduced when we work with containers, using for example Kubernetes, in a cloud environment. The application will run, directly, in containers, although the containers will be in the virtual machines, and the persistent data will be stored in [persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/), although this volumes will be in attached disks.

It is possible to resize a persistent volume in Kubernetes since version 1.11, but it is not supported by all the cloud providers. Therefore, what should we do if we need a bigger volume?

> It is not possible in Azure AKS at this moment.

The first idea that comes up is to attach a new volume to the container, like we would do with a disk and a virtual machine. But it is not possible to attach new volumes to running containers.

So we think in stopping the application for a while and migrate the data between volumes. How can we do it?

> The examples are based on Azure AKS.


#### Delete the Pod

The first step should be to stop the pod in order to avoid writes in the middle of the migration which could cause problems.

```
$ kubectl delete -f myapp.yml
```

> I guess that the definition of your current deployment/replicaset/pod is in myapp.yml

#### Bigger Volume

The second step is to provide a bigger volume.

*pvc.yml*

```diff
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-claim
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: managed-premium
  resources:
    requests:
      storage: 100Gi
---
+ apiVersion: v1
+ kind: PersistentVolumeClaim
+ metadata:
+   name: my-new-claim
+ spec:
+   accessModes:
+   - ReadWriteOnce
+   storageClassName: managed-premium
+   resources:
+     requests:
+       storage: 150Gi
```

> A new disk will be provisioned by the cloud provider, Azure in this case.

```bash
$ kubectl apply -f pvc.yml
```

#### Start a helper Pod

The next step is to run a helper pod where we will attach the two volumes and it will be used to execute the migration of the data.

helper.yml

```yml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: helper
spec:
  replicas: 1
  template:
    metadata:
      name: helper
      labels:
        app: helper
    spec:
      containers:
      - name: helper
        image: ubuntu
        command: 
        - "/bin/sleep"
        - "3600"
        volumeMounts:
          - name: my-new-pv
            mountPath: /data/new
          - name: my-pv
            mountPath: /data/old  
      volumes:
      - name: my-pv
        persistentVolumeClaim:
          claimName: my-claim
      - name: my-new-pv
        persistentVolumeClaim:
          claimName: my-new-claim
```

A ubuntu container will be started and two volumes will be attached to it. The current data will be found in */data/old* and the new one, */data/new* will be empty. The main process is a *sleep* command which will take one hour. If you need more time to execute your tasks, increase it.

```bash
$ kubectl apply -f helper.yml
```

Once the container is up and running, we will connect to it:

```bash
$ kubectl exec -it <name of the pod> bash
```

And we will execute the migration, for example, copy all the data from old volume to the new one:

```bash
$ cp -r /data/old /data/new
```

Finally, we can delete the helper:

```bash
$ kubectl delete -f helper.yml
```


#### Run the application with the new volume

Once the migration has finished, we can start the application pointing to the new volume.


*myapp.yml*

```diff
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 1
  template:
    metadata:
      name: my-app
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: myapp
        env:
        - name: DATA
          value: /var/data
        volumeMounts:
+       - name: my-new-pv
+         mountPath: /var/data
-       - name: my-pv
-         mountPath: /var/data
        ports:
        - containerPort: 8888
          name: my-port
      volumes:
+     - name: my-new-pv
+       persistentVolumeClaim:
+         claimName: my-new-claim
-     - name: my-pv
-       persistentVolumeClaim:
-         claimName: my-claim          
```

```bash
$ kubectl apply -f myapp.yml
```

#### Remove the old Persistent Volume

The persistent volume claim requests a disk to the cloud provider and it costs money so, if we are not going to use the old volume anymore, we should delete it:

```bash
$ kubectl delete pvc my-claim
```


## Conclusion

Working in the cloud is very flexible but we always have to take into account the limitations or the future problems that we can have, besides working with containers increases the flexibility but also increases the complexity of the environment.