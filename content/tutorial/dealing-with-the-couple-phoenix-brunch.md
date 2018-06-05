---
title: "Dealing with the couple Phoenix & Brunch"
date: 2017-02-11
url: /tutorial/dealing-with-the-couple-phoenix-brunch
tags: ["tutorial"]
index: true
---

<div class="blog-content">
 <span class="imgPusher" style="float:left;height:0px">
 </span>
 <span style="display: table;width:203px;position:relative;float:left;max-width:100%;;clear:left;margin-top:0px;*margin-top:0px">
 <a>
  <img alt="Picture" class="galleryImageBorder wsite-image" src="/img/phoenix_1.png" style="margin-top: 5px; margin-bottom: 10px; margin-left: 0px; margin-right: 10px; border-width:1px;padding:3px; max-width:100%"/>
 </a>
 <span class="wsite-caption" style="display: table-caption; caption-side: bottom; font-size: 90%; margin-top: -10px; margin-bottom: 10px; text-align: center;">
 </span>
 </span>
 <div class="paragraph" style="display:block;">
  Currently I am working in a project where I have to integrate many real-time data sources and show the results to the users as soon as possible. As you can imagine
 <a href="http://elixir-lang.org/" target="_blank">
   Elixir
 </a>
  +
 <a href="http://phoenixframework.org/" target="_blank">
   PhoenixFramework
 </a>
  are my chosen tools. This kind of projects always imply a lot of challenges that we have to solved (check my fork of
 <a href="https://github.com/mendrugory/floki" target="_blank">
   Floki
 </a>
  ). But the tedious one has not been directly related with the particular case of this project, it has been dealing with
 <a href="http://brunch.io/" target="_blank">
   Brunch
 </a>
  . Brunch is a front-end build tool that is brought by Phoenix by default. It seems to be simple (especially if you compare with
 <a href="https://webpack.github.io/" target="_blank">
   Webpack
 </a>
  ), but my problems began when I need to execute different Javascript scripts depending on the rendered Phoenix template.
 <br>
 </br>
 </div>
 <hr style="width:100%;clear:both;visibility:hidden;">
 <span class="imgPusher" style="float:right;height:0px">
 </span>
 <span style="display: table;width:auto;position:relative;float:right;max-width:100%;;clear:right;margin-top:0px;*margin-top:0px">
  <a>
   <img alt="Picture" class="galleryImageBorder wsite-image" src="/img/brunch_1.jpeg" style="margin-top: 10px; margin-bottom: 10px; margin-left: 0px; margin-right: 10px; border-width:0; max-width:100%"/>
  </a>
  <span class="wsite-caption" style="display: table-caption; caption-side: bottom; font-size: 90%; margin-top: -10px; margin-bottom: 10px; text-align: center;">
  </span>
 </span>
 <div class="paragraph" style="display:block;">
   The HTML from Phoenix are rendered in server side and Brunch compiles and joins everything (by default) to one Javascript file(or maybe two if you want to have the 3rd party in other place). That provokes that you will have an unique entry point, but if you will have several "views" already rendered from the server, maybe you will have several needs that will be covered by different Javascript codes and you don't want to execute always all.
 </div>
 <hr style="width:100%;clear:both;visibility:hidden;">
  <div class="paragraph">
   <strong>
     ¿How did I finally solve it?
   </strong>
   <br>
    <br>
      Brunch can compile and join the Javascript files in the way that you decide, therefore I decided to create two main folders inside web/static/js path:
     <ul>
      <li>
        common: It will contain all the logic
      </li>
      <li>
        specific: It will contain the entry points
      </li>
     </ul>
     <br>
       We will say to brunch that it has to produce a common.js file for all the javascript files that are not in "specific" folder (common + 3rd party) and one file per template that will be under "specific" folder (I created a folder per template because it was better for my sanity where the js file and the folder have the same name. Maybe in the future you have to have more specific logic in each template and you need to have more js files in each folder). I know that I will finally produce more files that we would have desired, but the heaviest part should be in common.js, and each entry point Javascript file will contain a few lines of code (in my case, less than 10 lines each) because we only need to use these files for initializations.
      <br>
      </br>
     </br>
    </br>
   </br>
  </div>
  <div>
   <div align="left" class="wcustomhtml" id="233063890483428982" style="width: 100%; overflow-y: hidden;">
    <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
     <pre style="margin: 0; line-height: 125%">
