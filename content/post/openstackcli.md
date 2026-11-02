---
title: OpenstackCLI
date: 2021-03-30
url: /post/openstackcli
---


Some days ago I released [openstackcli](https://github.com/mendrugory/docker-openstackcli), a new containerized tool to work with the [Openstack API](https://docs.openstack.org/api-quick-start/).

This project was motivated by a missing feature in [OVH's Managed Kubernetes](https://www.ovhcloud.com/en/public-cloud/kubernetes/): snapshots of in-use volumes.

If you are already working with Kubernetes, you would like to do everything the Kubernetes way, including volume snapshots, and this is possible as you can see in the [docs](https://kubernetes.io/docs/concepts/storage/volume-snapshots/).

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

However, we have to detach the volume before taking the snapshot, because OVH's infrastructure does not yet support snapshots of attached volumes. Check the issue [here](https://github.com/ovh/public-cloud-roadmap/issues/77).

The main purpose of taking a snapshot of a detached volume is to keep the data consistent. On the other hand, not every application is running 24/7. We know that there are moments when the application is not actually being used and we can carry out some maintenance tasks, like backups. There are applications that are only used during office hours or during sporting events, so we can easily detect time windows when they are not in use. In that case, we would not need to detach and reattach the volumes before and after the snapshots.

Since we were working with OVH and we know that its Public Cloud is [Openstack](https://www.openstack.org/), why not work directly against the API?

## Openstackcli

If we want to work against the Openstack API from Kubernetes, we need a container with the necessary software. There are some Docker images available, but I packaged my own in order to add other tools like [jq](https://stedolan.github.io/jq/), which will be very useful for the scripts. Now we can launch Kubernetes [Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/) or [CronJobs](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/) to take the snapshots or perform any other maintenance task, like cleaning them up.

Remember that the pod needs access to the Openstack API.

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

I think this is a temporary solution, but I hope it is useful in the meantime.

* [Code](https://github.com/mendrugory/docker-openstackcli)
* [Repository](https://hub.docker.com/repository/docker/mendrugory/openstackcli)