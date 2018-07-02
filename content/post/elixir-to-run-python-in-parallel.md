---
title: "Elixir to Run Python in Parallel"
date: 2017-06-01
url: /post/elixir-to-run-python-in-parallel
---

<div class="blog-content">
 <div class="paragraph">
  Data Scientist is considered the sexiest job around the world, at least in the tech world, and it is true that sometimes the results that you can get applying Machine Learning techniques seem to be sorcery. Therefore a lot of people are trying to become data scientist.
 <br>
  <br>
    A lot of them use Python and almost all the on-line courses use also Python to teach the concepts.
   <br>
   </br>
  </br>
 </br>
 </div>
 <div class="paragraph">
  Python is a lovely programming language. It is so easy to get into and the final code is, sometimes, even beautiful. But all these things mean nothing when you face the
 <a href="https://en.wikipedia.org/wiki/Global_interpreter_lock" target="_blank">
   GIL
 </a>
  . There some solutions like
 <a href="https://docs.python.org/3.6/library/multiprocessing.html" target="_blank">
   multiprocessing
 </a>
  . It is good when you can split the data in big chunks. Other approaches are related to apply horizontal scaling techniques in order to get a vertical scaling, what does it mean? If you have read about the GIL you already know that Python will only take advantage of one CPU core of your PC so, why don't we launch more Pythons and let's split the load between them using load balancing, messaging systems, and so on?
 <br>
   You can see that the first example is prepared to scale-in and the second one to scale-out. But all of them seem to be too much for a guy who only wants to develop a "Python code" so let's back to the data science world.
  <br>
  </br>
 </br>
 </div>
 <div class="paragraph">
  Typing an algorithm in Python is a pleasure and the ecosystem is great. Libraries like
 <a href="http://www.numpy.org" target="_blank">
   numpy
 </a>
  or
 <a href="https://www.scipy.org" target="_blank">
   scipy
 </a>
  will help you a lot. But you can also find higher-level libraries like
 <a href="http://scikit-learn.org" target="_blank">
   scikit-learn
 </a>
  or
 <a href="https://www.tensorflow.org" target="_blank">
   tensorflow
 </a>
  . So, how am I not going to use Python for Machine Learning?
 <br>
 </br>
 </div>
 <div class="paragraph">
  Some weeks ago I spend some time watching videos and presentations about Erlang and I remembered two things:
 <ul>
  <li>
    Francesco Cesarini, Founder &amp; Technical Director of
   <a href="http://erlang-solutions.com/" target="_blank">
     Erlang Solutions
   </a>
    , defined Erlang as an orchestration language.
  </li>
  <li>
   <a href="https://en.wikipedia.org/wiki/Demonware" target="_blank">
     Demonware
   </a>
    , the company behind the infrastructure for Call of Duty, uses Erlang to handle connections, tasks and especially, to control Python (
   <a href="https://www.erlang-factory.com/upload/presentations/395/ErlangandFirst-PersonShooters.pdf" target="_blank">
     slides
   </a>
    ).
   <br>
   </br>
  </li>
 </ul>
 <br>
   Why don't I try to handle my Python code using Elixir? In this way I will be able to scale and specially to add fault tolerance.
 </br>
 </div>
 <div class="paragraph">
  With these things in mi mind I began to code
 <a href="https://github.com/mendrugory/piton/" target="_blank">
   Piton
 </a>
  which is a library that uses
 <a href="http://erlang.org/doc/reference_manual/ports.html" target="_blank">
   Erlang Ports
 </a>
  , thanks to
 <a href="http://erlport.org/" target="_blank">
   ErlPort
 </a>
  , to directly communicate Elixir and Python.
 <br>
  <br>
    The first step is to have a Python project. I am going to use a simple example of a Fibonnaci calculator.
   <br>
   </br>
  </br>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="717846230400282701" style="width: 100%; overflow-y: hidden;">
  <!-- HTML generated using hilite.me -->
  <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
   <pre style="margin: 0; line-height: 125%">
