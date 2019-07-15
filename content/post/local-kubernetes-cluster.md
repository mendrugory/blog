---
title: Local Kubernetes Cluster
date: 2019-07-15
url: /post/local-kubernetes-cluster
---

We all know the potential of having Kubernetes in production. How easy is to build distributed systems and maintain them. When you want to locally test your complex application there are some tools like: [minikube](https://github.com/kubernetes/minikube) or [microk8s](https://microk8s.io/) what are really good, but they are one-single node cluster. What happen when I want to test my application, locally, but even closer to a real environment?

To help with this issue, I have been working on a [project](https://github.com/mendrugory/kubernetes-vagrant) which uses *Vagrant* to create a bunch of virtual machines which will work as Kubernetes nodes, and other *stuffs* that we will see later, and *Ansible* to configure them.

## Dependencies

* [Vagrant](https://www.vagrantup.com/)
* A Vagrant provider to run virtual machines (i.e. [VirtualBox](https://www.virtualbox.org/))
* [Ansible](https://www.ansible.com/)


## Requirements

* RAM: 16 GB recommended, at least 8 GB. 


## Clone the repository

```bash
$ git clone git@github.com:mendrugory/kubernetes-vagrant.git
$ cd kubernetes-vagrant
```


## Start up the virtual machines

Only one Kubernetes master and two Kubernetes nodes has been defined which will run Ubuntu 16.04.

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

Run the following command to have the cluster up and running:

```bash
$ vagrant up
```

## Install Kubernetes

You only have to run the Ansible Playbook, `k8s.yml`:

```
$ ansible-playbook k8s.yml
```

Ansible Playbook finishes with a task which will create a file called `mykubeconfig` which should be used to work with the just created kubernetes cluster through the tool `kubectl`.

```
$ kubectl get nodes -o wide --kubeconfig mykubeconfig
NAME   STATUS   ROLES    AGE     VERSION   INTERNAL-IP     EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
k8s1   Ready    master   1m      v1.13.5   192.168.33.11   <none>        Ubuntu 16.04.6 LTS   4.4.0-151-generic   docker://18.6.1
k8s2   Ready    <none>   57s     v1.13.5   192.168.33.21   <none>        Ubuntu 16.04.6 LTS   4.4.0-151-generic   docker://18.6.1
k8s3   Ready    <none>   57s     v1.13.5   192.168.33.22   <none>        Ubuntu 16.04.6 LTS   4.4.0-151-generic   docker://18.6.1
``` 


## Deploy Kubernetes Applications

We are going to deploy already packaged applications as examples:

### Deploy 2 replicas of Nginx and expose them.

```
$ kubectl run nginx --port=80 --image=nginx --replicas=2 --kubeconfig mykubeconfig 
$ kubectl expose deployment nginx --port=80 --type=NodePort --kubeconfig mykubeconfig 
```


### Deploy 2 replicas of Apache Web server and expose them.
```
$ kubectl run httpd --port=80 --image=httpd --replicas=2 --kubeconfig mykubeconfig 
$ kubectl expose deployment httpd --port=80 --type=NodePort --kubeconfig mykubeconfig 
```


### Visit the Applications

Check out the assigned Ports.

```
kubectl get svc --kubeconfig mykubeconfig 
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
httpd        NodePort    10.103.208.92   <none>        80:30275/TCP   18s
nginx        NodePort    10.105.235.99   <none>        80:32741/TCP   70s
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP        8m
```


Make requests to any of the nodes

```
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

```
$ curl 192.168.33.21:30275
<html><body><h1>It works!</h1></body></html>
```


## Exposing Applications

The servers only have private IPs, therefore it would not be possible to access to the applications from other computers. We are going to expose applications from our Kubernetes cluster using two approaches:

* Load Balancer
* Ingress

But we are not going to use any external packages.


## Load Balancer

When you run a Kubernetes cluster offered by a Cloud provider and you whish to expose your services using a Load Balancer Service, a load balancer which belongs to the cloud provider (for instance [AWS ELB](https://aws.amazon.com/elasticloadbalancing/)) will be launched and it will serve our application to the outside world.

We will try to replicate that behaviour. A new virtual machine will be launched (check [Vagrantfile](https://github.com/mendrugory/kubernetes-vagrant/blob/master/lb/Vagrantfile)) as load balancer. Instead of installing any software product with the capacity of balancing requests between our replicas, we will use [Netfilter](https://en.wikipedia.org/wiki/Netfilter) through the tool `iptables`. The configuration will be applied by the Ansible role `load-balancer`.

The virtual machine will receive a public IP which will be accessible by other computers in the same network.

### Start up the Load Balancer

#### Start up the virtual machine

```
$ vagrant up
```

#### Set up the load balancer for the Nginx

We are not going to use any dedicated software to balance requests between the nginx replicas, we will take advantage of the **Netfilter** of Linux through the application **iptables** to redirect the requests that arrive to the load balancer server to the Nginx replicas.

It is necessary to pass the IPs of the nodes, the port of the app and the network interface (by default will be `enp0s9`).

```
$ ansible-playbook lb.yml --extra-vars "server1=192.168.33.21 server2=192.168.33.22 app_port=32741 interface=enp0s9"
```

> Check the given Public IP.

#### Visit the application through the Load Balancer

```
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

Exposing traffic using load balancers have a drawback and a complete load balancer is used for only one service-application. Ingresses try to help to serve more than one service. One of the most used is [Nginx](https://www.nginx.com/).

We are going to try to expose two applications (nginx web server and apache web server) through our own ingress. An Ansible role will deploy a Nginx which will route the traffic based on the prefix url:

* /app1 => to nginx
* /app2 => to apache


The virtual machine will receive a public IP which will be accessible by other computers in the same network.

### Start up the Ingress

#### Start up the virtual machine

```
$ vagrant up
```

#### Set up the Ingress

It is necessary to pass the IPs of the nodes and the ports of the apps.

```
$ ansible-playbook ingress.yml --extra-vars "server1=192.168.33.21 server2=192.168.33.22 app1_port=32741 app2_port=30275"
```


> Check the given Public IP.

#### Visit the applications through the Ingress

```
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

```
$ curl 192.168.1.144/app2/
<html><body><h1>It works!</h1></body></html>
```

## Conclusion 

We have been able to create a bunch of virtual servers using Vagrant with Virtualbox and configure them using Ansible. Besides, taking advantage of our networking knowledge we have created other virtual servers, which will be accessible, to expose traffic into our Kubernetes cluster.

Enjoy your k8s !!