<span style="color: #000000;">exports.config</span> <span style="color: #000000;">=</span> {
 <span style="color: #000000;">files:</span> <span style="color: #000000;">{</span>
   <span style="color: #000000;">javascripts:</span> <span style="color: #000000;">{</span>
     <span style="color: #000000;">joinTo:</span> <span style="color: #000000;">{</span>
       <span style="color: #007700">"js/common.js"</span>: <span style="color: #000000;">/(?!web\/static\/js\/specific)/</span>,
       <span style="color: #007700">"js/myscript.js"</span>: <span style="color: #000000;">/^(web\/static\/js\/specific\/one)/</span>,
       <span style="color: #007700">"js/myscript2.js"</span>: <span style="color: #000000;">/^(web\/static\/js\/specific\/two)/</span>,
       <span style="color: #007700">"js/myscript3.js"</span>: <span style="color: #000000;">/^(web\/static\/js\/specific\/three)/</span>
      }
<span style="color: #000000;">...</span>
<span style="color: #000000;">}</span>
</pre>
    </div>
   </div>
  </div>
  <div class="paragraph">
    All the Phoenix templates are rendered using a layout (web/static/templates/layout/app.html.eex). This file assumes that Brunch is going to produce a Javascript file called app.js which will contain all the logic that we need and also the entry point. In our case, we have to substitute this file by common.js because this new file will contain all the logic. And then, we have to add two new lines:
   <br>
   </br>
  </div>
  <div>
   <div align="left" class="wcustomhtml" id="960471825842483900" style="width: 100%; overflow-y: hidden;">
    <!-- HTML generated using hilite.me -->
    <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
     <pre style="margin: 0; line-height: 125%">
<span style="color: #007700">&lt;script </span><span style="color: #0000CC">type=</span><span style="background-color: #fff0f0">"text/javascript"</span> <span style="color: #0000CC">src=</span><span style="background-color: #fff0f0">"&lt;%= static_path(@conn, "</span><span style="color: #FF0000; background-color: #FFAAAA">/</span><span style="color: #0000CC">your_app</span><span style="color: #FF0000; background-color: #FFAAAA">/</span><span style="color: #0000CC">js</span><span style="color: #FF0000; background-color: #FFAAAA">/</span><span style="color: #0000CC">common</span><span style="color: #FF0000; background-color: #FFAAAA">.</span><span style="color: #0000CC">js</span><span style="color: #FF0000; background-color: #FFAAAA">")</span> <span style="color: #FF0000; background-color: #FFAAAA">%</span><span style="color: #007700">&gt;</span><span style="color: #FF0000; background-color: #FFAAAA">"</span><span style="color: #333333">&gt;</span><span style="color: #007700">&lt;/script&gt;</span>
<span style="color: #007700">&lt;script </span><span style="color: #0000CC">type=</span><span style="background-color: #fff0f0">"text/javascript"</span> <span style="color: #0000CC">src=</span><span style="background-color: #fff0f0">"&lt;%= static_path(@conn, "</span><span style="color: #FF0000; background-color: #FFAAAA">/</span><span style="color: #0000CC">your_app</span><span style="color: #FF0000; background-color: #FFAAAA">/</span><span style="color: #0000CC">js</span><span style="color: #FF0000; background-color: #FFAAAA">/"</span> <span style="color: #FF0000; background-color: #FFAAAA">&lt;</span><span style="color: #007700">&gt;</span> assigns[<span style="color: #333333">:</span>js_script]) <span style="color: #333333">&lt;&gt;</span> <span style="background-color: #fff0f0">".js"</span> <span style="color: #333333">%&gt;</span><span style="color: #FF0000; background-color: #FFAAAA">"</span><span style="color: #333333">&gt;</span><span style="color: #007700">&lt;/script&gt;</span>
<span style="color: #007700">&lt;script&gt;</span>require(<span style="background-color: #fff0f0">"&lt;%= "</span>web<span style="color: #333333">/</span><span style="color: #008800; font-weight: bold">static</span><span style="color: #333333">/</span>js<span style="color: #333333">/</span>specific<span style="color: #333333">/</span><span style="background-color: #fff0f0">" &lt;&gt; assigns[:js_script] &lt;&gt; "</span><span style="color: #333333">/</span><span style="background-color: #fff0f0">" &lt;&gt; assigns[:js_script] %&gt;"</span>)<span style="color: #007700">&lt;/script&gt;</span>
</pre>
    </div>
   </div>
  </div>
  <div class="paragraph">
    The first one will indicate the necessary Javascript file in each case and the second one will indicate that it is the entry point, so it will be executed.
   <br>
    <br>
      As you can see, we have used the possibility of reading custom values that could be stored in the user connection, hence; we only have to indicate in the variable ":js_script" which one should be used in each case.
     <br>
     </br>
    </br>
   </br>
  </div>
  <div>
   <div align="left" class="wcustomhtml" id="302950672358084645" style="width: 100%; overflow-y: hidden;">
    <!-- HTML generated using hilite.me -->
    <div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;">
     <pre style="margin: 0; line-height: 125%">
<span style="color: #008800; font-weight: bold">defmodule</span> <span style="color: #003366; font-weight: bold">YourApp</span><span style="color: #333333">.</span><span style="color: #003366; font-weight: bold">YourController</span> <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">  use</span> <span style="color: #003366; font-weight: bold">YourApp</span><span style="color: #333333">.</span><span style="color: #003366; font-weight: bold">Web</span>, <span style="color: #AA6600">:controller</span>

 <span style="color: #008800; font-weight: bold">def</span> index(conn, _params) <span style="color: #008800; font-weight: bold">do</span>
<span style="color: #008800; font-weight: bold">   </span>conn
   <span style="color: #333333">|&gt;</span> assign(<span style="color: #AA6600">:js_script</span>, <span style="background-color: #fff0f0">"my_script"</span>)
   <span style="color: #333333">|&gt;</span> render(<span style="background-color: #fff0f0">"index.html"</span>)
 <span style="color: #008800; font-weight: bold">end</span>

<span style="color: #008800; font-weight: bold">end</span>
</pre>
    </div>
   </div>
  </div>
  <div class="paragraph">
    The main logic and the 3rd party code which are the heaviest part will be in common.js, so it will be downloaded once and cached by the browser and the specific parts, just your few lines of initialization, will be downloaded for each template, although it will finally be cached as well.
   <br>
    <br>
      This method was the simplest one that I found in order to solve my problems, I hope that it is useful for someone else.
     <br>
     </br>
    </br>
   </br>
  </div>
 </hr>
 </hr>
</div>