<span style="color: #008800; font-weight: bold">def</span> <span style="color: #0066BB; font-weight: bold">fib</span>(n):
   <span style="color: #008800; font-weight: bold">if</span> n <span style="color: #333333">&lt;</span> <span style="color: #0000DD; font-weight: bold">0</span>: <span style="color: #008800; font-weight: bold">raise</span> <span style="color: #FF0000; font-weight: bold">Exception</span>(<span style="background-color: #fff0f0">"No negative values !!!"</span>)
   <span style="color: #008800; font-weight: bold">if</span> n <span style="color: #333333">==</span> <span style="color: #0000DD; font-weight: bold">0</span>: <span style="color: #008800; font-weight: bold">return</span> <span style="color: #0000DD; font-weight: bold">0</span>
   <span style="color: #008800; font-weight: bold">if</span> n <span style="color: #333333">&lt;</span> <span style="color: #0000DD; font-weight: bold">3</span>: <span style="color: #008800; font-weight: bold">return</span> <span style="color: #0000DD; font-weight: bold">1</span>
   <span style="color: #008800; font-weight: bold">return</span> fib(n <span style="color: #333333">-</span> <span style="color: #0000DD; font-weight: bold">1</span>) <span style="color: #333333">+</span> fib(n <span style="color: #333333">-</span> <span style="color: #0000DD; font-weight: bold">2</span>)
</pre>
  </div>
 </div>
 </div>
 <div class="paragraph">
  Then, create the module of your own Port using Piton.Port:
 <br>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="906750156870079196" style="width: 100%; overflow-y: hidden;">
  <!-- HTML generated using hilite.me -->
  <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
   <pre style="margin: 0; line-height: 125%">
<span style="color: #008800; font-weight: bold">defmodule</span> <span style="color: #003366; font-weight: bold">MyPoolPort</span> <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">  use</span> <span style="color: #003366; font-weight: bold">Piton</span><span style="color: #333333">.</span><span style="color: #003366; font-weight: bold">Port</span>
 <span style="color: #008800; font-weight: bold">def</span> start(), <span style="color: #008800; font-weight: bold">do</span>: <span style="color: #003366; font-weight: bold">MyPoolPort</span><span style="color: #333333">.</span>start([<span style="color: #AA6600">path:</span> <span style="color: #003366; font-weight: bold">Path</span><span style="color: #333333">.</span>expand(<span style="background-color: #fff0f0">"python_folder"</span>), <span style="color: #AA6600">python:</span> <span style="background-color: #fff0f0">"python"</span>], [])
 <span style="color: #008800; font-weight: bold">def</span> fun(pid, n), <span style="color: #008800; font-weight: bold">do</span>: <span style="color: #003366; font-weight: bold">MyPoolPort</span><span style="color: #333333">.</span>execute(pid, <span style="color: #AA6600">:functions</span>, <span style="color: #AA6600">:fun</span>, [n])
<span style="color: #008800; font-weight: bold">end</span>
</pre>
  </div>
 </div>
 </div>
 <div class="paragraph">
  It is mandatory to have a start() function which provides the path to the python project and the python interpreter which could belong to a virtual environment. Then you can define as many function as you need. I recommend to create some wrappers for the  execute() function which only needs the
 <em>
   pid
 </em>
  of the process which is connected to one Python, the atom of the python module, the atom of the python function and a list of arguments for the python function.
 <br>
  <br>
    Now we only have to launch our Piton.Pool, indicating which module is going to use and the number Pythons we want to run, and use it:
   <br>
   </br>
  </br>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="988124758396821656" style="width: 100%; overflow-y: hidden;">
  <!-- HTML generated using hilite.me -->
  <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
   <pre style="margin: 0; line-height: 125%">
iex<span style="color: #333333">&gt;</span> {:ok, pool} <span style="color: #333333">=</span> Piton<span style="color: #333333">.</span>Pool<span style="color: #333333">.</span>start_link([module: MyPoolPort, pool_number: <span style="color: #0000DD; font-weight: bold">2</span>], [])
{:ok, <span style="color: #888888">#PID&lt;0.176.0&gt;}</span>

iex<span style="color: #333333">&gt;</span> Piton<span style="color: #333333">.</span>Pool<span style="color: #333333">.</span>execute(pool, :fib, [<span style="color: #0000DD; font-weight: bold">20</span>])
<span style="color: #0000DD; font-weight: bold">6765</span>
</pre>
  </div>
 </div>
 </div>
 <div class="paragraph">
  Thanks to Elixir, although your python code raises exceptions, the Piton.Poll will always have the indicated number of Python interpreters ready.
 <br>
  <br>
    You can check the
   <a href="https://github.com/mendrugory/piton" target="_blank">
     github
   </a>
    , the
   <a href="https://hexdocs.pm/piton/" target="_blank">
     docs
   </a>
    and the
   <a href="https://hex.pm/packages/piton" target="_blank">
     hex
   </a>
    .
   <br>
    <br>
      Python loves Elixir !!
     <br>
     </br>
    </br>
   </br>
  </br>
 </br>
 </div>
</div>
