---
title: Local Kubernetes Cluster
date: 2019-07-15
url: /post/local-kubernetes-cluster
---

We all know the potential of having Kubernetes in production, and how easy it makes building and maintaining distributed systems. When you want to test your complex application locally, there are tools like [minikube](https://github.com/kubernetes/minikube) or [microk8s](https://microk8s.io/) that are really good, but they are single-node clusters. What happens when I want to test my application locally, but in something closer to a real environment?

To help with this, I have been working on a [project](https://github.com/mendrugory/kubernetes-vagrant) that uses *Vagrant* to create a bunch of virtual machines that work as Kubernetes nodes (and other *stuff* that we will see later), and *Ansible* to configure them.

## Dependencies

* [Vagrant](https://www.vagrantup.com/)
* A Vagrant provider to run virtual machines (e.g. [VirtualBox](https://www.virtualbox.org/))
* [Ansible](https://www.ansible.com/)


## Requirements

* RAM: 16 GB recommended, at least 8 GB. 


## Clone the repository

```bash
$ git clone git@github.com:mendrugory/kubernetes-vagrant.git
$ cd kubernetes-vagrant
```


## Start up the virtual machines

One Kubernetes master and two Kubernetes nodes have been defined, all running Ubuntu 16.04.

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|

  config.vm.define "k8s1" do |k8s1|
    k8s1.vm.hostname = "k8s1"
    k8s1.vm.box = "ubuntu/xenial64"
    k8s1.vm.network "private_network", ip: "192.168.33.11" 
  end

  config.vm.define "k8s2" do |k8s2|
    k8s2.vm.hostname = "k8s2"
    k8s2.vm.box = "ubuntu/xenial64"
    k8s2.vm.network "private_network", ip: "192.168.33.21" 
  end

  config.vm.define "k8s3" do |k8s3|
    k8s3.vm.hostname = "k8s3"
    k8s3.vm.box = "ubuntu/xenial64"
    k8s3.vm.network "private_network", ip: "192.168.33.22" 
  end    

end
```

Run the following command to get the virtual machines up and running:

```bash
$ vagrant up
```

## Install Kubernetes

You only have to run the Ansible playbook `k8s.yml`:

```bash
$ ansible-playbook k8s.yml
```

The playbook finishes with a task that creates a file called `mykubeconfig`, which you should use to work with the newly created Kubernetes cluster through `kubectl`.

```bash
$ kubectl get nodes -o wide --kubeconfig mykubeconfig
NAME   STATUS   ROLES    AGE     VERSION   INTERNAL-IP     EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
k8s1   Ready    master   1m      v1.13.5   192.168.33.11   <none>        Ubuntu 16.04.6 LTS   4.4.0-151-generic   docker://18.6.1
k8s2   Ready    <none>   57s     v1.13.5   192.168.33.21   <none>        Ubuntu 16.04.6 LTS   4.4.0-151-generic   docker://18.6.1
k8s3   Ready    <none>   57s     v1.13.5   192.168.33.22   <none>        Ubuntu 16.04.6 LTS   4.4.0-151-generic   docker://18.6.1
```


## Deploy Kubernetes Applications

As examples, we are going to deploy some already packaged applications:

### Deploy 2 replicas of Nginx and expose them

```bash
$ kubectl run nginx --port=80 --image=nginx --replicas=2 --kubeconfig mykubeconfig 
$ kubectl expose deployment nginx --port=80 --type=NodePort --kubeconfig mykubeconfig 
```


### Deploy 2 replicas of the Apache web server and expose them

```bash
$ kubectl run httpd --port=80 --image=httpd --replicas=2 --kubeconfig mykubeconfig 
$ kubectl expose deployment httpd --port=80 --type=NodePort --kubeconfig mykubeconfig 
```


### Visit the Applications

Check the assigned ports:

```bash
kubectl get svc --kubeconfig mykubeconfig 
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
httpd        NodePort    10.103.208.92   <none>        80:30275/TCP   18s
nginx        NodePort    10.105.235.99   <none>        80:32741/TCP   70s
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP        8m
```


Make requests to any of the nodes:

```bash
$ curl 192.168.33.21:32741
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

```bash
$ curl 192.168.33.21:30275
<html><body><h1>It works!</h1></body></html>
```


## Exposing Applications

The servers only have private IPs, so the applications cannot be accessed from other computers. We are going to expose applications from our Kubernetes cluster using two approaches:

* Load Balancer
* Ingress

And we are not going to use any external packages.


## Load Balancer

When you run a Kubernetes cluster offered by a cloud provider and you wish to expose your services using a LoadBalancer Service, a load balancer belonging to the cloud provider (for instance, an [AWS ELB](https://aws.amazon.com/elasticloadbalancing/)) is launched and serves your application to the outside world.

We will try to replicate that behaviour. A new virtual machine will be launched as the load balancer (check the [Vagrantfile](https://github.com/mendrugory/kubernetes-vagrant/blob/master/lb/Vagrantfile)). Instead of installing software capable of balancing requests between our replicas, we will use [Netfilter](https://en.wikipedia.org/wiki/Netfilter) through `iptables`. The configuration is applied by the Ansible role `load-balancer`.

The virtual machine will get a public IP that is reachable by other computers on the same network.

### Start up the Load Balancer

#### Start up the virtual machine

```bash
$ vagrant up
```

#### Set up the load balancer for Nginx

We are not going to use any dedicated software to balance requests between the Nginx replicas. Instead, we will take advantage of Linux's **Netfilter** through **iptables** to redirect the requests that reach the load balancer server to the Nginx replicas.

You need to pass the IPs of the nodes, the port of the app and the network interface (`enp0s9` by default).

```bash
$ ansible-playbook lb.yml --extra-vars "server1=192.168.33.21 server2=192.168.33.22 app_port=32741 interface=enp0s9"
```

> Check the assigned public IP.

#### Visit the application through the Load Balancer

```bash
$ curl 192.168.1.143
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```


## Ingress

Exposing traffic with load balancers has a drawback: a whole load balancer is used for just one service. Ingresses help to serve more than one service. One of the most widely used is [Nginx](https://www.nginx.com/).

We are going to expose two applications (the Nginx web server and the Apache web server) through our own ingress. An Ansible role will deploy an Nginx that routes the traffic based on the URL prefix:

* /app1 => to nginx
* /app2 => to apache


The virtual machine will get a public IP that is reachable by other computers on the same network.

### Start up the Ingress

#### Start up the virtual machine

```bash
$ vagrant up
```

#### Set up the Ingress

You need to pass the IPs of the nodes and the ports of the apps.

```bash
$ ansible-playbook ingress.yml --extra-vars "server1=192.168.33.21 server2=192.168.33.22 app1_port=32741 app2_port=30275"
```


> Check the assigned public IP.

#### Visit the applications through the Ingress

```bash
$ curl 192.168.1.144/app1/
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

```bash
$ curl 192.168.1.144/app2/
<html><body><h1>It works!</h1></body></html>
```

## Conclusion

We have created a bunch of virtual servers using Vagrant with VirtualBox and configured them using Ansible. Besides that, taking advantage of our networking knowledge, we have created other publicly reachable virtual servers to expose traffic from our Kubernetes cluster.

Enjoy your k8s!!