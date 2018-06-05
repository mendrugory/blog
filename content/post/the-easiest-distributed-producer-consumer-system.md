---
title: "The Easiest Distributed Producer-Consumer System"
date: 2017-05-26
url: /blog/the-easiest-distributed-producer-consumer-system
---

<div class="blog-content">
 <div class="paragraph">
  Two months ago I published in
 <a href="https://hex.pm/" target="_blank">
   hex
 </a>
  the first version of
 <a href="https://hex.pm/packages/galena" target="_blank">
   Galena
 </a>
  , a Topic producer-consumer library built on top of
 <a href="https://github.com/elixir-lang/gen_stage" target="_blank">
   GenStage
 </a>
  for
 <a href="http://elixir-lang.org/" target="_blank">
   Elixir
 </a>
  . It was initially designed to create some flows of
 <a href="https://hexdocs.pm/galena/Galena.Producer.html" target="_blank">
   producers
 </a>
  ,
 <a href="https://hexdocs.pm/galena/Galena.ProducerConsumer.html" target="_blank">
   producer-consumers
 </a>
  and
 <a href="https://hexdocs.pm/galena/Galena.Consumer.html" target="_blank">
   consumers
 </a>
  where the message has to be delivered as soon as possible. Each consumer or producer-consumer can receive messages from several producers and/or producer-consumers. Besides, consumers and producer-consumers can select the messages that want to receive thanks to a topic approach.
 <br>
  <br>
    This library was thought to be used by an application which will run in a server but, why don't we try to distribute the Galena's producers, producer-consumers, consumers to several machines?
   <br>
    <br>
      Elixir code runs in the Erlang Virtual Machine or BEAM and therefore it can take advantage of all its distribution features.
     <br>
      <br>
        Erlang provides the module
       <a href="http://erlang.org/doc/man/global.html" target="_blank">
         global
       </a>
        which helps us to registry the name of the process globally. It means that we can interact with any globally registered process after connecting the nodes.
       <br>
        <br>
          Taking into account these ideas we can use Galena as a distributed producer-consumer system:
         <br>
         </br>
        </br>
       </br>
      </br>
     </br>
    </br>
   </br>
  </br>
 </br>
 </div>
 <div>
 <div align="center" class="wcustomhtml" id="986571655195118012" style="width: 100%; overflow-y: hidden;">
  <!-- HTML generated using hilite.me -->
  <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
   <pre style="margin: 0; line-height: 125%">
iex(test1<span style="color: #996633">@machine1</span>)<span style="color: #333333">&gt;</span> <span style="color: #003366; font-weight: bold">MyProducer</span><span style="color: #333333">.</span>start_link([], [<span style="color: #AA6600">name:</span> {<span style="color: #AA6600">:global</span>, <span style="color: #AA6600">:producer</span>}]

iex(test2<span style="color: #996633">@machine2</span>)<span style="color: #333333">&gt;</span> <span style="color: #003366; font-weight: bold">MyConsumer</span><span style="color: #333333">.</span>start_link([<span style="color: #AA6600">producers_info:</span> [{[<span style="background-color: #fff0f0">"topic"</span>], {<span style="color: #AA6600">:global</span>, <span style="color: #AA6600">:producer</span>}}]], [<span style="color: #AA6600">name:</span> {<span style="color: #AA6600">:global</span>, <span style="color: #AA6600">:consumer</span>}])
</pre>
  </div>
 </div>
 </div>
 <div class="paragraph">
 <br>
   Check the code in
  <a href="https://github.com/mendrugory/galena" target="_blank">
    Github
  </a>
   .
  <br>
   <br>
     Enjoy your distributed messages !!!
    <br>
    </br>
   </br>
  </br>
 </br>
 </div>
</div>
