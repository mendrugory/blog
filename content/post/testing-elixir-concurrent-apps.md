---
title: "Testing Elixir Concurrent Apps"
date: 2017-03-30
url: /post/testing-elixir-concurrent-apps
index: true
---

<div class="blog-content">
 <div class="paragraph">
  This week I have released my two first contributions to the elixir open source community in
 <a href="http://hex.pm/" target="_blank">
   hex
 </a>
  :
 <strong>
   Galena
 </strong>
  and
 <strong>
   Conejo
 </strong>
  .
 <br>
  <br>
   <a href="https://hex.pm/packages/galena" target="_blank">
    <strong>
      Galena
    </strong>
   </a>
    is a library based on
   <a href="https://hex.pm/packages/gen_stage" target="_blank">
     GenStage
   </a>
    that will help you to create your topic producer-consumer data pipelines.
   <br>
    <br>
     <a href="https://hex.pm/packages/conejo" target="_blank">
      <strong>
        Conejo
      </strong>
     </a>
      is an OTP application/library that facilitate the creation of publishers and consumers of
     <a href="https://www.rabbitmq.com/" target="_blank">
       RabbitMQ
     </a>
      or AMQP servers. It is based on the library
     <a href="https://github.com/pma/amqp" target="_blank">
       amqp
     </a>
      .
     <br>
     </br>
    </br>
   </br>
  </br>
 </br>
 </div>
 <div class="wsite-spacer" style="height:44px;">
 </div>
 <div class="paragraph" style="text-align:left;">
  I am very surprised that most of the questions that I have received are about the tests:
 <br>
  <br>
   <em>
     "How do you test the libraries if they provide Behaviours to develop concurrent apps?"
   </em>
   <br>
    <br>
      Let's see how.
     <br>
     </br>
    </br>
   </br>
  </br>
 </br>
 </div>
 <div class="wsite-spacer" style="height:31px;">
 </div>
 <h2 class="wsite-content-title">
  Testing
 <br/>
 </h2>
 <div class="paragraph">
  Elixir has a really good unit test framework,
 <a href="https://hexdocs.pm/ex_unit/ExUnit.html" target="_blank">
   ExUnit
 </a>
  , which is really easy to use. Otherwise we are talking about concurrent apps, where more of the logic will be executed in different processes, because we want to scale and Elixir allows you to do it, and ExUnit will only pass a test if the assert function is in the main process.
 <br>
  <br>
    As we have already mentioned we are using Elixir and we use the Actor Model, therefore we can send messages between the processes. Let's see how to use it for testing.
   <br>
   </br>
  </br>
 </br>
 </div>
 <div class="paragraph">
 <strong>
   Register the main Process
  <br/>
 </strong>
  The first step is to register the process where the test is running:
 <br>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="975457614549993096" style="width: 100%; overflow-y: hidden;">
  <!-- HTML generated using hilite.me -->
  <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
   <pre style="margin: 0; line-height: 125%">
test <span style="background-color: #fff0f0">"receive message"</span> <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold"> </span><span style="color: #003366; font-weight: bold">Process</span><span style="color: #333333">.</span>register(self(), <span style="color: #AA6600">:main_process</span>)
<span style="color: #333333">...</span>
</pre>
  </div>
 </div>
 </div>
 <div class="paragraph">
 <strong>
   Send the message with the result to the Main Process
 </strong>
 <br>
   The callback which will handle the message in the secondary process, has to send the result to the main process.
  <br>
  </br>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="290979635155908287" style="width: 100%; overflow-y: hidden;">
  <!-- HTML generated using hilite.me -->
  <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
   <pre style="margin: 0; line-height: 125%">
<span style="color: #333333">...</span>
<span style="color: #008800; font-weight: bold">def</span> handle_message(message) <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold"> </span>result <span style="color: #333333">=</span> do_work(message)
  send(<span style="color: #AA6600">:main_process</span>, result)
 <span style="color: #333333">...</span>
<span style="color: #008800; font-weight: bold">end</span>
<span style="color: #333333">...</span>
</pre>
  </div>
 </div>
 </div>
 <div class="paragraph">
 <strong>
   Check the result in the Main Process
 </strong>
 <br>
   Finally we have to check the result in the main process. Sometimes, we are only interested in knowing that the message was processed by the secondary process and sent to the main one:
  <br>
  </br>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="111055268660012674" style="width: 100%; overflow-y: hidden;">
  <!-- HTML generated using hilite.me -->
  <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
   <pre style="margin: 0; line-height: 125%">
<span style="color: #888888"># Main Process</span>
<span style="color: #333333">...</span>
<span style="color: #003366; font-weight: bold">Logger</span><span style="color: #333333">.</span>info(<span style="background-color: #fff0f0">"Waiting for the message ..."</span>)
assert_receive(message, timeout)
</pre>
  </div>
 </div>
 </div>
 <div class="paragraph">
  But in other cases we also want to check that the secondary process executed its transformation well. (Assuming that the message is a tuple with a topic and a result)
 <br>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="470482798586984341" style="width: 100%; overflow-y: hidden;">
  <!-- HTML generated using hilite.me -->
  <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
   <pre style="margin: 0; line-height: 125%">
<span style="color: #888888"># Main Process</span>
<span style="color: #333333">...</span>
<span style="color: #008800; font-weight: bold">receive</span> <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold"> </span>{topic, result} <span style="color: #333333">-&gt;</span>
    assert topic <span style="color: #333333">==</span> expected_topic
    assert result <span style="color: #333333">==</span> expected_result
  msg <span style="color: #333333">-&gt;</span>
    assert <span style="color: #003366; font-weight: bold">false</span>
<span style="color: #008800; font-weight: bold">end</span>
</pre>
  </div>
 </div>
 </div>
 <div class="wsite-spacer" style="height:50px;">
 </div>
 <div class="paragraph">
  This is only an introduction with some easy examples, but you can do a lot with this approach.
 <br>
  <br>
    You can check the tests of
   <a href="https://github.com/mendrugory/conejo" target="_blank">
     Conejo
   </a>
    and
   <a href="https://github.com/mendrugory/galena" target="_blank">
     Galena
   </a>
    .
   <br>
   </br>
  </br>
 </br>
 </div>
</div>
