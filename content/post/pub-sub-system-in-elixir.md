---
title: "Pub-Sub System in Elixir"
date: 2016-05-16
url: /post/pub-sub-system-in-elixir
index: true
---

<div class="blog-content">
<br><br>
 <span class="imgPusher" style="float:left;height:0px">
 </span>
 <span style="display: table;width:189px;position:relative;float:left;max-width:100%;;clear:left;margin-top:19px;*margin-top:38px">
 <a>
  <img alt="Picture" class="galleryImageBorder wsite-image" src="/img/elixir_drop.png" style="margin-top: 5px; margin-bottom: 10px; margin-left: 0px; margin-right: 10px; none; max-width:100%"/>
 </a>
 <span class="wsite-caption" style="display: table-caption; caption-side: bottom; font-size: 90%; margin-top: -10px; margin-bottom: 10px; text-align: center;">
 </span>
 </span>
 <div class="paragraph" style="text-align:justify;display:block;">
  Elixir is a really awesome language which runs in the Erlang Virtual Machine (
 <em>
   BEAM
 </em>
  ). So it takes advantage of all the concurrent and distribution features of Erlang by free.
 <br>
   In order to demonstrate it I have decided to develop a proof of concept (PoC) of a Publisher-Subscriber system only using Elixir.
  <br>
  </br>
 </br>
 </div>
 <hr style="width:100%;clear:both;visibility:hidden;">
 <div>
  <div style="height: 20px; overflow: hidden; width: 100%;">
  </div>
  <hr class="styled-hr" style="width:100%;">
   <div style="height: 20px; overflow: hidden; width: 100%;">
   </div>
  </hr>
 </div>
 <h2 class="wsite-content-title" style="text-align:left;">
   Coding
 </h2>
 <div class="paragraph" style="text-align:left;">
   I will begin building a Publisher. This module will be based on
  <a href="http://elixir-lang.org/docs/stable/elixir/GenServer.html" target="_blank">
    GenServer
  </a>
   . It will take care of subscriptions, unsubscriptions and notifications. But we have to remember that Elixir as pure functional programming language does not maintain a state, how will be able to know who is subscribed?
  <br>
   <br>
     Elixir provides a module called
    <a href="http://elixir-lang.org/docs/stable/elixir/Agent.html" target="_blank">
      Agent
    </a>
     which is prepared for this kind of issues. I will create a SubscriptionManager based on Agent in order to track all the subscriptions.
    <br>
    </br>
   </br>
  </br>
 </div>
 <div>
  <div align="left" class="wcustomhtml" id="358753411713350102" style="width: 100%; overflow-y: hidden;">
   <!-- HTML generated using hilite.me -->
   <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
    <pre style="margin: 0; line-height: 125%">
<span style="color: #008800; font-weight: bold">defmodule</span> <span style="color: #003366; font-weight: bold">SubscriptionManager</span> <span style="color: #008800; font-weight: bold">do</span>

