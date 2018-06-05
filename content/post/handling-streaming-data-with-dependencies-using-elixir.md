---
title: "Handling Streaming Data with Dependencies using Elixir"
date: 2017-04-10
url: /blog/handling-streaming-data-with-dependencies-using-elixir
---

<div class="blog-content">
 <div class="paragraph">
 <font size="5">
  <strong>
   <em>
     Life is a succession of asynchronous events which could depend on each other.
   </em>
  </strong>
 </font>
 <br>
  <br>
    Some weeks ago my friends and I wanted to play a football match, therefore we booked a
   <span>
    <span>
      football pitch of a sport centre for the next Saturday at 17pm. On that Saturday, I arrived to the sport centre at 16:44 and other guys were already there and they had arrived between 16:31 and 16:42. Five minutes later arrived more people. At 16:50 only three people were not there, we were 19 and and we needed 22 in order to enjoy a proper football match. We called them but only one was contacted and he arrived 3 minutes later. At 17:00 we were 20 people and we decided to wait 5 minutes more before beginning the football match. They did not arrive and we began to play a football match with teams of 10 people.
     <br>
      <br>
        As you can see, the football match is an event that depends on other events like booking the football pitch or people arriving. When the event has to be processed, you have already checked all your dependencies, if not, you can wait or try to get them. Finally you can perform the event, cancel it or adapt it.
      </br>
     </br>
    </span>
   </span>
   <br>
   </br>
  </br>
 </br>
 </div>
 <div>
 <div style="height: 20px; overflow: hidden; width: 100%;">
 </div>
 <hr class="styled-hr" style="width:100%;">
  <div style="height: 20px; overflow: hidden; width: 100%;">
  </div>
 </hr>
 </div>
 <h2 class="wsite-content-title">
  Streaming Data
 <br/>
 </h2>
 <div class="paragraph">
  Nowadays I have been dealing with a new problem: Working with a streaming of data where the received messages could have relation between them. When I work with a stream of data and my chosen language has been Elixir, I try to take advantage of the concurrency offered by the Erlang Virtual Machine. That approach help me to
 <span>
  <span>
    parallelize
  </span>
 </span>
  as much as possible improving the performance but also other challenges are introduced, for instance, the data is not ordered, hence; trying to process messages in concurrent flows, could increase the difficulties.
 <br>
  <br>
    Meanwhile I was reading
   <a href="https://www.amazon.es/Designing-Scalability-Erlang-OTP-Fault-Tolerant/dp/1449320732" target="_blank">
     Designing for Scalability with Erlang/OTP
   </a>
    and it explains how the synchronous and asynchronous calls to a gen_server are implemented. It gave me an idea. If I am going to process each received event in a different process, why don't I check/request its dependencies and the process will wait for a message with:
   <ul>
    <li>
      The
     <em>
       dependency
     </em>
      : when the received event needs its dependencies to be processed.
     <br/>
    </li>
    <li>
      A
     <em>
       reference to the dependency
     </em>
      : when the received event only needs to know that the dependencies are there in order to be processed later.
     <br/>
    </li>
    <li>
      (Optional but recommended) A
     <em>
       time out message
     </em>
      when the dependency does not arrive. In this case you will decide if waiting more, request the dependency (if it is possible), continue the processing without the dependency (if it is possible), discard the event, ...
    </li>
   </ul>
  </br>
 </br>
 </div>
 <h2 class="wsite-content-title">
  Conductor for our orchestra of processes
 <br/>
 </h2>
 <div class="paragraph">
 <a href="https://hex.pm/packages/barenboim" target="_blank">
   Barenboim
 </a>
  is a OTP application which will help you with this task.
 <br>
  <br>
    In the process where the received event is being processed, define how to check/request your dependency and call the function get_data() of Barenboim module.
   <br>
   </br>
  </br>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="304119729293128262" style="width: 100%; overflow-y: hidden;">
  <!-- HTML generated using hilite.me -->
  <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
   <pre style="margin: 0; line-height: 125%">
fun <span style="color: #333333">=</span> <span style="color: #008800; font-weight: bold">fn</span>(dependency_ref) <span style="color: #333333">-&gt;</span> <span style="color: #003366; font-weight: bold">MyDataModule</span><span style="color: #333333">.</span>get(dependency_ref) <span style="color: #008800; font-weight: bold">end</span>
{<span style="color: #AA6600">:ok</span>, data} <span style="color: #333333">=</span> <span style="color: #003366; font-weight: bold">Barenboim</span><span style="color: #333333">.</span>get_data(dependency_ref, fun)
</pre>
  </div>
 </div>
 </div>
 <div class="paragraph">
  The checking phase will be executed in the same process, but if the dependency is not available yet, the process will wait for a message from Barenboim which will send the dependency when this one is ready.
 <br>
  <br>
    Meanwhile, another process receives the wanted event and process it. When this event is processed, this process has to notify to Barenboim that the event is ready.
   <br>
   </br>
  </br>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="851641138445063686" style="width: 100%; overflow-y: hidden;">
  <!-- HTML generated using hilite.me -->
  <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
   <pre style="margin: 0; line-height: 125%">
<span style="color: #888888"># Only the reference</span>
<span style="color: #003366; font-weight: bold">Barenboim</span><span style="color: #333333">.</span>notify({<span style="color: #AA6600">:reference</span>, dependency_ref})

<span style="color: #888888"># Or the data</span>
<span style="color: #003366; font-weight: bold">Barenboim</span><span style="color: #333333">.</span>notify({<span style="color: #AA6600">:data</span>, dependency_ref, dependency_data})
</pre>
  </div>
 </div>
 </div>
 <div class="paragraph">
  I highly recommend to pass a time out to the function get_data(), if not, it will wait forever (maybe it fits your needs because you are very sure that all the events will arrive). In that case, you can receive a time out message:
 <br>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="833333695121697442" style="width: 100%; overflow-y: hidden;">
  <!-- HTML generated using hilite.me -->
  <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
   <pre style="margin: 0; line-height: 125%">
<span style="color: #008800; font-weight: bold">case</span> <span style="color: #003366; font-weight: bold">Barenboim</span><span style="color: #333333">.</span>get_data(dependency_ref, fun, <span style="color: #6600EE; font-weight: bold">5_000</span>) <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold"> </span>{<span style="color: #AA6600">:ok</span>, data} <span style="color: #333333">-&gt;</span> <span style="color: #888888"># go on</span>
  {<span style="color: #AA6600">:timeout</span>, any} <span style="color: #333333">-&gt;</span> <span style="color: #888888"># decide what you can do</span>
<span style="color: #008800; font-weight: bold">end</span>
</pre>
  </div>
 </div>
 </div>
 <div class="paragraph">
  Barenboim uses a pool of process (
 <a href="https://github.com/devinus/poolboy" target="_blank">
   poolboy
 </a>
  ) and can be configured it (check the
 <a href="https://hexdocs.pm/barenboim/Barenboim.html" target="_blank">
   docs
 </a>
  ).
 <br>
  <br>
    Enjoy Streaming !!!
   <br>
   </br>
  </br>
 </br>
 </div>
</div>
