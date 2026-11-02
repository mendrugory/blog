---
title: "How to resize your persistent volume in Kubernetes?"
date: 2019-09-14
url: /post/kubernetes-persistent-volumes
description: Working with data in Kubernetes means that you will manage persistent volumes. Resizing your pv is not simple if your cloud provider does not support yet
---

One of the first steps when you work with data that must be stored is estimating the disk size. This is really hard, especially when your application is new.

Working in a cloud environment gives us the flexibility to use the resources we need at *almost* any moment, without wasting money on resources that we will only use in the future, if everything goes well.

If your application runs on a cloud virtual machine and needs to store some data, the usual approach is to attach a new disk for that data instead of using the virtual machine's default disk. This new disk could then be used by another virtual machine if the current one has any problem, without losing the data.

Attaching new disks to our cloud virtual machines is very common, but what happens when we work with containers?

Some new concepts come into play when we work with containers in a cloud environment, using Kubernetes for example. The application runs directly in containers (although the containers live on the virtual machines), and the persistent data is stored in [persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) (although these volumes are backed by attached disks).

**It has been possible to resize a persistent volume in Kubernetes** since version 1.11, but not all cloud providers support it. So what should we do if we need a bigger volume?

> It is not possible in Azure AKS at the moment.

The first idea that comes to mind is to attach a new volume to the container, as we would do with a disk and a virtual machine. But it is not possible to attach new volumes to running containers.

So we think about stopping the application for a while and migrating the data between volumes. How can we do it?

> The examples are based on Azure AKS.


#### Delete the Pod

The first step should be to stop the pod, to avoid writes in the middle of the migration that could cause problems.

```bash
$ kubectl delete -f myapp.yml
```

> I assume that the definition of your current deployment/replicaset/pod is in myapp.yml.

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

The next step is to run a helper pod, with both volumes attached, which we will use to migrate the data.

*helper.yml*

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

An Ubuntu container will be started with the two volumes attached to it. The current data will be in */data/old*, and the new volume, */data/new*, will be empty. The main process is a *sleep* command that lasts one hour. If you need more time for your tasks, increase it.

```bash
$ kubectl apply -f helper.yml
```

Once the container is up and running, we connect to it:

```bash
$ kubectl exec -it <name of the pod> bash
```

And run the migration, for example, copying all the data from the old volume to the new one:

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

The persistent volume claim requests a disk from the cloud provider, and that costs money, so if we are not going to use the old volume anymore, we should delete it:

```bash
$ kubectl delete pvc my-claim
```


## Conclusion

Working in the cloud is very flexible, but we always have to take into account its limitations and the problems we may face in the future. Besides, working with containers increases flexibility, but it also increases the complexity of the environment.