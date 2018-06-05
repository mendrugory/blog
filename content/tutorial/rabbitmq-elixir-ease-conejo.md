---
title: "RabbitMQ + Elixir + Ease = Conejo"
date: 2016-12-03
url: /tutorial/rabbitmq-elixir-ease-conejo
tags: ["tutorial"]
index: true
---

<div class="blog-content">
 <span class="imgPusher" style="float:left;height:0px">
 </span>
 <span style="display: table;width:178px;position:relative;float:left;max-width:100%;;clear:left;margin-top:0px;*margin-top:0px">
 <a>
  <img alt="Picture" class="galleryImageBorder wsite-image" src="/img/rabbitmq-sh-600x600.png" style="margin-top: 5px; margin-bottom: 10px; margin-left: 0px; margin-right: 10px; none; max-width:100%"/>
 </a>
 <span class="wsite-caption" style="display: table-caption; caption-side: bottom; font-size: 90%; margin-top: -10px; margin-bottom: 10px; text-align: center;">
 </span>
 </span>
 <div class="paragraph" style="display:block;">
  I like a lot
 <a href="https://www.rabbitmq.com" target="_blank">
   RabbitMQ
 </a>
  . It is a really good system when you have to work with events and build your systems with asynchrony in mind. Although
 <em>
   silver bullets
 </em>
  do not exist in the SW world, when you require a complex routing, scalability and reliability, RabbitMQ is your system.
 <br>
 </br>
 </div>
 <hr style="width:100%;clear:both;visibility:hidden;">
 <span class="imgPusher" style="float:right;height:0px">
 </span>
 <span style="display: table;width:163px;position:relative;float:right;max-width:100%;;clear:right;margin-top:8px;*margin-top:16px">
  <a>
   <img alt="Picture" class="galleryImageBorder wsite-image" src="/img/elixir_drop.png" style="margin-top: 5px; margin-bottom: 10px; margin-left: 0px; margin-right: 10px; none; max-width:100%"/>
  </a>
  <span class="wsite-caption" style="display: table-caption; caption-side: bottom; font-size: 90%; margin-top: -10px; margin-bottom: 10px; text-align: center;">
  </span>
 </span>
 <div class="paragraph" style="display:block;">
   I usually work in a polyglot environment, but when one of the systems has to deal with a lot of connections (i.e. Message Queue Systems, Databases, Web Servers, ...)
  <a href="http://elixir-lang.org/" target="_blank">
    Elixir
  </a>
   is my choice. And when I have to deal with RabbitMQ I use a wonderful library,
  <a href="https://github.com/pma/amqp" target="_blank">
    pma/amqp
  </a>
   .
  <br>
  </br>
 </div>
 <hr style="width:100%;clear:both;visibility:hidden;">
  <div class="paragraph">
    When we code several projects that involve the same language and the interaction with the same system, we always have to build the same code base. In this case, a connection to RabbitMQ and the different channels with their own callbacks. The only difference between the code which manages the channels are the callabacks; therefore I decided to create a OTP application which will always take care of this common code, the name -&gt;
   <a href="https://github.com/mendrugory/conejo" target="_blank">
     Conejo
   </a>
    (Rabbit in Spanish).
  </div>
  <div class="paragraph">
    I began creating a module which will take care of the RabbitMQ connection (only one tcp connection per project, if you have more "connections" use the channels). This module will be a
   <a href="http://elixir-lang.org/docs/stable/elixir/GenServer.html" target="_blank">
     GenServer
   </a>
    which will be supervise by a
   <a href="http://elixir-lang.org/docs/stable/elixir/Supervisor" target="_blank">
     Supervisor
   </a>
    .
   <br>
   </br>
  </div>
  <div class="paragraph">
    Once that I had the connection, I decide to create some Behaviours which will decrease the complexity of working with RabbitMQ: Conejo.Consumer and Conejo.Publisher. Both of them are based on GenServer. If you want to use the first one, you will only implement a function
   <span>
     consume
   </span>
    (
   <span>
     channel
   </span>
    ,
   <span>
     tag
   </span>
    ,
   <span>
     redelivered
   </span>
    ,
   <span>
     payload
   </span>
    ) and if you use the second one, you don't need to implement any code on your module to make it work.
   <br>
   </br>
  </div>
  <div>
   <div align="left" class="wcustomhtml" id="788386262918556766" style="width: 100%; overflow-y: hidden;">
    <!-- HTML generated using hilite.me -->
    <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
     <pre style="margin: 0; line-height: 125%">
