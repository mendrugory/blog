---
title: "Elixir to Run Python in Parallel"
date: 2017-06-01
url: /post/elixir-to-run-python-in-parallel
---

Data scientist is considered the sexiest job in the world, at least in the tech world, and it is true that the results you can get by applying Machine Learning techniques sometimes seem like sorcery. That is why a lot of people are trying to become data scientists.

Many of them use Python, and almost all online courses also use Python to teach the concepts.

Python is a lovely programming language. It is very easy to get into, and the resulting code is sometimes even beautiful. But none of that matters when you run into the [GIL](https://en.wikipedia.org/wiki/Global_interpreter_lock). There are some solutions, like [multiprocessing](https://docs.python.org/3.6/library/multiprocessing.html), which works well when you can split the data into big chunks. Other approaches apply horizontal scaling techniques to get vertical scaling. What does that mean? If you have read about the GIL, you already know that Python will only take advantage of one CPU core of your machine, so why not launch more Pythons and split the load between them using load balancing, messaging systems, and so on?

You can see that the first example is designed to scale up and the second one to scale out. But all of them seem like too much for someone who only wants to write "Python code", so let's go back to the data science world.

Writing an algorithm in Python is a pleasure, and the ecosystem is great. Libraries like [numpy](http://www.numpy.org) or [scipy](https://www.scipy.org) will help you a lot, and you can also find higher-level libraries like [scikit-learn](http://scikit-learn.org) or [tensorflow](https://www.tensorflow.org). So how could I not use Python for Machine Learning?

Some weeks ago I spent some time watching videos and presentations about Erlang, and I remembered two things:

- Francesco Cesarini, Founder & Technical Director of [Erlang Solutions](http://erlang-solutions.com/), described Erlang as an orchestration language.
- [Demonware](https://en.wikipedia.org/wiki/Demonware), the company behind the infrastructure for Call of Duty, uses Erlang to handle connections and tasks and, especially, to control Python ([slides](https://www.erlang-factory.com/upload/presentations/395/ErlangandFirst-PersonShooters.pdf)).

Why not try to manage my Python code with Elixir? That way I would be able to scale and, especially, add fault tolerance.

With these things in mind, I began to write [Piton](https://github.com/mendrugory/piton/), a library that uses [Erlang Ports](http://erlang.org/doc/reference_manual/ports.html), thanks to [ErlPort](http://erlport.org/), to make Elixir and Python communicate directly.

The first step is to have a Python project. I am going to use a simple example: a Fibonacci calculator.

```python
def fib(n):
    if n < 0: raise Exception("No negative values !!!")
    if n == 0: return 0
    if n < 3: return 1
    return fib(n - 1) + fib(n - 2)
```

Then, create your own Port module using Piton.Port:

```elixir
defmodule MyPoolPort do
  use Piton.Port
  def start(), do: MyPoolPort.start([path: Path.expand("python_folder"), python: "python"], [])
  def fun(pid, n), do: MyPoolPort.execute(pid, :functions, :fun, [n])
end
```

It is mandatory to have a start() function that provides the path to the Python project and the Python interpreter, which could belong to a virtual environment. Then you can define as many functions as you need. I recommend creating some wrappers for the execute() function, which only needs the *pid* of the process connected to a Python interpreter, the atom of the Python module, the atom of the Python function, and a list of arguments for the Python function.

Now we only have to launch our Piton.Pool, indicating which module it is going to use and the number of Pythons we want to run, and then use it:

```elixir
iex> {:ok, pool} = Piton.Pool.start_link([module: MyPoolPort, pool_number: 2], [])
{:ok, #PID<0.176.0>}

iex> Piton.Pool.execute(pool, :fib, [20])
6765
```

Thanks to Elixir, even if your Python code raises exceptions, the Piton.Pool will always have the specified number of Python interpreters ready.

You can check out the [github](https://github.com/mendrugory/piton) repository, the [docs](https://hexdocs.pm/piton/) and the [hex](https://hex.pm/packages/piton) package.

Python loves Elixir!!
