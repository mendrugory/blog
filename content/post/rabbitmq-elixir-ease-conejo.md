---
title: "RabbitMQ + Elixir + Ease = Conejo"
date: 2016-12-03
url: /post/rabbitmq-elixir-ease-conejo
index: true
---

<p style="text-align:center">
  <img alt="RabbitMQ" src="/img/rabbitmq-sh-600x600.png" style="max-width:178px;width:100%"/>
</p>

I like [RabbitMQ](https://www.rabbitmq.com) a lot. It is a really good system when you have to work with events and build your systems with asynchrony in mind. Although *silver bullets* do not exist in the software world, when you need complex routing, scalability and reliability, RabbitMQ is your system.

<p style="text-align:center">
  <img alt="Elixir" src="/img/elixir_drop.png" style="max-width:163px;width:100%"/>
</p>

I usually work in a polyglot environment, but when one of the systems has to deal with a lot of connections (e.g. message queue systems, databases, web servers, ...), [Elixir](http://elixir-lang.org/) is my choice. And when I have to deal with RabbitMQ, I use a wonderful library, [pma/amqp](https://github.com/pma/amqp).

When we code several projects that use the same language and interact with the same system, we always end up writing the same code base: in this case, a connection to RabbitMQ and the different channels with their own callbacks. The only difference between the pieces of code that manage the channels is the callbacks; therefore, I decided to create an OTP application that always takes care of this common code. Its name: [Conejo](https://github.com/mendrugory/conejo) ("rabbit" in Spanish).

I began by creating a module that takes care of the RabbitMQ connection (only one TCP connection per project; if you need more "connections", use channels). This module is a [GenServer](http://elixir-lang.org/docs/stable/elixir/GenServer.html) supervised by a [Supervisor](http://elixir-lang.org/docs/stable/elixir/Supervisor).

Once I had the connection, I decided to create some Behaviours that reduce the complexity of working with RabbitMQ: Conejo.Consumer and Conejo.Publisher. Both of them are based on GenServer. If you want to use the first one, you only have to implement the function `consume(channel, tag, redelivered, payload)`, and if you use the second one, you don't need to implement any code in your module to make it work.

```elixir
defmodule MyApplication.MyConsumer do
  use Conejo.Consumer

  def consume(_channel, _tag, _redelivered, payload) do
    IO.puts "Received  -> #{inspect payload}"
  end
end

options = Application.get_all_env(:my_application)[:consumer] 

{:ok, consumer} = MyApplication.MyConsumer.start_link(options, [name: :consumer])
```

```elixir
defmodule MyApplication.MyPublisher do
  use Conejo.Publisher

end

{:ok, publisher} = MyApplication.MyPublisher.start_link([], [name: :publisher])

#Synchronous
MyApplication.MyPublisher.sync_publish(:publisher, "my_exchange", "example", "Hola")

#Asynchronous
MyApplication.MyPublisher.async_publish(:publisher, "my_exchange", "example", "Adios")
```

Beforehand, you have to define the configuration for the Conejo connection (mandatory) and for your channels (optional). Conejo follows the configuration of pma/amqp. For example:

```elixir
config :my_application, :consumer,
  exchange: "my_exchange",
  exchange_type: "topic",
  queue_name: "my_queue",
  queue_declaration_options: [{:auto_delete, true}, {:exclusive, true}],
  queue_bind_options: [routing_key: "example"],
  consume_options: [no_ack: true]


config :conejo, 
  host: "my_host",
  port: 5672,
  username: "user",
  password: "pass"
```

As you can see, it's really easy.

Add it to your project and start using it :D

```elixir
# mix.exs
def deps do
  [{:conejo, git: "https://github.com/mendrugory/conejo.git"}] # or [{:conejo, "~> 0.1.0"}] when is available
end

def application do
  [applications: [:conejo]]
end
```

Check it out on [GitHub](https://github.com/mendrugory/conejo).
