---
title: OpenstackCLI
date: 2021-03-30
url: /post/openstackcli
---


Some days ago I released [openstackcli](https://github.com/mendrugory/docker-openstackcli), a new containerized tool to work with [Openstack API](https://docs.openstack.org/api-quick-start/).

The motivation of this project was driven by a missing feature from [OVH's Managed Kubernetes](https://www.ovhcloud.com/en/public-cloud/kubernetes/): Snapshots from in-use volumes.

If you are already working with Kubernetes, you would like to perform everything in a Kubernetes way, including volume's snapshots, and this is possible as you can see in the [docs](https://kubernetes.io/docs/concepts/storage/volume-snapshots/).

That would be as easy as:

```yaml
apiVersion: snapshot.storage.k8s.io/v1
kind: VolumeSnapshot
metadata:
  name: my-snapshot
spec:
  source:
    persistentVolumeClaimName: my-pvc
```

But, we should detach the volume before performing the snapshot due to OVH Infrastructure does not support yet to do it with an attached volume. Check the issue [here](https://github.com/ovh/public-cloud-roadmap/issues/77).

The main purpose of performing a snapshot to a non-attached volume is to keep the consistency in the data. Otherwise, no every application is working 24/7. We know that there is some moments where the application is not actually used and we can apply some maintenance tasks, like back-ups. You can find applications which are used during office hours or during some sport events and we can easily detect time windows where they are not used. In this way, we would not need to detach and attach the volumes before and after the snapshots.

As we were working with OVH, we know that its Public Cloud is [Openstack](https://www.openstack.org/), why don't we try to work directly against the API?

## Openstackcli

If we want to work against the Openstack's API from Kubernetes, we need a container with the necessary software. In this case there are some available docker image, but I packaged mine in order to add some other useful tools like [jq](https://stedolan.github.io/jq/) which will be very useful for the scripts. Now we can launch some Kubernetes [Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/) or [CronJobs](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/) in order to perform the snapshots or whatever other maintenance task, like clean them up.

Rememeber that the pod has to have access to the Openstack API

*Cronjob example*:

```yaml
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: my-cronjob
spec:
  schedule: 0 0 * * *
  jobTemplate:
    spec:
      template:
        spec:       
          containers:
          - name: snapshot
            image: mendrugory/openstackcli
            command: 
            - /bin/sh
            - -c
            - openstack volume snapshot create --force --volume my-wonderful-data my-snapshot
            env:
            # Remember to give access to the Openstack API
            ...              
```

## Conclusion

I think that this is a temporary solution, but I expect that it is useful meanwhile.

* [Code](https://github.com/mendrugory/docker-openstackcli) 
* [Repository](https://hub.docker.com/repository/docker/mendrugory/openstackcli)