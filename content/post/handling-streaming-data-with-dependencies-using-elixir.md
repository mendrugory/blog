---
title: "Handling Streaming Data with Dependencies using Elixir"
date: 2017-04-10
url: /post/handling-streaming-data-with-dependencies-using-elixir
---

***Life is a succession of asynchronous events which could depend on each other.***

Some weeks ago my friends and I wanted to play a football match, so we booked a football pitch at a sports centre for the following Saturday at 5pm. That Saturday, I arrived at the sports centre at 16:44, and some of the other guys were already there; they had arrived between 16:31 and 16:42. Five minutes later more people arrived. At 16:50 only three people were missing: we were 19, and we needed 22 to enjoy a proper football match. We called them, but we only reached one of them, and he arrived 3 minutes later. At 17:00 we were 20 people, and we decided to wait 5 more minutes before starting. The others did not arrive, so we began to play with teams of 10.

As you can see, the football match is an event that depends on other events, like booking the football pitch or people arriving. When the event has to be processed, you check all its dependencies; if they are not all there, you can wait or try to get them. Finally, you can perform the event, cancel it or adapt it.

* * *

## Streaming Data

Lately I have been dealing with a new problem: working with a stream of data where the received messages can be related to each other. When I work with a stream of data and my chosen language is Elixir, I try to take advantage of the concurrency offered by the Erlang Virtual Machine. That approach helps me parallelize as much as possible, improving performance, but it also introduces other challenges. For instance, the data is not ordered, so processing messages in concurrent flows can make things harder.

Meanwhile, I was reading [Designing for Scalability with Erlang/OTP](https://www.amazon.es/Designing-Scalability-Erlang-OTP-Fault-Tolerant/dp/1449320732), which explains how synchronous and asynchronous calls to a gen_server are implemented. It gave me an idea. If I am going to process each received event in a different process, why not check/request its dependencies and have the process wait for a message with:

- The *dependency*: when the received event needs its dependencies in order to be processed.
- A *reference to the dependency*: when the received event only needs to know that the dependencies are there in order to be processed later.
- (Optional but recommended) A *timeout message* when the dependency does not arrive. In this case you decide whether to wait longer, request the dependency (if possible), continue processing without the dependency (if possible), discard the event, ...

## Conductor for our orchestra of processes

[Barenboim](https://hex.pm/packages/barenboim) is an OTP application that will help you with this task.

In the process where the received event is being processed, define how to check/request your dependency and call the get_data() function of the Barenboim module.

```elixir
fun = fn(dependency_ref) -> MyDataModule.get(dependency_ref) end
{:ok, data} = Barenboim.get_data(dependency_ref, fun)
```

The check will be executed in the same process, but if the dependency is not available yet, the process will wait for a message from Barenboim, which will send the dependency as soon as it is ready.

Meanwhile, another process receives the awaited event and processes it. Once this event has been processed, that process has to notify Barenboim that the event is ready.

```elixir
# Only the reference
Barenboim.notify({:reference, dependency_ref})

# Or the data
Barenboim.notify({:data, dependency_ref, dependency_data})
```

I highly recommend passing a timeout to the get_data() function; otherwise it will wait forever (which may fit your needs if you are completely sure that all the events will arrive). With a timeout, you may receive a timeout message:

```elixir
case Barenboim.get_data(dependency_ref, fun, 5_000) do
  {:ok, data} -> # go on
  {:timeout, any} -> # decide what you can do
end
```

Barenboim uses a pool of processes ([poolboy](https://github.com/devinus/poolboy)) and can be configured (check the [docs](https://hexdocs.pm/barenboim/Barenboim.html)).

Enjoy streaming!!!
