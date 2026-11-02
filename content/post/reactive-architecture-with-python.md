---
title: "Reactive Architecture with Python"
date: 2016-03-26
url: /post/reactive-architecture-with-python
index: true
---

Nowadays every system tries to be a real-time system, or at least a soft real-time system. To achieve this, most systems overuse the [pull](https://en.wikipedia.org/wiki/Pull_technology) approach. This causes a lot of useless requests that lead to overloading. Why are they useless? Because this approach tries to catch changes by requesting over and over again, but there will always be a delay: the new data has to already be there when the request is made.

Today we have languages like [Erlang](https://www.erlang.org/) or [Elixir](http://elixir-lang.org/) (I am very excited about Elixir and you will see some posts about it in the future), or frameworks like [Akka](http://akka.io/). But what can we do when we have a lot of complex algorithms written in a language like [Python](https://www.python.org/), or our team does not know Erlang?

For this reason I decided to build a proof of concept of a reactive architecture using [Python](https://www.python.org/). The PoC is a system that tracks the CPU usage of a machine, although it is (almost) ready to measure the resource usage of many machines.

To build a complete system, I chose [Tornado Web Framework](http://www.tornadoweb.org/en/stable/), [RabbitMQ](http://www.rabbitmq.com/) and [AngularJS](https://angularjs.org/) for the different modules.

The first module, [resources-tracker](https://github.com/mendrugory/reactive-architecture-python/tree/master/resources-tracker), is in charge of measuring the CPU and sending the measurements through the RabbitMQ broker using a [topic exchange](https://www.rabbitmq.com/tutorials/tutorial-five-python.html). The topic will be *&lt;name-of-the-machine&gt;.measure*.

![Picture](/img/9318034_orig.png)

The next module, [resources-analyzer](https://github.com/mendrugory/reactive-architecture-python/tree/master/resources-analyzer), receives all the measurements (topic: *#.measure*), determines the CPU usage level of the machine (*HIGH, MEDIUM, LOW*) and sends it through the RabbitMQ broker using the topic *&lt;machine's-name-of-received-measure&gt;.analysis*.

The last module, [resources-web](https://github.com/mendrugory/reactive-architecture-python/tree/master/resources-web), shows the results of the previous module. For this task I use the Tornado web framework, which is connected to RabbitMQ using the [TornadoConnection](https://pika.readthedocs.org/en/0.9.13/modules/adapters/tornado.html) of [Pika](http://pika.readthedocs.org), and to a simple AngularJS client through a [WebSocket](https://en.wikipedia.org/wiki/WebSocket).

Therefore, each time a CPU measurement is taken, the final result is shown in the browser without making any request.

<p style="text-align:center">
  <img alt="Picture" src="/img/5672711_orig.png" style="max-width:100%"/>
</p>

This approach makes the system very scalable. Everything can be built as a module that listens to the right topics and sends its result on another topic, so that it can be used by other modules. The final picture may remind us of a neural network.

<p style="text-align:center">
  <img alt="Picture" src="/img/9121422.png" style="max-width:377px;width:100%"/>
</p>

All the code can be found on [github](https://github.com/mendrugory/reactive-architecture-python). Each module contains a Dockerfile, which will help you a lot to run the entire system using [Docker](https://www.docker.com/).
