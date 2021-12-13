---
title: "Launching Remote KIND (Kubernetes in Docker) Clusters"
date: 2021-12-10
url: /post/remote-kind
---

> NOTE: This content is just for testing and experimentation. It can provoke many security issues therefore, do not use in a production environment.

Some days ago, a question was raised in a Slack channel of [Clastix](https://clastix.io/), the company I currently work for: *Could I launch a kind cluster in a remote machine?* I directly thought, *yes, you could*, but let's see how.

[Kind](https://kind.sigs.k8s.io/) is a magnificient tool for those people who develop for [Kubernetes](kubernetes.io/) or test their SW directly in [Kubernetes](kubernetes.io/). Every Kind's nodes will be a [Docker](https://www.docker.com/) container and this is the key part of having the possibility of launching remote Kind clusters.

Firstly, we need to have a remote machine. If you already have it, you can skip next section and go directly to [Docker](#docker) section.

## Virtual Machine

For this experiment, a virtual machine will be used as remote machine. You can work with the system you are more comfortable with but the example is based on [Multipass](https://multipass.run/) to use [KVM](https://en.wikipedia.org/wiki/Kernel-based_Virtual_Machine) in a Linux machine.

### Launch Virtual Machine

```bash
ubuntu@local:~$ multipass launch --name remote
```

### List Virtual Machines

```bash
ubuntu@local:~$ multipass list                   
Name                    State             IPv4             Image
remote                  Running           10.192.157.65    Ubuntu 20.04 LTS
```

### Go into

```bash
ubuntu@local:~$ multipass shell remote
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-91-generic x86_64)
ubuntu@remote:~$ 
```

Now we have a remote Linux machine running Ubuntu 20.04

## Docker

Docker daemon is a server application which exposes an API to work with. By default, docker daemon is only exposed through a UNIX socket but we can also expose a TCP socket modifying the configuration.

Firstly, we must have docker.

### Installation

```bash
ubuntu@remote:~$ sudo apt update
ubuntu@remote:~$ sudo apt install -y docker.io
```

### Configuration

The easiest way to manage docker's configuration is using the file `/etc/docker/daemon.json` where we are going to expose the daemon using the remote machine IP and we will also leave the UNIX socket to still working with `docker CLI` from the remote machine:

```json
{
  "debug": true,
  "hosts": ["tcp://10.192.157.65", "unix:///var/run/docker.sock"]
}
```

And check that `ExecStart` field within file `/lib/systemd/system/docker.service` is like:

```conf
ExecStart=/usr/bin/dockerd  --containerd=/run/containerd/containerd.sock
```

Finally, restart the service and add your user to the `docker` group to avoid using `sudo` (it will work after finishing the session):

```bash
ubuntu@remote:~$ sudo systemctl daemon-reload
ubuntu@remote:~$ sudo systemctl restart docker
ubuntu@remote:~$ sudo gpasswd -a $USER docker
```

And we can work with docker:

```bash
ubuntu@remote:~$ docker pull kindest/node:v1.23.0
v1.23.0: Pulling from kindest/node
cef70b0687d2: Pull complete 
1a0fe9cf114d: Pull complete 
Digest: sha256:49824ab1727c04e56a21a5d8372a402fcd32ea51ac96a2706a12af38934f81ac
Status: Downloaded newer image for kindest/node:v1.23.0
docker.io/kindest/node:v1.23.0

ubuntu@remote:~$ docker images
REPOSITORY     TAG       IMAGE ID       CREATED      SIZE
kindest/node   v1.23.0   b3dd68fe0a8c   2 days ago   1.46GB
```

Let's go back to our local machine.

### Pointing Remote Docker Daemon

`Docker CLI` uses environment variable `DOCKER_HOST` to build their requests, so:

```bash
export DOCKER_HOST="tcp://10.192.157.65"
```

Now, all the `docker cli` requests will go to the remote machine

```bash
ubuntu@local:~$ docker images
REPOSITORY     TAG       IMAGE ID       CREATED      SIZE
kindest/node   v1.23.0   b3dd68fe0a8c   2 days ago   1.46GB
```

## Kind

The first step is installing Kind. Check how to install it [here](https://kind.sigs.k8s.io/docs/user/quick-start#installation).

### Check Kind Version

```bash
ubuntu@local:~$ kind --version
kind v0.11.1
```

### Launch a Remote Kind Cluster

Kind offers a very flexible configuration, but for our case, we will be focus on the `API Server`.

```bash
ubuntu@local:~$ cat <<EOF | kind create cluster --image=kindest/node:v1.23.0 --name remote --config=-
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
networking:
  apiServerAddress: "10.192.157.65"
  apiServerPort: 6443
EOF
Creating cluster "remote" ...
 ✓ Ensuring node image (kindest/node:v1.23.0) 🖼
 ✓ Preparing nodes 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
Set kubectl context to "kind-remote"
You can now use your cluster with:

kubectl cluster-info --context kind-remote

Have a question, bug, or feature request? Let us know! https://kind.sigs.k8s.io/#community 🙂
```

### Check your current context

```bash
ubuntu@local:~$ kubectl config current-context
kind-remote
```

### Check the Nodes

As we said, Kind's nodes are Docker containers therefore, let's check them:

```bash
ubuntu@local:~$ docker ps
CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS              PORTS                          NAMES
e9e6723bf875        kindest/node:v1.23.0   "/usr/local/bin/entr…"   2 minutes ago       Up 2 minutes        10.192.157.65:6443->6443/tcp   remote-control-plane
```

```bash
ubuntu@local:~$ kubectl get nodes
NAME                   STATUS   ROLES                  AGE     VERSION
remote-control-plane   Ready    control-plane,master   2m36s   v1.23.0
```

### Deploy Applications

Kind is ready to run your applications.

```bash
ubuntu@local:~$ kubectl run nginx --image=nginx:alpine
pod/nginx created

ubuntu@local:~$ kubectl get po
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          22s
```

Let's check the app:

```bash
ubuntu@local:~$ kubectl port-forward nginx 8080:80 &
[1] 587897
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80

ubuntu@local:~$ curl localhost:8080
Handling connection for 8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
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
