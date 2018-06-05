---
title: "Reactive Architecture with Python"
date: 2016-03-26
url: /tutorial/reactive-architecture-with-python
tags: ["tutorial"]
index: true
---

<div class="blog-content">
 <div class="paragraph" style="text-align:left;">
  Currently all systems try to be real time systems, or at least soft-real-time systems. In order to get it, most of the systems abuse of
 <a href="https://en.wikipedia.org/wiki/Pull_technology" target="_blank">
   pulling
 </a>
  methodology. This approach provokes a lot of
 <span>
  <span>
    useless requests that cause overloading. Why are they useless? Because this approach tries to catch the changes requesting and requesting over and over, but it will always have a delay. The new data has to be there when the request is done.
  </span>
 </span>
 <br/>
 </div>
 <div class="paragraph" style="text-align:left;">
  Nowadays we have languages like
 <a href="https://www.erlang.org/" target="_blank">
   Erlang
 </a>
  or
 <a href="http://elixir-lang.org/" target="_blank">
   Elixir
 </a>
  (I am very excited with Elixir and you will see some posts in the future) or frameworks like
 <a href="http://akka.io/" target="_blank">
   Akka
 </a>
  . But what can we do when we have a lot of complex algorithms written in a language like
 <a href="https://www.python.org/" target="_blank">
   Python
 </a>
  or your team does not know Erlang?
 <br/>
 <br/>
  For this reason I decided to build a proof of concept of a reactive architecture using
 <a href="https://www.python.org/" target="_blank">
   Python
 </a>
  . The PoC will be a system which will track the used CPU of a machine, although it is (almost) prepared for measuring the used resources of a lot of machines.
 <br/>
 <br/>
 </div>
 <div class="paragraph" style="text-align:justify;">
  In order to build a complete system I have chosen
 <a href="http://www.tornadoweb.org/en/stable/" target="_blank">
   Tornado Web Framework
 </a>
  ,
 <a href="http://www.rabbitmq.com/" target="_blank">
   RabbitMQ
 </a>
  and
 <a href="https://angularjs.org/" target="_blank">
   AngularJS
 </a>
  to build the different modules.
 <br/>
 <br/>
  The first module,
 <a href="https://github.com/mendrugory/reactive-architecture-python/tree/master/resources-tracker" target="_blank">
   resources-tracker,
 </a>
 <span id="selectionBoundary_1459035203267_620750873143664">
   ﻿
 </span>
 <span id="selectionBoundary_1459035203266_008041173279814129">
   ﻿
 </span>
  will be on charge of measuring the CPU and sending through the RabbitMQ broker using a
 <a href="https://www.rabbitmq.com/tutorials/tutorial-five-python.html" target="_blank">
   topic exchange
 </a>
  . The topic will be
 <em>
   &lt;name-of-the-machine&gt;.measure.
 </em>
 <br/>
 <br/>
 </div>
 <div>
 <div class="wsite-image wsite-image-border-medium " style="padding-top:5px;padding-bottom:10px;margin-left:0px;margin-right:10px;text-align:left">
  <a>
   <img alt="Picture" src="/img/9318034_orig.png" style="width:auto;max-width:100%"/>
  </a>
  <div style="display:block;font-size:90%">
  </div>
 </div>
 </div>
 <div class="paragraph" style="text-align:left;">
 <br/>
  The next module,
 <a href="https://github.com/mendrugory/reactive-architecture-python/tree/master/resources-analyzer" target="_blank">
   resources-analyzer
 </a>
  , will receive all the measures (topic:
 <em>
   #.measure
 </em>
  ), will determine the range of use of the CPU of the machine (
 <em>
   HIGH, MEDIUM, LOW)
 </em>
  and will send it through the RabbitMQ broker using the topic
 <em>
   &lt;machine's-name-of-received-measure&gt;.analysis
 </em>
  .
 <br/>
 <br/>
  The last module,
 <a href="https://github.com/mendrugory/reactive-architecture-python/tree/master/resources-web" target="_blank">
   resources-web
 </a>
  , will show the results of the module before. For this task Tornado web framework which will be connected to RabbitMQ using the
 <a href="https://pika.readthedocs.org/en/0.9.13/modules/adapters/tornado.html" target="_blank">
   TornadoConnection
 </a>
  of
 <a href="http://pika.readthedocs.org" target="_blank">
   Pika
 </a>
  and to the simple AngularJS client through a
 <a href="https://en.wikipedia.org/wiki/WebSocket" target="_blank">
   WebSocket
 </a>
  .
 <br/>
 <br/>
  Therefore, each time a CPU measure is taken, the final result will be represented in a browser without making any request.
 <br/>
 </div>
 <div>
 <div class="wsite-image wsite-image-border-none " style="padding-top:10px;padding-bottom:10px;margin-left:0;margin-right:0;text-align:center">
  <a>
   <img alt="Picture" src="/img/5672711_orig.png" style="width:auto;max-width:100%"/>
  </a>
  <div style="display:block;font-size:90%">
  </div>
 </div>
 </div>
 <div class="paragraph" style="text-align:left;">
 <br/>
  This approach makes the system very scalable. Everything can be build as a module listening to the right topics and sending the result using another topic, in order to be used by other modules. The final picture can remind us a neural network.
 <br/>
 </div>
 <div>
 <div class="wsite-image wsite-image-border-none " style="padding-top:10px;padding-bottom:10px;margin-left:0;margin-right:0;text-align:center">
  <a>
   <img alt="Picture" src="/img/9121422.png" style="width:377;max-width:100%"/>
  </a>
  <div style="display:block;font-size:90%">
  </div>
 </div>
 </div>
 <div class="paragraph" style="text-align:left;">
 <br/>
  All the code can be found in
 <a href="https://github.com/mendrugory/reactive-architecture-python" target="_blank">
   github
 </a>
  . Each module contains a Dockerfile and it will help you a lot to run the entire system using
 <a href="https://www.docker.com/" target="_blank">
   Docker
 </a>
  .
 <br/>
 </div>
 <div class="wsite-adsense">
 <script src="//www.weebly.com/weebly/apps/serveAds.php?type=adsense&amp;elementid=846266586641027355&amp;ineditor=0&amp;subdomain=mendrugory.weebly.com&amp;pubid=pub-2477529535053270&amp;adformat=468x60&amp;adtype=text_image&amp;bordercolor=FFFFFF&amp;bgcolor=FFFFFF&amp;linkcolor=0F53FF&amp;textcolor=000000&amp;urlcolor=008000" type="text/javascript">
 </script>
 </div>
</div>