<span style="color: #008800; font-weight: bold">  def</span> start_link <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span><span style="color: #003366; font-weight: bold">Agent</span><span style="color: #333333">.</span>start_link(<span style="color: #008800; font-weight: bold">fn</span> <span style="color: #333333">-&gt;</span> <span style="color: #FF0000; background-color: #FFAAAA">%</span>{} <span style="color: #008800; font-weight: bold">end</span>)
 <span style="color: #008800; font-weight: bold">end</span>

 <span style="color: #008800; font-weight: bold">def</span> start <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span><span style="color: #003366; font-weight: bold">Agent</span><span style="color: #333333">.</span>start(<span style="color: #008800; font-weight: bold">fn</span> <span style="color: #333333">-&gt;</span> <span style="color: #FF0000; background-color: #FFAAAA">%</span>{} <span style="color: #008800; font-weight: bold">end</span>)
 <span style="color: #008800; font-weight: bold">end</span>

 <span style="color: #008800; font-weight: bold">def</span> get_all(agent) <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span><span style="color: #003366; font-weight: bold">Agent</span><span style="color: #333333">.</span>get(agent, <span style="color: #008800; font-weight: bold">fn</span> x <span style="color: #333333">-&gt;</span> x <span style="color: #008800; font-weight: bold">end</span>)
 <span style="color: #008800; font-weight: bold">end</span>

 <span style="color: #008800; font-weight: bold">def</span> get(agent, key) <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span><span style="color: #003366; font-weight: bold">Agent</span><span style="color: #333333">.</span>get(agent, <span style="color: #008800; font-weight: bold">fn</span> x <span style="color: #333333">-&gt;</span> <span style="color: #003366; font-weight: bold">Map</span><span style="color: #333333">.</span>get(x, key) <span style="color: #008800; font-weight: bold">end</span>)
 <span style="color: #008800; font-weight: bold">end</span>

 <span style="color: #008800; font-weight: bold">def</span> put(agent, key, value) <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span><span style="color: #003366; font-weight: bold">Agent</span><span style="color: #333333">.</span>update(agent, <span style="color: #008800; font-weight: bold">fn</span> x <span style="color: #333333">-&gt;</span> <span style="color: #003366; font-weight: bold">Map</span><span style="color: #333333">.</span>put(x, key, value) <span style="color: #008800; font-weight: bold">end</span>)
 <span style="color: #008800; font-weight: bold">end</span>

 <span style="color: #008800; font-weight: bold">def</span> delete(agent, key) <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span><span style="color: #003366; font-weight: bold">Agent</span><span style="color: #333333">.</span>get_and_update(agent, <span style="color: #008800; font-weight: bold">fn</span> x <span style="color: #333333">-&gt;</span> <span style="color: #003366; font-weight: bold">Map</span><span style="color: #333333">.</span>pop(x, key) <span style="color: #008800; font-weight: bold">end</span>)
 <span style="color: #008800; font-weight: bold">end</span>

<span style="color: #008800; font-weight: bold">end</span>
</pre>
   </div>
  </div>
 </div>
 <div class="paragraph" style="text-align:left;">
   We can also improve this module using
  <a href="http://elixir-lang.org/getting-started/mix-otp/ets.html" target="_blank">
    ETS
  </a>
   but let's do it other time ;).
  <br>
  </br>
 </div>
 <div class="paragraph" style="text-align:left;">
   Once that we have a module on charge of the subscriptions, we can develop the Publisher module:
  <br>
  </br>
 </div>
 <div>
  <div align="left" class="wcustomhtml" id="437533440522221613" style="width: 100%; overflow-y: hidden;">
   <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
    <pre style="margin: 0; line-height: 125%">
<span style="color: #008800; font-weight: bold">defmodule</span> <span style="color: #003366; font-weight: bold">Publisher</span> <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">use</span> <span style="color: #003366; font-weight: bold">GenServer</span>

 <span style="color: #888888"># Server</span>
 <span style="color: #008800; font-weight: bold">def</span> start <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span>{<span style="color: #AA6600">:ok</span>, subscription_manager} <span style="color: #333333">=</span> <span style="color: #003366; font-weight: bold">SubscriptionManager</span><span style="color: #333333">.</span>start
   <span style="color: #003366; font-weight: bold">GenServer</span><span style="color: #333333">.</span>start(<span style="color: #003366; font-weight: bold">Publisher</span>, subscription_manager)
 <span style="color: #008800; font-weight: bold">end</span>

 <span style="color: #008800; font-weight: bold">def</span> handle_cast({<span style="color: #AA6600">:subscribe</span>, name, pid}, subscription_manager) <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span><span style="color: #003366; font-weight: bold">SubscriptionManager</span><span style="color: #333333">.</span>put(subscription_manager, name, pid)
    {<span style="color: #AA6600">:noreply</span>, subscription_manager}
 <span style="color: #008800; font-weight: bold">end</span>

 <span style="color: #008800; font-weight: bold">def</span> handle_cast({<span style="color: #AA6600">:unsubscribe</span>, name}, subscription_manager) <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span><span style="color: #003366; font-weight: bold">SubscriptionManager</span><span style="color: #333333">.</span>delete(subscription_manager, name)
    {<span style="color: #AA6600">:noreply</span>, subscription_manager}
 <span style="color: #008800; font-weight: bold">end</span>

 <span style="color: #008800; font-weight: bold">def</span> handle_cast({<span style="color: #AA6600">:notify</span>, message}, subscription_manager) <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span><span style="color: #003366; font-weight: bold">SubscriptionManager</span><span style="color: #333333">.</span>get_all(subscription_manager) <span style="color: #333333">|&gt;</span>
   <span style="color: #003366; font-weight: bold">Enum</span><span style="color: #333333">.</span>map(<span style="color: #008800; font-weight: bold">fn</span> {key, value} <span style="color: #333333">-&gt;</span> <span style="color: #003366; font-weight: bold">GenEvent</span><span style="color: #333333">.</span>notify(value, message) <span style="color: #008800; font-weight: bold">end</span>)
    {<span style="color: #AA6600">:noreply</span>, subscription_manager}
 <span style="color: #008800; font-weight: bold">end</span>

