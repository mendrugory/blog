---
title: "Testing Elixir Concurrent Apps"
date: 2017-03-30
url: /post/testing-elixir-concurrent-apps
index: true
---

This week I released my first two contributions to the Elixir open source community on [hex](http://hex.pm/): **Galena** and **Conejo**.

[**Galena**](https://hex.pm/packages/galena) is a library based on [GenStage](https://hex.pm/packages/gen_stage) that will help you create topic-based producer-consumer data pipelines.

[**Conejo**](https://hex.pm/packages/conejo) is an OTP application/library that makes it easy to create publishers and consumers for [RabbitMQ](https://www.rabbitmq.com/) or other AMQP servers. It is based on the [amqp](https://github.com/pma/amqp) library.

I was very surprised that most of the questions I have received are about the tests:

*"How do you test the libraries if they provide Behaviours to develop concurrent apps?"*

Let's see how.

## Testing

Elixir has a really good unit test framework, [ExUnit](https://hexdocs.pm/ex_unit/ExUnit.html), which is very easy to use. However, we are talking about concurrent apps, where most of the logic is executed in different processes (because we want to scale, and Elixir lets us do it), and ExUnit will only pass or fail a test based on assertions made in the main test process.

As we are using Elixir, we have the Actor Model, so we can send messages between processes. Let's see how to use that for testing.

**Register the main process**

The first step is to register the process where the test is running:

```elixir
test "receive message" do
  Process.register(self(), :main_process)
...
```

**Send the message with the result to the main process**

The callback that handles the message in the secondary process has to send the result to the main process.

```elixir
...
def handle_message(message) do
  result = do_work(message)
  send(:main_process, result)
  ...
end
...
```

**Check the result in the main process**

Finally, we have to check the result in the main process. Sometimes we are only interested in knowing that the message was processed by the secondary process and sent to the main one:

```elixir
# Main Process
...
Logger.info("Waiting for the message ...")
assert_receive(message, timeout)
```

But in other cases we also want to check that the secondary process performed its transformation correctly (assuming that the message is a tuple with a topic and a result):

```elixir
# Main Process
...
receive do
  {topic, result} ->
    assert topic == expected_topic
    assert result == expected_result
  msg ->
    assert false
end
```

This is only an introduction with some simple examples, but you can do a lot with this approach.

You can check out the tests of [Conejo](https://github.com/mendrugory/conejo) and [Galena](https://github.com/mendrugory/galena).