<span style="color: #008800; font-weight: bold">defmodule</span> <span style="color: #003366; font-weight: bold">MyApplication</span><span style="color: #333333">.</span><span style="color: #003366; font-weight: bold">MyConsumer</span> <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">  use</span> <span style="color: #003366; font-weight: bold">Conejo</span><span style="color: #333333">.</span><span style="color: #003366; font-weight: bold">Consumer</span>

 <span style="color: #008800; font-weight: bold">def</span> consume(_channel, _tag, _redelivered, payload) <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span><span style="color: #003366; font-weight: bold">IO</span><span style="color: #333333">.</span>puts <span style="background-color: #fff0f0">"Received  -&gt; </span><span style="background-color: #eeeeee">#{</span>inspect payload<span style="background-color: #eeeeee">}</span><span style="background-color: #fff0f0">"</span>
 <span style="color: #008800; font-weight: bold">end</span>
<span style="color: #008800; font-weight: bold">end</span>

options <span style="color: #333333">=</span> <span style="color: #003366; font-weight: bold">Application</span><span style="color: #333333">.</span>get_all_env(<span style="color: #AA6600">:my_application</span>)[<span style="color: #AA6600">:consumer</span>] 

{<span style="color: #AA6600">:ok</span>, consumer} <span style="color: #333333">=</span> <span style="color: #003366; font-weight: bold">MyApplication</span><span style="color: #333333">.</span><span style="color: #003366; font-weight: bold">MyConsumer</span><span style="color: #333333">.</span>start_link(options, [<span style="color: #AA6600">name:</span> <span style="color: #AA6600">:consumer</span>])
</pre>
    </div>
   </div>
  </div>
  <div>
   <div align="left" class="wcustomhtml" id="446145607312726211" style="width: 100%; overflow-y: hidden;">
    <!-- HTML generated using hilite.me -->
    <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
     <pre style="margin: 0; line-height: 125%">
<span style="color: #008800; font-weight: bold">defmodule</span> <span style="color: #003366; font-weight: bold">MyApplication</span><span style="color: #333333">.</span><span style="color: #003366; font-weight: bold">MyPublisher</span> <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">  use</span> <span style="color: #003366; font-weight: bold">Conejo</span><span style="color: #333333">.</span><span style="color: #003366; font-weight: bold">Publisher</span>

<span style="color: #008800; font-weight: bold">end</span>

{<span style="color: #AA6600">:ok</span>, publisher} <span style="color: #333333">=</span> <span style="color: #003366; font-weight: bold">MyApplication</span><span style="color: #333333">.</span><span style="color: #003366; font-weight: bold">MyPublisher</span><span style="color: #333333">.</span>start_link([], [<span style="color: #AA6600">name:</span> <span style="color: #AA6600">:publisher</span>])

<span style="color: #888888">#Synchronous</span>
<span style="color: #003366; font-weight: bold">MyApplication</span><span style="color: #333333">.</span><span style="color: #003366; font-weight: bold">MyPublisher</span><span style="color: #333333">.</span>sync_publish(<span style="color: #AA6600">:publisher</span>, <span style="background-color: #fff0f0">"my_exchange"</span>, <span style="background-color: #fff0f0">"example"</span>, <span style="background-color: #fff0f0">"Hola"</span>)

