---
title: Mesos for Local Development
date: 2018-11-13
url: /post/mesos-local-development
---

Last week I felt nostalgic about working with [Mesos](http://mesos.apache.org/), because I no longer have any projects that use this technology and people only ask for [Kubernetes](http://kubernetes.io/).

Mesos is a project that was born at Berkeley, is now under the [Apache umbrella](https://www.apache.org/index.html#projects-list), and whose goal is managing computer clusters. It helps us create distributed systems with the feeling that we are working with a single, really big machine. Mesos gathers the resources of all the machines that join the cluster and takes care of deploying applications wherever there are enough resources.

Mesos is very powerful and, according to some studies, it can scale to a huge number of nodes much better than Kubernetes. But we don't need to compare Mesos and Kubernetes because, according to the [Mesosphere](https://mesosphere.com/blog/kubernetes-dcos/) folks, they can work together (one on top of the other).

I worked on Mesos projects, with its orchestrator [Marathon](https://mesosphere.github.io/marathon/), for 3 years, and the results were really good, but the market is moving towards Kubernetes and we have to go where the market goes.

So I wanted to release a useful tool for those who want to play with Mesos or try their systems locally (if they can). The project launches 3 virtual machines that form the Mesos cluster.

## Dependencies

* [Vagrant](https://www.vagrantup.com/)
* A Vagrant provider to run virtual machines (e.g. [VirtualBox](https://www.virtualbox.org/))
* [Ansible](https://www.ansible.com/)


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

Now you can check any of the Mesos servers (e.g. [192.168.33.11:5050](http://192.168.33.11:5050)):

![Mesos Dashboard](/img/mesos-dashboard.png)


And the Marathon servers (e.g. [192.168.33.11:8080](http://192.168.33.11:8080)):

![Marathon Dashboard](/img/marathon-dashboard.png)


I also installed [Docker](https://www.docker.com/), so we can try launching 3 replicas of Nginx in our Mesos cluster:


![Nginx Marathon General](/img/nginx-marathon-1.png)

![Nginx Marathon General](/img/nginx-marathon-2.png)


And after a while we will see:

![Nginx Marathon General](/img/nginx-marathon-3.png)

![Nginx Marathon General](/img/nginx-marathon-4.png)

![Nginx Marathon General](/img/nginx-marathon-5.png)


You can check the repo [here](https://github.com/mendrugory/mesos-vagrant).