<span style="color: #888888"># Client</span>
 <span style="color: #008800; font-weight: bold">def</span> subscribe(server, name, pid) <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span><span style="color: #003366; font-weight: bold">GenServer</span><span style="color: #333333">.</span>cast(server, {<span style="color: #AA6600">:subscribe</span>, name, pid})
 <span style="color: #008800; font-weight: bold">end</span>

 <span style="color: #008800; font-weight: bold">def</span> unsubscribe(server, name) <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span><span style="color: #003366; font-weight: bold">GenServer</span><span style="color: #333333">.</span>cast(server, {<span style="color: #AA6600">:unsubscribe</span>, name})
 <span style="color: #008800; font-weight: bold">end</span>

 <span style="color: #008800; font-weight: bold">def</span> notify(server, message) <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">    </span><span style="color: #003366; font-weight: bold">GenServer</span><span style="color: #333333">.</span>cast(server, {<span style="color: #AA6600">:notify</span>, message})
    <span style="color: #003366; font-weight: bold">IO</span><span style="color: #333333">.</span>puts <span style="background-color: #fff0f0">"I have published: '</span><span style="background-color: #eeeeee">#{</span>message<span style="background-color: #eeeeee">}</span><span style="background-color: #fff0f0">'"</span>
 <span style="color: #008800; font-weight: bold">end</span>

<span style="color: #008800; font-weight: bold">end</span>
</pre>
   </div>
  </div>
 </div>
 <div class="paragraph" style="text-align:left;">
   Then, we only need to develop our Subscriber module. It will be based on
  <a href="http://elixir-lang.org/docs/stable/elixir/GenEvent.html" target="_blank">
    GenEvent
  </a>
   and it will handle the received events in the easier way that we know:
  <em>
    Printing
  </em>
   .
  <br>
  </br>
 </div>
 <div>
  <div align="left" class="wcustomhtml" id="975002537314175764" style="width: 100%; overflow-y: hidden;">
   <!-- HTML generated using hilite.me -->
   <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
    <pre style="margin: 0; line-height: 125%">
<span style="color: #008800; font-weight: bold">defmodule</span> <span style="color: #003366; font-weight: bold">Subscriber</span> <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">use</span> <span style="color: #003366; font-weight: bold">GenEvent</span>

 <span style="color: #008800; font-weight: bold">def</span> start <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span>{<span style="color: #AA6600">:ok</span>, pid} <span style="color: #333333">=</span> <span style="color: #003366; font-weight: bold">GenEvent</span><span style="color: #333333">.</span>start([])
   <span style="color: #003366; font-weight: bold">GenEvent</span><span style="color: #333333">.</span>add_handler(pid, <span style="color: #003366; font-weight: bold">Subscriber</span>, [])
    {<span style="color: #AA6600">:ok</span>, pid}
 <span style="color: #008800; font-weight: bold">end</span>

 <span style="color: #008800; font-weight: bold">def</span> handle_event(message, state) <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span><span style="color: #003366; font-weight: bold">IO</span><span style="color: #333333">.</span>puts <span style="background-color: #fff0f0">"I have received: '</span><span style="background-color: #eeeeee">#{</span>message<span style="background-color: #eeeeee">}</span><span style="background-color: #fff0f0">'"</span>
    {<span style="color: #AA6600">:ok</span>, state}
 <span style="color: #008800; font-weight: bold">end</span>
