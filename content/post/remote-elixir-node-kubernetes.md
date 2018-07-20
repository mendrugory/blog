---
title: Remote Elixir/Erlang Node connected to Kubernetes
date: 2018-07-20
url: /post/remote-elixir-node-kubernetes
---

Kubernetes is becoming the _standard_ container orchestrator because of many reasons:

* Open Source
* Google
* Huge community
* Developed in Go
* and so on

Today we can work in very complex projects which involved too many languages and technologies. Containers (like Docker) and orchestration tools help a lot in these cases.

Elixir and Erlang are two languages that I work with, especially when I have to deal with problems of concurrency and low latency. Working with these two languages directly give you the possibility of building distributed systems in a really easy way. Erlang/Elixir purists normally try to avoid containers and therefore orchestration tools. It is true that the Erlang ecosystem provides all the necessary tools in order to build a fault-tolerant distributed system so, why are you going to use containers? But we are facing a different problem where we are going to use the best tool for each job. When different tools/languages are used in the same project and all of them are going to create a heterogeneous distributed system, containers and orchestration tools are the best approach that I know.

Create an Elixir/Erlang docker image and run the container could be easy. Running it inside Kubernetes is easy as well. Building a distributed erlang system in Kubernetes could be more complex but connecting a local node to kubernetes erlang/elixir node that is part of an Erlang cluster is really challenging.

Let's explain how Distributed Erlang works.

## Distributed Erlang

Erlang ecosystem provides a method to connect distributed nodes which is called _Distributed Erlang_.

When an Erlang/Elixir node is going to be started, we have to provide a _cookie_ and a _long name_. The cookie has to be the same in all nodes that belongs to the same cluster. The long name will contain a unique name inside the same server, usually the name of the application, and the IP of the server separate by an _@_, for instance, app@192.168.1.10.

Erlang:
```bash
$ erl -name app@192.168.1.10 -setcookie cookie
```

Elixir:
```bash
$ iex --name app@192.168.1.10 --cookie cookie
```

Once we have our node up and running, we can connect our node to another known one:

Erlang:
```bash
1> net_kernel:connect_node('otherapp@192.168.1.22').
```

Elixir:
```bash
iex(1)> Node.connect(:"otherapp@192.168.1.22")
```

Distributed Erlang uses TCP by default as and you know we need a port to open a TCP socket. The node will request the port to the [epmd](http://erlang.org/doc/man/epmd.html) server which will be running in the machine whose IP is part of the long name of the node that we want to connect with. The epmd server runs on port 4369 by default.

![Distributed Erlang](/img/distributed_erlang.png)

## Erlang/Elixir and Kubernetes

Networking in Kubernetes is not easy, especially when we have to make _special things_. The most important part in order to get our Erlang distributed cluster is to have a reliable way of getting the IP of the pods in order to be reacheble by other Erlang/Elixir nodes. StatefulSet will be the Kubernetes object that will be used. It provides a known name of the pod (__<name of the app>-<0..>.<selector>.default.svc.cluster.local__) which will be included as DNS entry.
Different environment variables will be included in the yaml definition:

* ERL_PORT: The Erlang Port
* REPLACE_OS_VARS: Variable which says to the BEAM to change some configuration values for the given environment variables
* HOST: The IP of the pod

The pod will expose two ports: The epmd port and the erlang port. The service will indicate that we don't need cluster ip: `clusterIP: None`.

vm.args

```erlang
## Name of the node
-name app@${HOST}
# Port where the EVM will be listened by others EVM
-kernel inet_dist_listen_min ${ERL_PORT} inet_dist_listen_max ${ERL_PORT}
...
```

app.yaml
```yaml
apiVersion: apps/v1beta1
kind: StatefulSet
metadata:
  name: app
spec:
  serviceName: "app"
  replicas: 1
  template:
    metadata:
      name: app
      labels:
        app: app
    spec:
      containers:
      - name: app
        image: myregistry/app
        env:
        - name: ERL_PORT
          value: "5555"
        - name: REPLACE_OS_VARS
          value: "true"
        - name: HOST
          valueFrom:
            fieldRef:
              fieldPath: status.podIP
        ports:
        - containerPort: 4369
          name: epmd
        - containerPort: 5555
          name: erlang
---
apiVersion: v1
kind: Service
metadata:
  name: app
spec:
  ports:
  - port: 4369
    targetPort: 4369
    name: epmd
  - port: 5555
    targetPort: 5555
    name: erlang  
  clusterIP: None  
  selector:
    app: app
```

Now we have our Erlang/Elixir application up and running inside our Kubernetes cluster. If we resolve the IP for the name __app-0.app.default.svc.cluster.local__ and we connect to __app@IP__ we will have our erlang cluster of two nodes.

## Connecting from Outside Kubernetes

Kubernetes isolates the cluster from outside connections without adding any other effort from our part, which is really good for our production environments. But sometimes we need to connect to the production environments and for our Erlang/Elixir applications, which are part of an Erlang cluster, is a little bit tricky.

Kubectl can forward traffic from some ports of a chosen pod, so we can forward the epmd port and the erlang port of our application:

```bash
kubectl port-forward --namespace=default app-0 4369 5555 &
```

Now, we have the ports 4369 andd 5555 of the pod and of our machine connected.

![Port Forwarding](/img/port_forwarding.png)

If we try to start a BEAM node, it will be registered in the epmd of the Kubernetes cluster. But how will connect our local node to the kubernetes node? We know the name "app" but, which IP should be used?

* Pod IP `app@10.244.1.8_`: That IP is not reachable from our local host, therefore it will not be possible.
* Localhost `app@127.0.0.1`: Such as the ports are connected to the pod's ports, the epmd communication would work and the initial handshake with the application as well, but the communication will not be established because the node is _app@10.244.1.8_.

We need to use the pod ip but it has to work as local. The solution would be to redirect all the output traffic whose destination is the pod's ip to localhost.

### Iptables to the rescue

#### Creating Rules
```bash
$ sudo iptables -t nat -A OUTPUT -d 10.244.1.8 -p tcp --dport 4369 -j DNAT --to-destination 127.0.0.1:4369
$ sudo iptables -t nat -A OUTPUT -d 10.244.1.8 -p tcp --dport 5555 -j DNAT --to-destination 127.0.0.1:5555
$ sudo iptables -t nat -L
```

#### Connection

Erlang:
```bash
1> net_kernel:connect_node('app@10.244.1.8').
```

Elixir:
```bash
iex(1)> Node.connect(:"app@10.244.1.8")
```

#### Deleting Rules

After finishing the connections we should remove the rules

```bash
$ sudo iptables -t nat -v -L OUTPUT -n --line-number
$ sudo iptables -t nat -D OUTPUT <line of rule for 4369>
$ sudo iptables -t nat -D OUTPUT <line of rule for 5555>
$ sudo iptables -t nat -L
```

I hope that this post is useful for more people. Comments are welcoming !!