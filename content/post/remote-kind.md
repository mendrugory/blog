---
title: "Launching Remote KIND (Kubernetes in Docker) Clusters"
date: 2021-12-10
url: /post/remote-kind
---

> NOTE: This content is just for testing and experimentation. It can cause many security issues, so do not use it in a production environment.

Some days ago, a question came up in a Slack channel at [Clastix](https://clastix.io/), the company I currently work for: *Could I launch a kind cluster on a remote machine?* My immediate thought was *yes, you could*, but let's see how.

[Kind](https://kind.sigs.k8s.io/) is a magnificent tool for people who develop for [Kubernetes](https://kubernetes.io/) or test their software directly on [Kubernetes](https://kubernetes.io/). Every Kind node is a [Docker](https://www.docker.com/) container, and this is the key to being able to launch remote Kind clusters.

First, we need a remote machine. If you already have one, you can skip the next section and go directly to the [Docker](#docker) section.

## Virtual Machine

For this experiment, a virtual machine will be used as the remote machine. You can use whichever system you are most comfortable with, but the example is based on [Multipass](https://multipass.run/) to use [KVM](https://en.wikipedia.org/wiki/Kernel-based_Virtual_Machine) on a Linux machine.

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

### Log into the Virtual Machine

```bash
ubuntu@local:~$ multipass shell remote
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-91-generic x86_64)
ubuntu@remote:~$ 
```

Now we have a remote Linux machine running Ubuntu 20.04.

## Docker

The Docker daemon is a server application that exposes an API to work with. By default, the Docker daemon is only exposed through a UNIX socket, but we can also expose a TCP socket by modifying the configuration.

First, we need Docker.

### Installation

```bash
ubuntu@remote:~$ sudo apt update
ubuntu@remote:~$ sudo apt install -y docker.io
```

### Configuration

The easiest way to manage Docker's configuration is through the file `/etc/docker/daemon.json`, where we are going to expose the daemon on the remote machine's IP. We will also keep the UNIX socket so we can still use the `docker CLI` from the remote machine:

```json
{
  "debug": true,
  "hosts": ["tcp://10.192.157.65", "unix:///var/run/docker.sock"]
}
```

Then check that the `ExecStart` field in the file `/lib/systemd/system/docker.service` looks like this:

```conf
ExecStart=/usr/bin/dockerd  --containerd=/run/containerd/containerd.sock
```

Finally, restart the service and add your user to the `docker` group to avoid using `sudo` (this takes effect after you log out and back in):

```bash
ubuntu@remote:~$ sudo systemctl daemon-reload
ubuntu@remote:~$ sudo systemctl restart docker
ubuntu@remote:~$ sudo gpasswd -a $USER docker
```

Now we can work with Docker:

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

### Pointing to the Remote Docker Daemon

The `Docker CLI` uses the environment variable `DOCKER_HOST` to build its requests, so:

```bash
export DOCKER_HOST="tcp://10.192.157.65"
```

Now all the `docker cli` requests will go to the remote machine:

```bash
ubuntu@local:~$ docker images
REPOSITORY     TAG       IMAGE ID       CREATED      SIZE
kindest/node   v1.23.0   b3dd68fe0a8c   2 days ago   1.46GB
```

## Kind

The first step is to install Kind. Check how to install it [here](https://kind.sigs.k8s.io/docs/user/quick-start#installation).

### Check Kind Version

```bash
ubuntu@local:~$ kind --version
kind v0.11.1
```

### Launch a Remote Kind Cluster

Kind offers a very flexible configuration, but in our case we will focus on the `API Server`.

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

As we said, Kind's nodes are Docker containers, so let's check them:

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
