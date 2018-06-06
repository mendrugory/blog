---
title: "Twitter Analytics in real time"
date: 2014-09-12
url: /post/twitter-analytics-in-real-time
index: true
---

<div class="blog-content">
 <div class="paragraph">
  Real-time analytics is very trendy. Nowadays, everyone wants to analyze the data that they have, even if those data are not very useful, and they want the results as soon as possible, even if they can wait for those results.
 <br>
  <span>
   <br>
    <span>
      One of the most powerful data sources are the social networks and inside this group, Twitter. Therefore I decided to build a system (as proof of concept) in order to analyze tweets related to BigData.
     <br>
      <span>
       <br>
        <span>
          Firstable I decided that the app would be deployed in
         <a href="https://www.openshift.com/products?gclid=CPP9nbWU3MACFdSWtAodG1IAOA" target="_blank" title="">
           openshift
         </a>
          . Openshift is a PaaS product from Red Hat and I like due to its flexibility.
         <a href="http://www.python.org" target="_blank" title="">
           Python
         </a>
          was the chosen language,
         <a href="https://www.djangoproject.com/" target="_blank" title="">
           Django
         </a>
          the chosen framework and
         <a href="http://www.mongodb.org/" target="_blank" title="">
           MongoDB
         </a>
          the chosen data base.
         <br>
          <span>
           <br>
            <span>
            </span>
             If you want to interact with Twitter you will need to register your app in order to get the Twitter's credentials for your new app.
            <span>
             <span>
               So, I visited the
              <a href="https://apps.twitter.com/" target="_blank" title="">
                Twitter Developers
              </a>
               web and I registered my app, talkingbigdata:
              <br>
               <span>
               </span>
              </br>
             </span>
            </span>
            <br>
             <span>
             </span>
            </br>
           </br>
          </span>
         </br>
        </span>
       </br>
      </span>
     </br>
    </span>
   </br>
  </span>
 </br>
 </div>
 <div>
 <div class="wsite-image wsite-image-border-medium" style="padding-top:5px;padding-bottom:10px;margin-left:0px;margin-right:10px;text-align:left">
  <a>
   <img alt="Picture" src="/img/1559413_orig.png" style="width:100%;max-width:645px"/>
  </a>
  <div style="display:block;font-size:90%">
  </div>
 </div>
 </div>
 <div class="paragraph" style="text-align:left;">
 <br>
  <span>
  </span>
  <font size="3">
    The most important part are:
   <br/>
  </font>
  <span>
   <font size="3">
    <em>
      - API key
     <br/>
    </em>
   </font>
   <span>
    <font size="3">
     <em>
       - API secret
      <br/>
     </em>
    </font>
    <span>
     <font size="3">
      <em>
        - Access token
       <br/>
      </em>
     </font>
     <span>
      <font size="3">
       <em>
         - Access token secret
       </em>
      </font>
      <br>
       <span>
       </span>
       <br>
        <span>
          Once that we have registered the app and we have obtained the credentials, let's code.
         <br>
          <span>
           <br>
            <span>
              The first part
            </span>
           </br>
          </span>
          <span>
            is connecting to the Twitter API Stream and for that purpose I utilized
           <a href="http://www.tweepy.org/" target="_blank" title="">
             tweepy
           </a>
            .
           <br>
            <span>
             <br>
              <span>
              </span>
              <br>
               <strong>
                <span>
                </span>
               </strong>
              </br>
             </br>
            </span>
            <strong>
              Authorization
            </strong>
             :
            <span>
             <br>
              <span>
              </span>
             </br>
            </span>
           </br>
          </span>
         </br>
        </span>
       </br>
      </br>
     </span>
    </span>
   </span>
  </span>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="111130199248475632" style="width: 100%; overflow-y: hidden;">
  <style type="text/css">
    .mycode, .mycode pre {         font-size: small;         color: black;         font-family: Consolas, "Courier New", Courier, Monospace;         background-color: #ffffff;         /*white-space: pre;*/ }  .mycode pre { margin: 0em; }  .mycode .rem { color: #008000; }  .mycode .kwrd { color: #0000ff; }  .mycode .str { color: #a31515; }  .mycode .op { color: #0000c0; }  .mycode .preproc { color: #cc6633; }  .mycode .asp { background-color: #ffff00; }  .mycode .html { color: #800000; }  .mycode .attr { color: #ff0000; }  .mycode .alt  {         background-color: #f4f4f4;         width: 100%;         margin: 0em; }  .mycode .lnum { color: #606060; }
  </style>
  <div class="mycode">
   <pre class="alt"> <span class="lnum">   1: </span><span class="kwrd">class</span> TwitterAuth(<span class="kwrd">object</span>): </pre>
   <pre> <span class="lnum">   2: </span>   <span class="str">''</span><span class="str">'</span> </pre>
   <pre class="alt"> <span class="str"><span class="lnum">   3: </span>    class to manage the twitter authorization</span> </pre>
   <pre> <span class="lnum">   4: </span>    '<span class="str">''</span> </pre>
   <pre class="alt"> <span class="lnum">   5: </span> </pre>
   <pre> <span class="lnum">   6: </span>    def __init__(self): </pre>
   <pre class="alt"> <span class="lnum">   7: </span>        self.auth = tweepy.OAuthHandler(secret.CONSUMER_KEY, secret.CONSUMER_SECRET) </pre>
   <pre> <span class="lnum">   8: </span>        self.auth.set_access_token(secret.ACCESS_TOKEN, secret.ACCESS_TOKEN_SECRET) </pre>
   <pre class="alt"> <span class="lnum">   9: </span>        self.api = tweepy.API(self.auth) </pre>
   <pre> <span class="lnum">  10: </span> </pre>
   <pre class="alt"> <span class="lnum">  11: </span>    def get_auth(self): </pre>
   <pre> <span class="lnum">  12: </span>       <span class="kwrd">return</span> self.auth </pre>
   <pre class="alt"> <span class="lnum">  13: </span> </pre>
   <pre> <span class="lnum">  14: </span>    def get_api(self): </pre>
   <pre class="alt"> <span class="lnum">  15: </span>       <span class="kwrd">return</span> self.api </pre>
  </div>
 </div>
 </div>
 <div class="paragraph" style="text-align:left;">
 <br>
  <span>
    Tweepy brings a very useful class, StreamListener(), which manages the Twitter's Stream. So I created CustomStreamListener class which inherits from StreamListener and it contains the common things and I also created MongoDBStreamListener class which inherits from
  </span>
   CustomStreamListener
  <span>
  </span>
   and it will perform the specific things for MongoDB data base. In this way I have the structure in order to add other data bases or even other things.
  <br>
   <span>
    <br>
     <span>
      <br>
       <span>
        <strong>
          Stream Listener
        </strong>
         :
        <span>
         <span>
          <br>
           <span>
           </span>
          </br>
         </span>
        </span>
       </span>
      </br>
     </span>
    </br>
   </span>
  </br>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="286955780396608849" style="width: 100%; overflow-y: hidden;">
  <style type="text/css">
    .mycode, .mycode pre {         font-size: small;         color: black;         font-family: Consolas, "Courier New", Courier, Monospace;         background-color: #ffffff;         /*white-space: pre;*/ }  .mycode pre { margin: 0em; }  .mycode .rem { color: #008000; }  .mycode .kwrd { color: #0000ff; }  .mycode .str { color: #a31515; }  .mycode .op { color: #0000c0; }  .mycode .preproc { color: #cc6633; }  .mycode .asp { background-color: #ffff00; }  .mycode .html { color: #800000; }  .mycode .attr { color: #ff0000; }  .mycode .alt  {         background-color: #f4f4f4;         width: 100%;         margin: 0em; }  .mycode .lnum { color: #606060; }
  </style>
  <div class="mycode">
   <pre class="alt"> <span class="lnum">   1: </span><span class="kwrd">class</span> CustomStreamListener(tweepy.StreamListener):  </pre>
   <pre> <span class="lnum">   2: </span>  <span class="str">''</span><span class="str">' </span> </pre>
   <pre class="alt"> <span class="str"><span class="lnum">   3: </span>   Custom class to manage the twitter streaming </span> </pre>
   <pre> <span class="lnum">   4: </span>   '<span class="str">''</span>  </pre>
   <pre class="alt"> <span class="lnum">   5: </span>   def __init__(self):  </pre>
   <pre> <span class="lnum">   6: </span>     super(CustomStreamListener, self).__init__()  </pre>
   <pre class="alt"> <span class="lnum">   7: </span>   </pre>
   <pre> <span class="lnum">   8: </span>   def on_data(self, tweet):  </pre>
   <pre class="alt"> <span class="lnum">   9: </span>     pass  </pre>
   <pre> <span class="lnum">  10: </span>   </pre>
   <pre class="alt"> <span class="lnum">  11: </span>   </pre>
   <pre> <span class="lnum">  12: </span> <span class="kwrd">class</span> MongoDBStreamListener(CustomStreamListener):  </pre>
   <pre class="alt"> <span class="lnum">  13: </span> <span class="str">''</span><span class="str">' class in order to manage the tweet and store into Mongo '</span><span class="str">''</span>  </pre>
   <pre> <span class="lnum">  14: </span>   </pre>
   <pre class="alt"> <span class="lnum">  15: </span>   def __init__(self):  </pre>
   <pre> <span class="lnum">  16: </span>     super(MongoDBStreamListener, self).__init__()  </pre>
   <pre class="alt"> <span class="lnum">  17: </span>   </pre>
   <pre> <span class="lnum">  18: </span>   def on_data(self, tweet):  </pre>
   <pre class="alt"> <span class="lnum">  19: </span>     super(MongoDBStreamListener, self).on_data(tweet)  </pre>
   <pre> <span class="lnum">  20: </span>     json_tweet = json.loads(tweet)  </pre>
   <pre class="alt"> <span class="lnum">  21: </span>     doc = self.process_tweet(json_tweet)  </pre>
   <pre> <span class="lnum">  22: </span>     MongoDBStreamListener.process_all_studies(doc)  </pre>
   <pre class="alt"> <span class="lnum">  23: </span>   </pre>
   <pre> <span class="lnum">  24: </span>   def on_status(self, status):  </pre>
   <pre class="alt"> <span class="lnum">  25: </span>     super(MongoDBStreamListener, self).on_status(status)  </pre>
   <pre> <span class="lnum">  26: </span>   </pre>
   <pre class="alt"> <span class="lnum">  27: </span>   def on_error(self, status_code):  </pre>
   <pre> <span class="lnum">  28: </span>    <span class="str">''</span><span class="str">' </span> </pre>
   <pre class="alt"> <span class="str"><span class="lnum">  29: </span>     don'</span>t kill the stream although an error happened.  </pre>
   <pre> <span class="lnum">  30: </span>    <span class="str">''</span>'  </pre>
   <pre class="alt"> <span class="lnum">  31: </span>     super(MongoDBStreamListener, self).on_error(status_code)  </pre>
   <pre> <span class="lnum">  32: </span>   </pre>
   <pre class="alt"> <span class="lnum">  33: </span>   def on_timeout(self):  </pre>
   <pre> <span class="lnum">  34: </span>     super(MongoDBStreamListener, self).on_timeout()  </pre>
   <pre class="alt"> <span class="lnum">  35: </span>   </pre>
   <pre> <span class="lnum">  36: </span>   def process_tweet(self, tweet):  </pre>
   <pre class="alt"> <span class="lnum">  37: </span>     doc = Tweet()  </pre>
   <pre> <span class="lnum">  38: </span>     doc.created_at = tweet[<span class="str">"created_at"</span>]  </pre>
   <pre class="alt"> <span class="lnum">  39: </span>     doc.favorite_count = tweet[<span class="str">"favorite_count"</span>]  </pre>
   <pre> <span class="lnum">  40: </span>     doc.favorited = tweet[<span class="str">"favorited"</span>]  </pre>
   <pre class="alt"> <span class="lnum">  41: </span>     doc.lang = tweet[<span class="str">"lang"</span>]  </pre>
   <pre> <span class="lnum">  42: </span>     doc.retweet_count = tweet[<span class="str">"retweet_count"</span>]  </pre>
   <pre class="alt"> <span class="lnum">  43: </span>     doc.retweeted = tweet[<span class="str">"retweeted"</span>]  </pre>
   <pre> <span class="lnum">  44: </span>     doc.source = tweet[<span class="str">"source"</span>]  </pre>
   <pre class="alt"> <span class="lnum">  45: </span>     doc.tweet_text = tweet[<span class="str">"text"</span>]  </pre>
   <pre> <span class="lnum">  46: </span>     doc.filter_level = tweet[<span class="str">"filter_level"</span>]  </pre>
   <pre class="alt"> <span class="lnum">  47: </span>     doc.tweet_id = tweet[<span class="str">"id"</span>]  </pre>
   <pre> <span class="lnum">  48: </span>    <span class="kwrd">if</span> tweet[<span class="str">"coordinates"</span>] <span class="kwrd">is</span> not None:  </pre>
   <pre class="alt"> <span class="lnum">  49: </span>       doc.longitude = tweet[<span class="str">"coordinates"</span>][<span class="str">"coordinates"</span>][0]  </pre>
   <pre> <span class="lnum">  50: </span>       doc.latitude = tweet[<span class="str">"coordinates"</span>][<span class="str">"coordinates"</span>][1]  </pre>
   <pre class="alt"> <span class="lnum">  51: </span>     self.save_tweet(doc, tweet)  </pre>
   <pre> <span class="lnum">  52: </span>     MongoDBStreamListener.delay()  </pre>
   <pre class="alt"> <span class="lnum">  53: </span>    <span class="kwrd">return</span> doc  </pre>
   <pre> <span class="lnum">  54: </span>   </pre>
   <pre class="alt"> <span class="lnum">  55: </span>   @staticmethod  </pre>
   <pre> <span class="lnum">  56: </span>   def process_all_studies(tweet):  </pre>
   <pre class="alt"> <span class="lnum">  57: </span>     source_worker = Worker(SourceProcessor(TweetSource), tweet)  </pre>
   <pre> <span class="lnum">  58: </span>     source_worker.start()  </pre>
   <pre class="alt"> <span class="lnum">  59: </span>     lang_worker = Worker(LanguageProcessor(TweetLanguage), tweet)  </pre>
   <pre> <span class="lnum">  60: </span>     lang_worker.start()  </pre>
   <pre class="alt"> <span class="lnum">  61: </span>   </pre>
   <pre> <span class="lnum">  62: </span>   def save_tweet(self, doc, tweet):  </pre>
   <pre class="alt"> <span class="lnum">  63: </span>    <span class="kwrd">if</span> MONGODB_USER <span class="kwrd">is</span> None:  </pre>
   <pre> <span class="lnum">  64: </span>       doc.save() </pre>
  </div>
 </div>
 </div>
 <div class="paragraph" style="text-align:left;">
 <br>
  <span>
  </span>
   When a tweet arrives, MongoDBStreamListener extracts the information from the tweet (it comes in JSON format) and puts it inside a Tweet object:
  <br>
   <span>
   </span>
   <br>
   </br>
  </br>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="344333195186446756" style="width: 100%; overflow-y: hidden;">
  <style type="text/css">
    .mycode, .mycode pre {         font-size: small;         color: black;         font-family: Consolas, "Courier New", Courier, Monospace;         background-color: #ffffff;         /*white-space: pre;*/ }  .mycode pre { margin: 0em; }  .mycode .rem { color: #008000; }  .mycode .kwrd { color: #0000ff; }  .mycode .str { color: #a31515; }  .mycode .op { color: #0000c0; }  .mycode .preproc { color: #cc6633; }  .mycode .asp { background-color: #ffff00; }  .mycode .html { color: #800000; }  .mycode .attr { color: #ff0000; }  .mycode .alt  {         background-color: #f4f4f4;         width: 100%;         margin: 0em; }  .mycode .lnum { color: #606060; }
  </style>
  <div class="mycode">
   <pre class="alt"> <span class="lnum">   1: </span><span class="kwrd">class</span> Tweet(mongoengine.Document):  </pre>
   <pre> <span class="lnum">   2: </span>   created_at = mongoengine.StringField(max_length=200)  </pre>
   <pre class="alt"> <span class="lnum">   3: </span>   tweet_id = mongoengine.IntField(<span class="kwrd">default</span>=-1)  </pre>
   <pre> <span class="lnum">   4: </span>   tweet_text = mongoengine.StringField(max_length=500)  </pre>
   <pre class="alt"> <span class="lnum">   5: </span>   source = mongoengine.StringField(max_length=200)  </pre>
   <pre> <span class="lnum">   6: </span>   retweet_count = mongoengine.IntField(<span class="kwrd">default</span>=0)  </pre>
   <pre class="alt"> <span class="lnum">   7: </span>   favorite_count = mongoengine.IntField(<span class="kwrd">default</span>=0)  </pre>
   <pre> <span class="lnum">   8: </span>   lang = mongoengine.StringField(max_length=5)  </pre>
   <pre class="alt"> <span class="lnum">   9: </span>   favorited = mongoengine.BooleanField(<span class="kwrd">default</span>=False)  </pre>
   <pre> <span class="lnum">  10: </span>   retweeted = mongoengine.BooleanField(<span class="kwrd">default</span>=False)  </pre>
   <pre class="alt"> <span class="lnum">  11: </span>   filter_level = mongoengine.StringField(max_length=60)  </pre>
   <pre> <span class="lnum">  12: </span>   latitude = mongoengine.FloatField(<span class="kwrd">default</span>=None)  </pre>
   <pre class="alt"> <span class="lnum">  13: </span>   longitude = mongoengine.FloatField(<span class="kwrd">default</span>=None)  </pre>
   <pre> <span class="lnum">  14: </span>   </pre>
   <pre class="alt"> <span class="lnum">  15: </span>   def __repr__(self):  </pre>
   <pre> <span class="lnum">  16: </span>    <span class="kwrd">return</span> str(Tweet.source.to_python(<span class="str">'utf-8'</span>)) </pre>
  </div>
 </div>
 </div>
 <div class="paragraph" style="text-align:left;">
  The
 <em>
   process_all_studies()
 </em>
  method will calculate all the studies from the Tweet information. In this case the study of the languages and the study of the sources. For that, a Worker() is launched per each study. A worker needs a Tweet() and TweetProcessor() which needs a model class.
 <span>
  <br>
   <span>
    <br>
     <span>
       This strategy helps us in order to add more studies in the future.
     </span>
    </br>
   </span>
   <br>
    <span>
    </span>
    <br>
     <span>
      <br>
       <span>
        <strong>
          Worker
        </strong>
         :
       </span>
      </br>
     </span>
     <br>
      <span>
      </span>
     </br>
    </br>
   </br>
  </br>
 </span>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="545188268248854941" style="width: 100%; overflow-y: hidden;">
  <style type="text/css">
    .mycode, .mycode pre {         font-size: small;         color: black;         font-family: Consolas, "Courier New", Courier, Monospace;         background-color: #ffffff;         /*white-space: pre;*/ }  .mycode pre { margin: 0em; }  .mycode .rem { color: #008000; }  .mycode .kwrd { color: #0000ff; }  .mycode .str { color: #a31515; }  .mycode .op { color: #0000c0; }  .mycode .preproc { color: #cc6633; }  .mycode .asp { background-color: #ffff00; }  .mycode .html { color: #800000; }  .mycode .attr { color: #ff0000; }  .mycode .alt  {         background-color: #f4f4f4;         width: 100%;         margin: 0em; }  .mycode .lnum { color: #606060; }
  </style>
  <div class="mycode">
   <pre class="alt"> <span class="lnum">   1: </span><span class="kwrd">class</span> Worker(multiprocessing.Process):  </pre>
   <pre> <span class="lnum">   2: </span>  <span class="str">''</span><span class="str">' </span> </pre>
   <pre class="alt"> <span class="str"><span class="lnum">   3: </span>   The class which will generate an independant process in order to process the tweet for </span> </pre>
   <pre> <span class="lnum">   4: </span>   a particular study.  </pre>
   <pre class="alt"> <span class="lnum">   5: </span>   '<span class="str">''</span>  </pre>
   <pre> <span class="lnum">   6: </span>   </pre>
   <pre class="alt"> <span class="lnum">   7: </span>   def __init__(self, tweet_processor, tweet):  </pre>
   <pre> <span class="lnum">   8: </span>    <span class="str">''</span><span class="str">' </span> </pre>
   <pre class="alt"> <span class="str"><span class="lnum">   9: </span>     Child class of processor. It needs a TweetProcessor class in order to be launch in a new process. </span> </pre>
   <pre> <span class="lnum">  10: </span>     '<span class="str">''</span>  </pre>
   <pre class="alt"> <span class="lnum">  11: </span>     super(Worker, self).__init__(target=tweet_processor.run, args=(tweet,))  </pre>
  </div>
 </div>
 </div>
 <div class="paragraph" style="text-align:left;">
 <strong>
  <span>
   <br>
    <span>
    </span>
    <br>
     <span>
     </span>
    </br>
   </br>
  </span>
   Processor of Tweets
 </strong>
  :
 <br>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="888201268219545620" style="width: 100%; overflow-y: hidden;">
  <style type="text/css">
    .mycode, .mycode pre {         font-size: small;         color: black;         font-family: Consolas, "Courier New", Courier, Monospace;         background-color: #ffffff;         /*white-space: pre;*/ }  .mycode pre { margin: 0em; }  .mycode .rem { color: #008000; }  .mycode .kwrd { color: #0000ff; }  .mycode .str { color: #a31515; }  .mycode .op { color: #0000c0; }  .mycode .preproc { color: #cc6633; }  .mycode .asp { background-color: #ffff00; }  .mycode .html { color: #800000; }  .mycode .attr { color: #ff0000; }  .mycode .alt  {         background-color: #f4f4f4;         width: 100%;         margin: 0em; }  .mycode .lnum { color: #606060; }
  </style>
  <div class="mycode">
   <pre class="alt"> <span class="lnum">   1: </span><span class="kwrd">class</span> TweetProcessor(<span class="kwrd">object</span>):  </pre>
   <pre> <span class="lnum">   2: </span>  <span class="str">''</span><span class="str">' </span> </pre>
   <pre class="alt"> <span class="str"><span class="lnum">   3: </span>   Base class in order to process a tweet </span> </pre>
   <pre> <span class="lnum">   4: </span>   '<span class="str">''</span>  </pre>
   <pre class="alt"> <span class="lnum">   5: </span>   </pre>
   <pre> <span class="lnum">   6: </span>   def __init__(self, model):  </pre>
   <pre class="alt"> <span class="lnum">   7: </span>     self.document = None  </pre>
   <pre> <span class="lnum">   8: </span>     self.model = model  </pre>
   <pre class="alt"> <span class="lnum">   9: </span>   </pre>
   <pre> <span class="lnum">  10: </span>   def run(self, tweet):  </pre>
   <pre class="alt"> <span class="lnum">  11: </span>    <span class="str">''</span><span class="str">' </span> </pre>
   <pre> <span class="str"><span class="lnum">  12: </span>     The function which will be launch in the process </span> </pre>
   <pre class="alt"> <span class="lnum">  13: </span>     '<span class="str">''</span>  </pre>
   <pre> <span class="lnum">  14: </span>     self.process(tweet)  </pre>
   <pre class="alt"> <span class="lnum">  15: </span>     self.save()  </pre>
   <pre> <span class="lnum">  16: </span>   </pre>
   <pre class="alt"> <span class="lnum">  17: </span>   def process(self, tweet):  </pre>
   <pre> <span class="lnum">  18: </span>    <span class="str">''</span><span class="str">' </span> </pre>
   <pre class="alt"> <span class="str"><span class="lnum">  19: </span>     Main method of the class. It will have to be implemented by the child classes. </span> </pre>
   <pre> <span class="lnum">  20: </span>     '<span class="str">''</span>  </pre>
   <pre class="alt"> <span class="lnum">  21: </span>     filter_key = self.get_the_key(tweet)  </pre>
   <pre> <span class="lnum">  22: </span>     self.document = self.get_the_document(filter_key)  </pre>
   <pre class="alt"> <span class="lnum">  23: </span>   </pre>
   <pre> <span class="lnum">  24: </span>   def get_the_document(self, filter_key):  </pre>
   <pre class="alt"> <span class="lnum">  25: </span>    <span class="str">''</span><span class="str">' </span> </pre>
   <pre> <span class="str"><span class="lnum">  26: </span>     Retrieve the document </span> </pre>
   <pre class="alt"> <span class="lnum">  27: </span>     '<span class="str">''</span>  </pre>
   <pre> <span class="lnum">  28: </span>     query = self.model.objects(filter_key=filter_key).limit(1)  </pre>
   <pre class="alt"> <span class="lnum">  29: </span>    <span class="kwrd">if</span> len(query) == 0:  </pre>
   <pre> <span class="lnum">  30: </span>       document = self.model()  </pre>
   <pre class="alt"> <span class="lnum">  31: </span>    <span class="kwrd">else</span>:  </pre>
   <pre> <span class="lnum">  32: </span>       document = query[0]  </pre>
   <pre class="alt"> <span class="lnum">  33: </span>    <span class="kwrd">return</span> document  </pre>
   <pre> <span class="lnum">  34: </span>   </pre>
   <pre class="alt"> <span class="lnum">  35: </span>   def save(self):  </pre>
   <pre> <span class="lnum">  36: </span>    <span class="str">''</span><span class="str">' </span> </pre>
   <pre class="alt"> <span class="str"><span class="lnum">  37: </span>     Save into database the document. </span> </pre>
   <pre> <span class="lnum">  38: </span>     '<span class="str">''</span>  </pre>
   <pre class="alt"> <span class="lnum">  39: </span>     self.document.save()  </pre>
   <pre> <span class="lnum">  40: </span>   </pre>
   <pre class="alt"> <span class="lnum">  41: </span>   def get_the_key(self, tweet):  </pre>
   <pre> <span class="lnum">  42: </span>    <span class="str">''</span><span class="str">' </span> </pre>
   <pre class="alt"> <span class="str"><span class="lnum">  43: </span>     Obtain the key from the tweet which will be used as filter </span> </pre>
   <pre> <span class="lnum">  44: </span>     '<span class="str">''</span>  </pre>
   <pre class="alt"> <span class="lnum">  45: </span>     pass  </pre>
   <pre> <span class="lnum">  46: </span>   </pre>
   <pre class="alt"> <span class="lnum">  47: </span>   def update_value(self):  </pre>
   <pre> <span class="lnum">  48: </span>    <span class="str">''</span><span class="str">' </span> </pre>
   <pre class="alt"> <span class="str"><span class="lnum">  49: </span>     Update method for the main value </span> </pre>
   <pre> <span class="lnum">  50: </span>     '<span class="str">''</span>  </pre>
   <pre class="alt"> <span class="lnum">  51: </span>     pass  </pre>
   <pre> <span class="lnum">  52: </span>   </pre>
   <pre class="alt"> <span class="lnum">  53: </span>   </pre>
   <pre> <span class="lnum">  54: </span> <span class="kwrd">class</span> SourceProcessor(TweetProcessor):  </pre>
   <pre class="alt"> <span class="lnum">  55: </span>  <span class="str">''</span><span class="str">' </span> </pre>
   <pre> <span class="str"><span class="lnum">  56: </span>   Class to process the source of the tweet </span> </pre>
   <pre class="alt"> <span class="lnum">  57: </span>   '<span class="str">''</span>  </pre>
   <pre> <span class="lnum">  58: </span>   </pre>
   <pre class="alt"> <span class="lnum">  59: </span>   def __init__(self, tweet):  </pre>
   <pre> <span class="lnum">  60: </span>     super(SourceProcessor, self).__init__(tweet)  </pre>
   <pre class="alt"> <span class="lnum">  61: </span>   </pre>
   <pre> <span class="lnum">  62: </span>   def process(self, tweet):  </pre>
   <pre class="alt"> <span class="lnum">  63: </span>     super(SourceProcessor, self).process(tweet)  </pre>
   <pre> <span class="lnum">  64: </span>    <span class="kwrd">if</span> self.document.filter_key <span class="kwrd">is</span> None:  </pre>
   <pre class="alt"> <span class="lnum">  65: </span>       self.document.filter_key = self.get_the_source(tweet.source)  </pre>
   <pre> <span class="lnum">  66: </span>       self.document.source = tweet.source  </pre>
   <pre class="alt"> <span class="lnum">  67: </span>       self.document.url = self.get_url(tweet.source)  </pre>
   <pre> <span class="lnum">  68: </span>     self.document.last_change = tweet.created_at  </pre>
   <pre class="alt"> <span class="lnum">  69: </span>     self.document.count += 1  </pre>
   <pre> <span class="lnum">  70: </span>     self.update_value()  </pre>
   <pre class="alt"> <span class="lnum">  71: </span>   </pre>
   <pre> <span class="lnum">  72: </span>   def get_the_key(self, tweet):  </pre>
   <pre class="alt"> <span class="lnum">  73: </span>    <span class="str">''</span><span class="str">' </span> </pre>
   <pre> <span class="str"><span class="lnum">  74: </span>     Get the key </span> </pre>
   <pre class="alt"> <span class="lnum">  75: </span>     '<span class="str">''</span>  </pre>
   <pre> <span class="lnum">  76: </span>     source = tweet.source  </pre>
   <pre class="alt"> <span class="lnum">  77: </span>    <span class="kwrd">return</span> self.get_the_source(source)  </pre>
   <pre> <span class="lnum">  78: </span>   </pre>
   <pre class="alt"> <span class="lnum">  79: </span>   def get_the_source(self, source):  </pre>
   <pre> <span class="lnum">  80: </span>    <span class="str">''</span><span class="str">' </span> </pre>
   <pre class="alt"> <span class="str"><span class="lnum">  81: </span>     Get the source from the tweet </span> </pre>
   <pre> <span class="lnum">  82: </span>     '<span class="str">''</span>  </pre>
   <pre class="alt"> <span class="lnum">  83: </span>     key = source.split(<span class="str">"&amp;gt;"</span>)[1].split(<span class="str">"&amp;lt;"</span>)[0].strip()  </pre>
   <pre> <span class="lnum">  84: </span>    <span class="kwrd">return</span> key.encode(<span class="str">"utf-8"</span>)  </pre>
   <pre class="alt"> <span class="lnum">  85: </span>   </pre>
   <pre> <span class="lnum">  86: </span>   def update_value(self):  </pre>
   <pre class="alt"> <span class="lnum">  87: </span>    <span class="str">''</span><span class="str">' </span> </pre>
   <pre> <span class="str"><span class="lnum">  88: </span>     Update method for the main value </span> </pre>
   <pre class="alt"> <span class="lnum">  89: </span>     '<span class="str">''</span>  </pre>
   <pre> <span class="lnum">  90: </span>     self.document.value += 1  </pre>
   <pre class="alt"> <span class="lnum">  91: </span>   </pre>
   <pre> <span class="lnum">  92: </span>   def get_url(self, source):  </pre>
   <pre class="alt"> <span class="lnum">  93: </span>     key = source.split(<span class="str">"rel="</span>)[0].split(<span class="str">"href="</span>)[1].split(<span class="str">"\""</span>)  </pre>
   <pre> <span class="lnum">  94: </span>    <span class="kwrd">return</span> key[1].encode(<span class="str">"utf-8"</span>)  </pre>
   <pre class="alt"> <span class="lnum">  95: </span>   </pre>
   <pre> <span class="lnum">  96: </span>   </pre>
   <pre class="alt"> <span class="lnum">  97: </span> <span class="kwrd">class</span> LanguageProcessor(TweetProcessor):  </pre>
   <pre> <span class="lnum">  98: </span>  <span class="str">''</span><span class="str">' </span> </pre>
   <pre class="alt"> <span class="str"><span class="lnum">  99: </span>   Class to process the language of the tweet </span> </pre>
   <pre> <span class="lnum"> 100: </span>   '<span class="str">''</span>  </pre>
   <pre class="alt"> <span class="lnum"> 101: </span>   </pre>
   <pre> <span class="lnum"> 102: </span>   def __init__(self, tweet):  </pre>
   <pre class="alt"> <span class="lnum"> 103: </span>     super(LanguageProcessor, self).__init__(tweet)  </pre>
   <pre> <span class="lnum"> 104: </span>   </pre>
   <pre class="alt"> <span class="lnum"> 105: </span>   def process(self, tweet):  </pre>
   <pre> <span class="lnum"> 106: </span>     super(LanguageProcessor, self).process(tweet)  </pre>
   <pre class="alt"> <span class="lnum"> 107: </span>    <span class="kwrd">if</span> self.document.filter_key <span class="kwrd">is</span> None:  </pre>
   <pre> <span class="lnum"> 108: </span>       self.document.filter_key = self.get_the_language(tweet.lang)  </pre>
   <pre class="alt"> <span class="lnum"> 109: </span>       self.document.language = tweet.source  </pre>
   <pre> <span class="lnum"> 110: </span>     self.document.last_change = tweet.created_at  </pre>
   <pre class="alt"> <span class="lnum"> 111: </span>     self.document.count += 1  </pre>
   <pre> <span class="lnum"> 112: </span>     self.update_value()  </pre>
   <pre class="alt"> <span class="lnum"> 113: </span>   </pre>
   <pre> <span class="lnum"> 114: </span>   def get_the_key(self, tweet):  </pre>
   <pre class="alt"> <span class="lnum"> 115: </span>    <span class="str">''</span><span class="str">' </span> </pre>
   <pre> <span class="str"><span class="lnum"> 116: </span>     Get the key </span> </pre>
   <pre class="alt"> <span class="lnum"> 117: </span>     '<span class="str">''</span>  </pre>
   <pre> <span class="lnum"> 118: </span>     key = tweet.lang  </pre>
   <pre class="alt"> <span class="lnum"> 119: </span>    <span class="kwrd">return</span> self.get_the_language(key)  </pre>
   <pre> <span class="lnum"> 120: </span>   </pre>
   <pre class="alt"> <span class="lnum"> 121: </span>   def get_the_language(self, lang):  </pre>
   <pre> <span class="lnum"> 122: </span>    <span class="str">''</span><span class="str">' </span> </pre>
   <pre class="alt"> <span class="str"><span class="lnum"> 123: </span>     Get the Language from the tweet </span> </pre>
   <pre> <span class="lnum"> 124: </span>     '<span class="str">''</span>  </pre>
   <pre class="alt"> <span class="lnum"> 125: </span>     language = lang.encode(<span class="str">"utf-8"</span>)  </pre>
   <pre> <span class="lnum"> 126: </span>    <span class="kwrd">return</span> language  </pre>
   <pre class="alt"> <span class="lnum"> 127: </span>   </pre>
   <pre> <span class="lnum"> 128: </span>   def update_value(self):  </pre>
   <pre class="alt"> <span class="lnum"> 129: </span>    <span class="str">''</span><span class="str">' </span> </pre>
   <pre> <span class="str"><span class="lnum"> 130: </span>     Update method for the main value </span> </pre>
   <pre class="alt"> <span class="lnum"> 131: </span>     '<span class="str">''</span>  </pre>
   <pre> <span class="lnum"> 132: </span>     self.document.value += 1 </pre>
  </div>
 </div>
 </div>
 <div class="paragraph" style="text-align:left;">
  As you can see, the current study is to count the number of languages and sources.
 <br>
  <span>
   <br>
    <span>
     <span>
      <br>
       <span>
        <strong>
          Model (for MongoDB)
        </strong>
         :
        <br/>
       </span>
      </br>
     </span>
    </span>
   </br>
  </span>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="844854037270112729" style="width: 100%; overflow-y: hidden;">
  <style type="text/css">
    .mycode, .mycode pre {         font-size: small;         color: black;         font-family: Consolas, "Courier New", Courier, Monospace;         background-color: #ffffff;         /*white-space: pre;*/ }  .mycode pre { margin: 0em; }  .mycode .rem { color: #008000; }  .mycode .kwrd { color: #0000ff; }  .mycode .str { color: #a31515; }  .mycode .op { color: #0000c0; }  .mycode .preproc { color: #cc6633; }  .mycode .asp { background-color: #ffff00; }  .mycode .html { color: #800000; }  .mycode .attr { color: #ff0000; }  .mycode .alt  {         background-color: #f4f4f4;         width: 100%;         margin: 0em; }  .mycode .lnum { color: #606060; }
  </style>
  <div class="mycode">
   <pre class="alt"> <span class="lnum">   1: </span><span class="kwrd">class</span> TweetSource(mongoengine.Document):  </pre>
   <pre> <span class="lnum">   2: </span>   source = mongoengine.StringField(max_length=200, <span class="kwrd">default</span>=None)  </pre>
   <pre class="alt"> <span class="lnum">   3: </span>   filter_key = mongoengine.StringField(max_length=200, <span class="kwrd">default</span>=None)  </pre>
   <pre> <span class="lnum">   4: </span>   count = mongoengine.IntField(<span class="kwrd">default</span>=0)  </pre>
   <pre class="alt"> <span class="lnum">   5: </span>   last_change = mongoengine.StringField()  </pre>
   <pre> <span class="lnum">   6: </span>   value = mongoengine.IntField(<span class="kwrd">default</span>=0)  </pre>
   <pre class="alt"> <span class="lnum">   7: </span>   url = mongoengine.StringField(max_length=200, <span class="kwrd">default</span>=<span class="str">"http://www.twitter.com"</span>)  </pre>
   <pre> <span class="lnum">   8: </span>   </pre>
   <pre class="alt"> <span class="lnum">   9: </span>   meta = {  </pre>
   <pre> <span class="lnum">  10: </span>    <span class="str">'indexes'</span>: [<span class="str">'filter_key'</span>, (<span class="str">'filter_key'</span>, <span class="str">'-value'</span>)],  </pre>
   <pre class="alt"> <span class="lnum">  11: </span>    <span class="str">'ordering'</span>: [<span class="str">'-value'</span>]  </pre>
   <pre> <span class="lnum">  12: </span>   }  </pre>
   <pre class="alt"> <span class="lnum">  13: </span>   </pre>
   <pre> <span class="lnum">  14: </span>   </pre>
   <pre class="alt"> <span class="lnum">  15: </span> <span class="kwrd">class</span> TweetLanguage(mongoengine.Document):  </pre>
   <pre> <span class="lnum">  16: </span>   language = mongoengine.StringField(max_length=209, <span class="kwrd">default</span>=None)  </pre>
   <pre class="alt"> <span class="lnum">  17: </span>   filter_key = mongoengine.StringField(max_length=209, <span class="kwrd">default</span>=None)  </pre>
   <pre> <span class="lnum">  18: </span>   count = mongoengine.IntField(<span class="kwrd">default</span>=0)  </pre>
   <pre class="alt"> <span class="lnum">  19: </span>   last_change = mongoengine.StringField()  </pre>
   <pre> <span class="lnum">  20: </span>   value = mongoengine.IntField(<span class="kwrd">default</span>=0)  </pre>
   <pre class="alt"> <span class="lnum">  21: </span>   </pre>
   <pre> <span class="lnum">  22: </span>   meta = {  </pre>
   <pre class="alt"> <span class="lnum">  23: </span>    <span class="str">'indexes'</span>: [<span class="str">'filter_key'</span>, (<span class="str">'filter_key'</span>, <span class="str">'-value'</span>)],  </pre>
   <pre> <span class="lnum">  24: </span>    <span class="str">'ordering'</span>: [<span class="str">'-value'</span>]  </pre>
   <pre class="alt"> <span class="lnum">  25: </span>   } </pre>
  </div>
 </div>
 </div>
 <div class="paragraph" style="text-align:left;">
 <br>
  <span>
  </span>
  <br>
   <span>
   </span>
    After having all the structure we have to connect to Twitter:
   <br>
    <span>
    </span>
    <br>
    </br>
   </br>
  </br>
 </br>
 </div>
 <div>
 <div align="left" class="wcustomhtml" id="283265378995516922" style="width: 100%; overflow-y: hidden;">
  <style type="text/css">
    .mycode, .mycode pre {         font-size: small;         color: black;         font-family: Consolas, "Courier New", Courier, Monospace;         background-color: #ffffff;         /*white-space: pre;*/ }  .mycode pre { margin: 0em; }  .mycode .rem { color: #008000; }  .mycode .kwrd { color: #0000ff; }  .mycode .str { color: #a31515; }  .mycode .op { color: #0000c0; }  .mycode .preproc { color: #cc6633; }  .mycode .asp { background-color: #ffff00; }  .mycode .html { color: #800000; }  .mycode .attr { color: #ff0000; }  .mycode .alt  {         background-color: #f4f4f4;         width: 100%;         margin: 0em; }  .mycode .lnum { color: #606060; }
  </style>
  <div class="mycode">
   <pre class="alt"> <span class="lnum">   1: </span><span class="kwrd">class</span> TwitterStreaming(<span class="kwrd">object</span>):  </pre>
   <pre> <span class="lnum">   2: </span>  <span class="str">''</span><span class="str">' </span> </pre>
   <pre class="alt"> <span class="str"><span class="lnum">   3: </span>   Twitter Streaming </span> </pre>
   <pre> <span class="lnum">   4: </span>   '<span class="str">''</span>  </pre>
   <pre class="alt"> <span class="lnum">   5: </span>   </pre>
   <pre> <span class="lnum">   6: </span>   def __init__(self, stream_listener=MongoDBStreamListener, auth=TwitterAuth):  </pre>
   <pre class="alt"> <span class="lnum">   7: </span>     self.twitter_auth = auth()  </pre>
   <pre> <span class="lnum">   8: </span>     self.stream_listener = stream_listener  </pre>
   <pre class="alt"> <span class="lnum">   9: </span>   </pre>
   <pre> <span class="lnum">  10: </span>   </pre>
   <pre class="alt"> <span class="lnum">  11: </span>   def run(self, tracking=None, locations=None):  </pre>
   <pre> <span class="lnum">  12: </span>    <span class="str">''</span><span class="str">' </span> </pre>
   <pre class="alt"> <span class="str"><span class="lnum">  13: </span>     Run the streaming </span> </pre>
   <pre> <span class="lnum">  14: </span>   </pre>
   <pre class="alt"> <span class="lnum">  15: </span>     tracking: list of topics  </pre>
   <pre> <span class="lnum">  16: </span>     '<span class="str">''</span>  </pre>
   <pre class="alt"> <span class="lnum">  17: </span>   </pre>
   <pre> <span class="lnum">  18: </span>     sapi = tweepy.streaming.Stream(self.twitter_auth.get_auth(), self.stream_listener())  </pre>
   <pre class="alt"> <span class="lnum">  19: </span>     sapi.filter(track=tracking, locations=[]) </pre>
  </div>
 </div>
 </div>
 <div class="paragraph" style="text-align:left;">
 <br>
  <span>
  </span>
  <br>
   <span>
     Now, we only need to query the data and show it:
    <span>
     <br>
      <a href="http://talkingbigdata-dollbox.rhcloud.com/" target="_blank">
       <span>
         http://talkingbigdata-dollbox.rhcloud.com
       </span>
      </a>
      <br>
       <span>
       </span>
      </br>
     </br>
    </span>
   </span>
  </br>
 </br>
 </div>
 <div>
 <div style="height:20px;overflow:hidden">
 </div>
 <div id="396008649661628026-slideshow">
 </div>
 <script type="text/javascript">
   (function(jQuery) { function init() { wSlideshow.render({elementID:"396008649661628026",nav:"thumbnails",navLocation:"right",captionLocation:"bottom",transition:"fade",autoplay:"0",speed:"5",aspectRatio:"auto",showControls:"true",randomStart:"false",images:[{"url":"3/9/1/8/39182937/3383161.png","width":"400","height":"208"},{"url":"3/9/1/8/39182937/458546.png","width":"400","height":"208"},{"url":"3/9/1/8/39182937/4526492.png","width":"400","height":"208"}]}) } jQuery ? jQuery(init) : document.observe('dom:loaded', init) })(window._W && _W.jQuery)
 </script>
 <div style="height:20px;overflow:hidden">
 </div>
 </div>
 <div class="paragraph" style="text-align:left;">
 <br>
  <span>
  </span>
  <br>
   <strong>
    <span>
    </span>
     Conclusion
   </strong>
    :
   <br>
    <span>
     <br>
      <span>
      </span>
       Openshift is a very good option when you want to try a PaaS for free. The only problem is that openshift stops your server if you don't receive a request for a long time, but I need to remember that my account is free.
      <br>
       <span>
        <span>
         <br>
          <span>
            Python was a very good option because it is easy and it has a lot of libraries which help you a lot, for example tweepy.
           <br>
            <span>
             <br>
              <span>
                MongoDB was another good option because is so simple to use when your data does not have relations. Besides, the Twitter's data come in json that is the format that
              </span>
             </br>
            </span>
           </br>
          </span>
         </br>
        </span>
         MongoDB uses for its Documents.
        <br>
         <span>
          <br>
           <span>
             Django is a very powerful framework but it was not key for this project. Moreover, Django-ORM does not have support for MongoDB and I missed a lot of things that Django gives you for free; hence, I had to work
            <a href="http://mongoengine.org/" target="_blank" title="">
              MongoEngine
            </a>
             which is a very good library and it
           </span>
          </br>
         </span>
          is like working with Django-ORM or SQLAlchemy. Therefore, after all, I would have used
         <a href="http://flask.pocoo.org/" target="_blank" title="">
           Flask
         </a>
          for this project.
         <br>
          <span>
           <br>
            <span>
              This project is in
             <a href="https://github.com/mendrugory/tweetsanalyzer" target="_blank" title="">
               github
             </a>
              .
            </span>
           </br>
          </span>
          <br>
           <span>
           </span>
          </br>
         </br>
        </br>
       </span>
      </br>
     </br>
    </span>
   </br>
  </br>
 </br>
 </div>
</div>
