---
title: "The Easiest Distributed Producer-Consumer System"
date: 2017-05-26
url: /post/the-easiest-distributed-producer-consumer-system
---

Two months ago I published on [hex](https://hex.pm/) the first version of [Galena](https://hex.pm/packages/galena), a topic-based producer-consumer library built on top of [GenStage](https://github.com/elixir-lang/gen_stage) for [Elixir](http://elixir-lang.org/). It was initially designed to create flows of [producers](https://hexdocs.pm/galena/Galena.Producer.html), [producer-consumers](https://hexdocs.pm/galena/Galena.ProducerConsumer.html) and [consumers](https://hexdocs.pm/galena/Galena.Consumer.html) where messages have to be delivered as soon as possible. Each consumer or producer-consumer can receive messages from several producers and/or producer-consumers. In addition, consumers and producer-consumers can select the messages they want to receive, thanks to a topic-based approach.

This library was designed to be used by an application running on a single server, but why not try to distribute Galena's producers, producer-consumers and consumers across several machines?

Elixir code runs on the Erlang Virtual Machine, or BEAM, and therefore it can take advantage of all its distribution features.

Erlang provides the [global](http://erlang.org/doc/man/global.html) module, which helps us register process names globally. This means that we can interact with any globally registered process once the nodes are connected.

Taking these ideas into account, we can use Galena as a distributed producer-consumer system:

```elixir
iex(test1@machine1)> MyProducer.start_link([], [name: {:global, :producer}]

iex(test2@machine2)> MyConsumer.start_link([producers_info: [{["topic"], {:global, :producer}}]], [name: {:global, :consumer}])
```

Check out the code on [Github](https://github.com/mendrugory/galena).

Enjoy your distributed messages!!!