<span style="color: #008800; font-weight: bold">end</span>
</pre>
   </div>
  </div>
 </div>
 <div>
  <div style="height: 20px; overflow: hidden; width: 100%;">
  </div>
  <hr class="styled-hr" style="width:100%;">
   <div style="height: 20px; overflow: hidden; width: 100%;">
   </div>
  </hr>
 </div>
 <h2 class="wsite-content-title" style="text-align:left;">
   Testing
  <br/>
 </h2>
 <div class="paragraph" style="text-align:left;">
   Erlang, and by extension Elixir, has the possibility of connecting Erlang Virtual Machines. I will use this capability within the same host but it will work with hosts that see each others.
  <br>
   <br>
     I will run three BEAMs, one for the publisher and two for the subscribers.
    <br>
    </br>
   </br>
  </br>
 </div>
 <div class="paragraph" style="text-align:left;">
  <strong>
    Node 1: The Publisher
  </strong>
  <br>
  </br>
 </div>
 <div class="paragraph" style="text-align:left;">
   Open a console and type:
  <br>
  </br>
 </div>
 <div>
  <div align="left" class="wcustomhtml" id="355358990745923273" style="width: 100%; overflow-y: hidden;">
   <!-- HTML generated using hilite.me -->
   <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
    <pre style="margin: 0; line-height: 125%">
<span style="color: #996633">$ </span>iex --sname node1 --cookie pubsub -S mix
</pre>
   </div>
  </div>
 </div>
 <div class="paragraph" style="text-align:left;">
   Now that we have an IEX running type the following:
  <br>
  </br>
 </div>
 <div>
  <div align="left" class="wcustomhtml" id="139129628635946027" style="width: 100%; overflow-y: hidden;">
   <!-- HTML generated using hilite.me -->
   <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
    <pre style="margin: 0; line-height: 125%">
iex<span style="color: #333333">(</span>node1@host<span style="color: #333333">)</span>&gt; <span style="color: #333333">{</span>:ok, manager<span style="color: #333333">}</span> <span style="color: #333333">=</span> Publisher.start
iex<span style="color: #333333">(</span>node1@host<span style="color: #333333">)</span>&gt; :global.register_name<span style="color: #333333">(</span><span style="background-color: #fff0f0">'manager'</span>, manager<span style="color: #333333">)</span>
</pre>
   </div>
  </div>
 </div>
 <div class="paragraph" style="text-align:left;">
   I am registering the Publisher process globally using the name "manager" therefore any process from any connected BEAM will be able to find it.
  <br>
  </br>
 </div>
 <div class="paragraph" style="text-align:left;">
  <strong>
    Node 2: The Subscriber
  </strong>
  <br>
  </br>
 </div>
 <div class="paragraph" style="text-align:left;">
   Open another console and type:
  <br>
  </br>
 </div>
 <div>
  <div align="left" class="wcustomhtml" id="853317404307165552" style="width: 100%; overflow-y: hidden;">
   <!-- HTML generated using hilite.me -->
   <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
    <pre style="margin: 0; line-height: 125%">
iex --sname node2 --cookie pubsub -S mix
</pre>
   </div>
  </div>
 </div>
 <div class="paragraph" style="text-align:left;">
   Now that we have an IEX running type the following:
  <br>
  </br>
 </div>
 <div>
  <div align="left" class="wcustomhtml" id="999236547552034311" style="width: 100%; overflow-y: hidden;">
   <!-- HTML generated using hilite.me -->
   <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
    <pre style="margin: 0; line-height: 125%">
iex<span style="color: #333333">(</span>node2@host<span style="color: #333333">)</span>&gt; Node.connect :<span style="background-color: #fff0f0">'node1@host'</span>
iex<span style="color: #333333">(</span>node2@host<span style="color: #333333">)</span>&gt; <span style="color: #996633">manager</span> <span style="color: #333333">=</span> :global.whereis_name<span style="color: #333333">(</span><span style="background-color: #fff0f0">'manager'</span><span style="color: #333333">)</span>
iex<span style="color: #333333">(</span>node2@host<span style="color: #333333">)</span>&gt; <span style="color: #333333">{</span>:ok, subscriber<span style="color: #333333">}</span> <span style="color: #333333">=</span> Subscriber.start
iex<span style="color: #333333">(</span>node2@host<span style="color: #333333">)</span>&gt; Publisher.subscribe<span style="color: #333333">(</span>manager, <span style="background-color: #fff0f0">'subs_node2'</span>, subscriber<span style="color: #333333">)</span>
</pre>
   </div>
  </div>
 </div>
 <div class="paragraph" style="text-align:left;">
   Firstly we have to connect to the node1 and then using the :global module this process is able to subscribe to the Publisher that can be even in other host.
  <br>
  </br>
 </div>
 <div class="paragraph" style="text-align:left;">
  <strong>
    Node 3: The Subscriber
  </strong>
  <br>
  </br>
 </div>
 <div>
  <div align="left" class="wcustomhtml" id="830534840556727732" style="width: 100%; overflow-y: hidden;">
   <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
    <pre style="margin: 0; line-height: 125%">
