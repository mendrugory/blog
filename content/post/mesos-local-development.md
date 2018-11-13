---
title: Mesos for Local Development
date: 2018-11-13
url: /post/mesos-local-development
---

Last week I felt so nostalgic about working with [Mesos](http://mesos.apache.org/) because I do not have more projects which work with these technology and people are only demanding [Kubernetes](http://kubernetes.io/).

Mesos is a project which was born in Berkeley, now under the [Apache umbrella](https://www.apache.org/index.html#projects-list), and whose target is the management of computer clusters. It will help us to create distributed systems with the feeling that we are only working with a really big machine. Mesos gather the resources of all the machines that join the cluster and take care of deploying the application in those places with enough resources.

Mesos is so powerful and according to some studies, it can scale to huge number of nodes much better than Kubernetes. But we don't need to compare Mesos and Kubernetes, because according to [Mesosphere](https://mesosphere.com/blog/kubernetes-dcos/) guys, they can work together (one on top another).

I have been working with Mesos projects, and its orchestrator Marathon, for 3 years, and the results were really good, but the movement of the market is towards Kubernetes and we have to stay where the market says.

Therefore, I wanted to release a repository in Github which will launch 3 virtual machines as cluster where Mesos will run. This tool is useful for those guys who want to play with Mesos or to try your systems locally (if you can).

## Dependencies

* (Vagrant)[https://www.vagrantup.com/]
* A Vagrant provider to run virtual machines (i.e. [VirtualBox](https://www.virtualbox.org/))
* (Ansible)[https://www.ansible.com/]


## Clone the repository

```bash
$ git clone git@github.com:mendrugory/mesos-vagrant.git
$ cd mesos-vagrant
```

## Start up the virtual machines

```bash
$ vagrant up
```

## Configure the virtual machines

```bash
$ ansible-playbook mesos.yml
```

Now you can check whatever of the mesos servers (i.e. [192.168.33.11:5050](http://192.168.33.11:5050)):

![Mesos Dashboard](/img/mesos-dashboard.png)


And Marathon servers (i.e. [192.168.33.11:8080](http://192.168.33.11:8080)):

![Marathon Dashboard](/img/marathon-dashboard.png)


I also installed [Docker](https://www.docker.com/), therefore we could try to launch 3 replicas of Nginx in our Mesos Cluster:


![Nginx Marathon General](/img/nginx-marathon-1.png)

![Nginx Marathon General](/img/nginx-marathon-2.png)

And after a while we will see:

![Nginx Marathon General](/img/nginx-marathon-3.png)

![Nginx Marathon General](/img/nginx-marathon-4.png)

![Nginx Marathon General](/img/nginx-marathon-5.png)


You can check the repo [here](https://github.com/mendrugory/mesos-vagrant).