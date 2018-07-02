---
title: "Micro-Services, Mesos, Marathon, Docker and Auto-Discovery"
date: 2016-08-26
url: /post/micro-services-mesos-marathon-docker-and-auto-discovery
---

<div class="blog-content">
 <span class="imgPusher" style="float:left;height:0px">
 </span>
 <span style="display: table;width:281px;position:relative;float:left;max-width:100%;;clear:left;margin-top:0px;*margin-top:0px">
 <a>
  <img alt="Picture" class="galleryImageBorder wsite-image" src="/img/mesosmarathondocker.png" style="margin-top: 5px; margin-bottom: 10px; margin-left: 0px; margin-right: 10px; none; max-width:100%"/>
 </a>
 <span class="wsite-caption" style="display: table-caption; caption-side: bottom; font-size: 90%; margin-top: -10px; margin-bottom: 10px; text-align: center;">
 </span>
 </span>
 <div class="paragraph" style="display:block;">
  Since I discovered the couple
 <a href="http://mesos.apache.org/" target="_blank">
   Mesos
 </a>
  -
 <a href="https://mesosphere.github.io/marathon/" target="_blank">
   Marathon
 </a>
  , I always try to use them to put my projects on production. They provide web interfaces where you can check your apps, a very easy REST API in order to interact with the apps, an easy way of managing the resources of your cluster and the most important part for me,
 <em>
   High Availability.
 </em>
 <br/>
 <br/>
  Marathon provides support for Docker containers what is a wonderful piece of news. Currently everything to me is a container. If you launch a Docker container using the
 <em>
   cmd command
 </em>
  of Marathon, the tool will be able to launch the app and it will be able to tracked as well but you will not be able to switch down from Marathon.
 <br/>
 <br/>
  This tech stack is amazing if your project is micro-services oriented. You will launch independent apps that are packaged in Docker images through Marathon. So far, it seems to be a perfect scenario, but Mesos will execute your apps in one server of your cluster (or some, depending on the instances that you have launched)  that you will not know at the beginning. If one app don't know where is running another app that it needs to request data to process it, how is it going to work? We have a bunch of blind apps.
 <br/>
 <br/>
  We have to add an auto-discovery system. You can find
 <a href="https://mesosphere.github.io/marathon/docs/service-discovery-load-balancing.html" target="_blank">
   several options
 </a>
  , I like to use
 <a href="https://mesosphere.github.io/mesos-dns/" target="_blank">
   Mesos-DNS
 </a>
  . This tool is a classical DNS but it asks to Mesos which apps are running on it, so through this DNS all the apps will know where the other apps are. One important thing will be how the app will be named because you won't have an IP or host name, you will have an app name:
 <em>
   &lt;the name of the app that is the id of marathon&gt;.marathon.mesos.
 </em>
 <br/>
 </div>
 <hr style="width:100%;clear:both;visibility:hidden;"/>
 <div>
 <div class="wsite-image wsite-image-border-none " style="padding-top:10px;padding-bottom:10px;margin-left:0;margin-right:0;text-align:center">
  <a>
   <img alt="Picture" src="/img/architecture_orig.png" style="width:auto;max-width:100%"/>
  </a>
  <div style="display:block;font-size:90%">
  </div>
 </div>
 </div>
 <div class="paragraph">
  Mesos-DNS will be another app launched through Marathon. You can find several docker images in
 <a href="https://hub.docker.com/search/?isAutomated=0&amp;isOfficial=0&amp;page=1&amp;pullCount=0&amp;q=Mesos-dns&amp;starCount=0" target="_blank">
   Docker Hub
 </a>
  . I like
 <a href="https://hub.docker.com/r/tobilg/mesos-dns/" target="_blank">
   tobilg/mesos-dns
 </a>
  .
 <br/>
 </div>
 <div class="paragraph">
  Now, we only have to add the DNS to all the Docker containers of our apps. And here I found a lot of problems.
 <br/>
 <br/>
  The network configuration has to be HOST if we want DNS-mesos works, something that is not compatible with adding a DNS through the Docker support of Marathon. We could add a DNS using an usual terminal command of Docker through the cmd command of Marathon, but then we will not be able to manage the apps very well. We could
 <a href="https://mesosphere.github.io/mesos-dns/docs/" target="_blank">
   overwrite
 </a>
  /etc/resolv.conf file with the configuration  that we want when the container starts up, but this method is very ugly.
 <br/>
 <br/>
 <br/>
  How did I solve it? In a really easy way:
 <br/>
 <br/>
  The DNS has to be running always on the same server and we need to know the IP, so you can use the
 <a href="https://mesosphere.github.io/marathon/docs/constraints.html" target="_blank">
   constraints
 </a>
  of Marathon in order to indicate where the DNS (or DNSs to have HA) will be running.
 <br/>
 <br/>
  If you have your apps in production, you will not try to mix apps of several projects which will be pointing to different DNSs. If you are in the right case, you have to point the host server to the known IP of Mesos-DNS. Docker makes a copy of the /etc/resolv.conf file of the host server into the Docker container when no DNS information is provided. Now, we only need to launch our apps through Marathon indicating HOST as network option.
 <br/>
 </div>
 <div class="wsite-adsense">
 <script src="//www.weebly.com/weebly/apps/serveAds.php?type=adsense&amp;elementid=615014495575564385&amp;ineditor=0&amp;subdomain=mendrugory.weebly.com&amp;pubid=pub-2477529535053270&amp;adformat=468x60&amp;adtype=text_image&amp;bordercolor=FFFFFF&amp;bgcolor=FFFFFF&amp;linkcolor=0F53FF&amp;textcolor=000000&amp;urlcolor=008000" type="text/javascript">
 </script>
 </div>
</div>