iex --sname node3 --cookie pubsub -S mix
</pre>
   </div>
  </div>
 </div>
 <div class="paragraph" style="text-align:left;">
   And type in the IEX:
  <br>
  </br>
 </div>
 <div>
  <div align="left" class="wcustomhtml" id="650106167787792674" style="width: 100%; overflow-y: hidden;">
   <!-- HTML generated using hilite.me -->
   <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
    <pre style="margin: 0; line-height: 125%">
iex<span style="color: #333333">(</span>node3@host<span style="color: #333333">)</span>&gt; Node.connect :<span style="background-color: #fff0f0">'node1@host'</span>
iex<span style="color: #333333">(</span>node3@host<span style="color: #333333">)</span>&gt; <span style="color: #996633">manager</span> <span style="color: #333333">=</span> :global.whereis_name<span style="color: #333333">(</span><span style="background-color: #fff0f0">'manager'</span><span style="color: #333333">)</span>
iex<span style="color: #333333">(</span>node3@host<span style="color: #333333">)</span>&gt; <span style="color: #333333">{</span>:ok, subscriber<span style="color: #333333">}</span> <span style="color: #333333">=</span> Subscriber.start
iex<span style="color: #333333">(</span>node3@host<span style="color: #333333">)</span>&gt; Publisher.subscribe<span style="color: #333333">(</span>manager, <span style="background-color: #fff0f0">'subs_node3'</span>, subscriber<span style="color: #333333">)</span>
</pre>
   </div>
  </div>
 </div>
 <div class="paragraph" style="text-align:left;">
  <strong>
    Publish a message:
   <br/>
  </strong>
   Using whatever IEX (node1, node2 or node3) type the following:
  <br>
  </br>
 </div>
 <div>
  <div align="left" class="wcustomhtml" id="212980223300545283" style="width: 100%; overflow-y: hidden;">
   <!-- HTML generated using hilite.me -->
   <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
    <pre style="margin: 0; line-height: 125%">
iex&gt; Publisher.notify<span style="color: #333333">(</span>manager, <span style="background-color: #fff0f0">"Hello !!"</span><span style="color: #333333">)</span>
</pre>
   </div>
  </div>
 </div>
 <div class="paragraph" style="text-align:left;">
   The two subscriber will receive the sent message, "Hello !!" in this case.
  <br>
  </br>
 </div>
 <div>
  <div class="wsite-image wsite-image-border-none" style="padding-top:10px;padding-bottom:10px;margin-left:0;margin-right:0;text-align:center">
   <a>
    <img alt="Picture" src="/img/1449276_orig.png" style="width:auto;max-width:100%"/>
   </a>
   <div style="display:block;font-size:90%">
   </div>
  </div>
 </div>
 <div>
  <div style="height: 20px; overflow: hidden; width: 100%;">
  </div>
  <hr class="styled-hr" style="width:100%;">
   <div style="height: 20px; overflow: hidden; width: 100%;">
   </div>
  </hr>
 </div>
 <h2 class="wsite-content-title" style="text-align:left;">
   Conclusion
  <br/>
 </h2>
 <div class="paragraph" style="text-align:left;">
   As you have seen, it is really easy to code a distributed system using Elixir.
  <br>
   <br>
     You can find the code in my
    <a href="https://github.com/mendrugory/pubsubex/" target="_blank">
      github
    </a>
     .
    <br>
     <br>
     </br>
    </br>
   </br>
  </br>
 </div>
 <div class="wsite-adsense">
  <script src="//www.weebly.com/weebly/apps/serveAds.php?type=adsense&amp;elementid=563146882365193592&amp;ineditor=0&amp;subdomain=mendrugory.weebly.com&amp;pubid=pub-2477529535053270&amp;adformat=468x60&amp;adtype=text_image&amp;bordercolor=FFFFFF&amp;bgcolor=FFFFFF&amp;linkcolor=0F53FF&amp;textcolor=000000&amp;urlcolor=008000" type="text/javascript">
  </script>
 </div>
 </hr>
</div>