<span style="color: #888888">#Asynchronous</span>
<span style="color: #003366; font-weight: bold">MyApplication</span><span style="color: #333333">.</span><span style="color: #003366; font-weight: bold">MyPublisher</span><span style="color: #333333">.</span>async_publish(<span style="color: #AA6600">:publisher</span>, <span style="background-color: #fff0f0">"my_exchange"</span>, <span style="background-color: #fff0f0">"example"</span>, <span style="background-color: #fff0f0">"Adios"</span>)
</pre>
    </div>
   </div>
  </div>
  <div class="paragraph">
    Previously, you have to define the configurations for the conejo connection (mandatory) and your channels (if you want). Conejo respects the configuration of pma/amqp. For example:
   <br>
   </br>
  </div>
  <div>
   <div align="left" class="wcustomhtml" id="891805287396577609" style="width: 100%; overflow-y: hidden;">
    <!-- HTML generated using hilite.me -->
    <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
     <pre style="margin: 0; line-height: 125%">
config <span style="color: #AA6600">:my_application</span>, <span style="color: #AA6600">:consumer</span>,
 <span style="color: #AA6600">exchange:</span> <span style="background-color: #fff0f0">"my_exchange"</span>,
 <span style="color: #AA6600">exchange_type:</span> <span style="background-color: #fff0f0">"topic"</span>,
 <span style="color: #AA6600">queue_name:</span> <span style="background-color: #fff0f0">"my_queue"</span>,
 <span style="color: #AA6600">queue_declaration_options:</span> [{<span style="color: #AA6600">:auto_delete</span>, <span style="color: #003366; font-weight: bold">true</span>}, {<span style="color: #AA6600">:exclusive</span>, <span style="color: #003366; font-weight: bold">true</span>}],
 <span style="color: #AA6600">queue_bind_options:</span> [<span style="color: #AA6600">routing_key:</span> <span style="background-color: #fff0f0">"example"</span>],
 <span style="color: #AA6600">consume_options:</span> [<span style="color: #AA6600">no_ack:</span> <span style="color: #003366; font-weight: bold">true</span>]


config <span style="color: #AA6600">:conejo</span>, 
 <span style="color: #AA6600">host:</span> <span style="background-color: #fff0f0">"my_host"</span>,
 <span style="color: #AA6600">port:</span> <span style="color: #6600EE; font-weight: bold">5672</span>,
 <span style="color: #AA6600">username:</span> <span style="background-color: #fff0f0">"user"</span>,
 <span style="color: #AA6600">password:</span> <span style="background-color: #fff0f0">"pass"</span>
</pre>
    </div>
   </div>
  </div>
  <div class="paragraph">
    As you can see, really easy.
   <br>
   </br>
  </div>
  <div class="paragraph">
    Add to your project and begin to use it :D
   <br>
   </br>
  </div>
  <div>
   <div align="left" class="wcustomhtml" id="534099908446584030" style="width: 100%; overflow-y: hidden;">
    <!-- HTML generated using hilite.me -->
    <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
     <pre style="margin: 0; line-height: 125%">
<span style="color: #888888"># mix.exs</span>
<span style="color: #008800; font-weight: bold">def</span> deps <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">  </span>[{<span style="color: #AA6600">:conejo</span>, <span style="color: #AA6600">git:</span> <span style="background-color: #fff0f0">"https://github.com/mendrugory/conejo.git"</span>}] <span style="color: #888888"># or [{:conejo, "~&gt; 0.1.0"}] when is available</span>
<span style="color: #008800; font-weight: bold">end</span>

<span style="color: #008800; font-weight: bold">def</span> application <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold"> </span>[<span style="color: #AA6600">applications:</span> [<span style="color: #AA6600">:conejo</span>]]
<span style="color: #008800; font-weight: bold">end</span>
</pre>
    </div>
   </div>
  </div>
  <div class="paragraph">
    Check it in
   <a href="https://github.com/mendrugory/conejo" target="_blank">
     Github
   </a>
    .
   <br>
   </br>
  </div>
 </hr>
 </hr>
</div>
