---
title: "Pub-Sub System in Elixir"
date: 2016-05-16
url: /post/pub-sub-system-in-elixir
index: true
---

<p style="text-align:center">
  <img alt="Elixir" src="/img/elixir_drop.png" style="max-width:189px;width:100%"/>
</p>

Elixir is a really awesome language that runs on the Erlang Virtual Machine (*BEAM*), so it gets all of Erlang's concurrency and distribution features for free.

To demonstrate it, I have decided to develop a proof of concept (PoC) of a Publisher-Subscriber system using only Elixir.

---

## Coding

I will begin by building a Publisher. This module will be based on [GenServer](http://elixir-lang.org/docs/stable/elixir/GenServer.html). It will take care of subscriptions, unsubscriptions and notifications. But we have to remember that Elixir, as a pure functional programming language, does not keep state, so how will it know who is subscribed?

Elixir provides a module called [Agent](http://elixir-lang.org/docs/stable/elixir/Agent.html), which is designed for exactly this kind of problem. I will create a SubscriptionManager based on Agent to keep track of all the subscriptions.

```elixir
defmodule SubscriptionManager do

  def start_link do
    Agent.start_link(fn -> %{} end)
  end

  def start do
    Agent.start(fn -> %{} end)
  end

  def get_all(agent) do
    Agent.get(agent, fn x -> x end)
  end

  def get(agent, key) do
    Agent.get(agent, fn x -> Map.get(x, key) end)
  end

  def put(agent, key, value) do
    Agent.update(agent, fn x -> Map.put(x, key, value) end)
  end

  def delete(agent, key) do
    Agent.get_and_update(agent, fn x -> Map.pop(x, key) end)
  end

end
```

We could also improve this module using [ETS](http://elixir-lang.org/getting-started/mix-otp/ets.html), but let's leave that for another time ;).

Now that we have a module in charge of the subscriptions, we can develop the Publisher module:

```elixir
defmodule Publisher do
  use GenServer

  # Server
  def start do
    {:ok, subscription_manager} = SubscriptionManager.start
    GenServer.start(Publisher, subscription_manager)
  end

  def handle_cast({:subscribe, name, pid}, subscription_manager) do
    SubscriptionManager.put(subscription_manager, name, pid)
    {:noreply, subscription_manager}
  end

  def handle_cast({:unsubscribe, name}, subscription_manager) do
    SubscriptionManager.delete(subscription_manager, name)
    {:noreply, subscription_manager}
  end

  def handle_cast({:notify, message}, subscription_manager) do
    SubscriptionManager.get_all(subscription_manager) |>
    Enum.map(fn {key, value} -> GenEvent.notify(value, message) end)
    {:noreply, subscription_manager}
  end

  # Client
  def subscribe(server, name, pid) do
    GenServer.cast(server, {:subscribe, name, pid})
  end

  def unsubscribe(server, name) do
    GenServer.cast(server, {:unsubscribe, name})
  end

  def notify(server, message) do
    GenServer.cast(server, {:notify, message})
    IO.puts "I have published: '#{message}'"
  end

end
```

Then we only need to develop our Subscriber module. It will be based on [GenEvent](http://elixir-lang.org/docs/stable/elixir/GenEvent.html), and it will handle the received events in the simplest way we know: *printing* them.

```elixir
defmodule Subscriber do
  use GenEvent

  def start do
    {:ok, pid} = GenEvent.start([])
    GenEvent.add_handler(pid, Subscriber, [])
    {:ok, pid}
  end

  def handle_event(message, state) do
    IO.puts "I have received: '#{message}'"
    {:ok, state}
  end
end
```

---

## Testing

Erlang, and by extension Elixir, can connect Erlang Virtual Machines to each other. I will use this capability on a single host, but it also works across hosts that can see each other.

I will run three BEAMs: one for the publisher and two for the subscribers.

**Node 1: The Publisher**

Open a console and type:

```bash
$ iex --sname node1 --cookie pubsub -S mix
```

Now that we have IEx running, type the following:

```elixir
iex(node1@host)> {:ok, manager} = Publisher.start
iex(node1@host)> :global.register_name('manager', manager)
```

I am registering the Publisher process globally under the name "manager", so any process on any connected BEAM will be able to find it.

**Node 2: The Subscriber**

Open another console and type:

```bash
iex --sname node2 --cookie pubsub -S mix
```

Now that we have IEx running, type the following:

```elixir
iex(node2@host)> Node.connect :'node1@host'
iex(node2@host)> manager = :global.whereis_name('manager')
iex(node2@host)> {:ok, subscriber} = Subscriber.start
iex(node2@host)> Publisher.subscribe(manager, 'subs_node2', subscriber)
```

First we have to connect to node1, and then, using the :global module, this process is able to subscribe to the Publisher, which could even be on another host.

**Node 3: The Subscriber**

```bash
iex --sname node3 --cookie pubsub -S mix
```

And type in IEx:

```elixir
iex(node3@host)> Node.connect :'node1@host'
iex(node3@host)> manager = :global.whereis_name('manager')
iex(node3@host)> {:ok, subscriber} = Subscriber.start
iex(node3@host)> Publisher.subscribe(manager, 'subs_node3', subscriber)
```

**Publish a message:**

In any of the IEx sessions (node1, node2 or node3), type the following:

```elixir
iex> Publisher.notify(manager, "Hello !!")
```

Both subscribers will receive the message, "Hello !!" in this case.

<p style="text-align:center">
  <img alt="Pub-Sub result" src="/img/1449276_orig.png" style="max-width:100%"/>
</p>

---

## Conclusion

As you have seen, it is really easy to code a distributed system using Elixir.

You can find the code on my [GitHub](https://github.com/mendrugory/pubsubex/).
