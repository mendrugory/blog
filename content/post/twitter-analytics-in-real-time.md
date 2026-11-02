---
title: "Twitter Analytics in real time"
date: 2014-09-12
url: /post/twitter-analytics-in-real-time
index: true
---

Real-time analytics is very trendy. Nowadays everyone wants to analyze the data they have, even if that data is not very useful, and they want the results as soon as possible, even when they could wait for them.

Social networks are among the most powerful data sources, and within this group, Twitter stands out. So I decided to build a system (as a proof of concept) to analyze tweets related to Big Data.

First of all, I decided that the app would be deployed on [openshift](https://www.openshift.com/products?gclid=CPP9nbWU3MACFdSWtAodG1IAOA). Openshift is a PaaS product from Red Hat, and I like it because of its flexibility. [Python](http://www.python.org) was the chosen language, [Django](https://www.djangoproject.com/) the chosen framework and [MongoDB](http://www.mongodb.org/) the chosen database.

If you want to interact with Twitter, you need to register your app to get Twitter credentials for it. So I visited the [Twitter Developers](https://apps.twitter.com/) website and registered my app, talkingbigdata:

![Picture](/img/1559413_orig.png)

The most important parts are:

- *API key*
- *API secret*
- *Access token*
- *Access token secret*

Once we have registered the app and obtained the credentials, let's code.

The first part is connecting to the Twitter Streaming API, and for that I used [tweepy](http://www.tweepy.org/).

**Authorization**:

```python
class TwitterAuth(object):
    '''
    class to manage the twitter authorization
    '''

    def __init__(self):
        self.auth = tweepy.OAuthHandler(secret.CONSUMER_KEY, secret.CONSUMER_SECRET)
        self.auth.set_access_token(secret.ACCESS_TOKEN, secret.ACCESS_TOKEN_SECRET)
        self.api = tweepy.API(self.auth)

    def get_auth(self):
        return self.auth

    def get_api(self):
        return self.api
```

Tweepy provides a very useful class, StreamListener(), which manages the Twitter stream. I created a CustomStreamListener class, which inherits from StreamListener and contains the common logic, and a MongoDBStreamListener class, which inherits from CustomStreamListener and does the MongoDB-specific work. This gives me a structure where I can add other databases, or even other things, later.

**Stream Listener**:

```python
class CustomStreamListener(tweepy.StreamListener):
    '''
    Custom class to manage the twitter streaming
    '''
    def __init__(self):
        super(CustomStreamListener, self).__init__()

    def on_data(self, tweet):
        pass


class MongoDBStreamListener(CustomStreamListener):
    ''' class in order to manage the tweet and store into Mongo '''

    def __init__(self):
        super(MongoDBStreamListener, self).__init__()

    def on_data(self, tweet):
        super(MongoDBStreamListener, self).on_data(tweet)
        json_tweet = json.loads(tweet)
        doc = self.process_tweet(json_tweet)
        MongoDBStreamListener.process_all_studies(doc)

    def on_status(self, status):
        super(MongoDBStreamListener, self).on_status(status)

    def on_error(self, status_code):
        '''
        don't kill the stream although an error happened.
        '''
        super(MongoDBStreamListener, self).on_error(status_code)

    def on_timeout(self):
        super(MongoDBStreamListener, self).on_timeout()

    def process_tweet(self, tweet):
        doc = Tweet()
        doc.created_at = tweet["created_at"]
        doc.favorite_count = tweet["favorite_count"]
        doc.favorited = tweet["favorited"]
        doc.lang = tweet["lang"]
        doc.retweet_count = tweet["retweet_count"]
        doc.retweeted = tweet["retweeted"]
        doc.source = tweet["source"]
        doc.tweet_text = tweet["text"]
        doc.filter_level = tweet["filter_level"]
        doc.tweet_id = tweet["id"]
        if tweet["coordinates"] is not None:
            doc.longitude = tweet["coordinates"]["coordinates"][0]
            doc.latitude = tweet["coordinates"]["coordinates"][1]
        self.save_tweet(doc, tweet)
        MongoDBStreamListener.delay()
        return doc

    @staticmethod
    def process_all_studies(tweet):
        source_worker = Worker(SourceProcessor(TweetSource), tweet)
        source_worker.start()
        lang_worker = Worker(LanguageProcessor(TweetLanguage), tweet)
        lang_worker.start()

    def save_tweet(self, doc, tweet):
        if MONGODB_USER is None:
            doc.save()
```

When a tweet arrives (in JSON format), MongoDBStreamListener extracts its information and puts it into a Tweet object:

```python
class Tweet(mongoengine.Document):
    created_at = mongoengine.StringField(max_length=200)
    tweet_id = mongoengine.IntField(default=-1)
    tweet_text = mongoengine.StringField(max_length=500)
    source = mongoengine.StringField(max_length=200)
    retweet_count = mongoengine.IntField(default=0)
    favorite_count = mongoengine.IntField(default=0)
    lang = mongoengine.StringField(max_length=5)
    favorited = mongoengine.BooleanField(default=False)
    retweeted = mongoengine.BooleanField(default=False)
    filter_level = mongoengine.StringField(max_length=60)
    latitude = mongoengine.FloatField(default=None)
    longitude = mongoengine.FloatField(default=None)

    def __repr__(self):
        return str(Tweet.source.to_python('utf-8'))
```

The *process_all_studies()* method calculates all the studies from the Tweet information; in this case, the study of languages and the study of sources. To do that, a Worker() is launched for each study. A worker needs a Tweet() and a TweetProcessor(), which in turn needs a model class.

This strategy makes it easy to add more studies in the future.

**Worker**:

```python
class Worker(multiprocessing.Process):
    '''
    The class which will generate an independant process in order to process the tweet for
    a particular study.
    '''

    def __init__(self, tweet_processor, tweet):
        '''
        Child class of processor. It needs a TweetProcessor class in order to be launch in a new process.
        '''
        super(Worker, self).__init__(target=tweet_processor.run, args=(tweet,))
```

**Processor of Tweets**:

```python
class TweetProcessor(object):
    '''
    Base class in order to process a tweet
    '''

    def __init__(self, model):
        self.document = None
        self.model = model

    def run(self, tweet):
        '''
        The function which will be launch in the process
        '''
        self.process(tweet)
        self.save()

    def process(self, tweet):
        '''
        Main method of the class. It will have to be implemented by the child classes.
        '''
        filter_key = self.get_the_key(tweet)
        self.document = self.get_the_document(filter_key)

    def get_the_document(self, filter_key):
        '''
        Retrieve the document
        '''
        query = self.model.objects(filter_key=filter_key).limit(1)
        if len(query) == 0:
            document = self.model()
        else:
            document = query[0]
        return document

    def save(self):
        '''
        Save into database the document.
        '''
        self.document.save()

    def get_the_key(self, tweet):
        '''
        Obtain the key from the tweet which will be used as filter
        '''
        pass

    def update_value(self):
        '''
        Update method for the main value
        '''
        pass


class SourceProcessor(TweetProcessor):
    '''
    Class to process the source of the tweet
    '''

    def __init__(self, tweet):
        super(SourceProcessor, self).__init__(tweet)

    def process(self, tweet):
        super(SourceProcessor, self).process(tweet)
        if self.document.filter_key is None:
            self.document.filter_key = self.get_the_source(tweet.source)
            self.document.source = tweet.source
            self.document.url = self.get_url(tweet.source)
        self.document.last_change = tweet.created_at
        self.document.count += 1
        self.update_value()

    def get_the_key(self, tweet):
        '''
        Get the key
        '''
        source = tweet.source
        return self.get_the_source(source)

    def get_the_source(self, source):
        '''
        Get the source from the tweet
        '''
        key = source.split("&gt;")[1].split("&lt;")[0].strip()
        return key.encode("utf-8")

    def update_value(self):
        '''
        Update method for the main value
        '''
        self.document.value += 1

    def get_url(self, source):
        key = source.split("rel=")[0].split("href=")[1].split("\"")
        return key[1].encode("utf-8")


class LanguageProcessor(TweetProcessor):
    '''
    Class to process the language of the tweet
    '''

    def __init__(self, tweet):
        super(LanguageProcessor, self).__init__(tweet)

    def process(self, tweet):
        super(LanguageProcessor, self).process(tweet)
        if self.document.filter_key is None:
            self.document.filter_key = self.get_the_language(tweet.lang)
            self.document.language = tweet.source
        self.document.last_change = tweet.created_at
        self.document.count += 1
        self.update_value()

    def get_the_key(self, tweet):
        '''
        Get the key
        '''
        key = tweet.lang
        return self.get_the_language(key)

    def get_the_language(self, lang):
        '''
        Get the Language from the tweet
        '''
        language = lang.encode("utf-8")
        return language

    def update_value(self):
        '''
        Update method for the main value
        '''
        self.document.value += 1
```

As you can see, the current studies count tweets by language and by source.

**Model (for MongoDB)**:

```python
class TweetSource(mongoengine.Document):
    source = mongoengine.StringField(max_length=200, default=None)
    filter_key = mongoengine.StringField(max_length=200, default=None)
    count = mongoengine.IntField(default=0)
    last_change = mongoengine.StringField()
    value = mongoengine.IntField(default=0)
    url = mongoengine.StringField(max_length=200, default="http://www.twitter.com")

    meta = {
        'indexes': ['filter_key', ('filter_key', '-value')],
        'ordering': ['-value']
    }


class TweetLanguage(mongoengine.Document):
    language = mongoengine.StringField(max_length=209, default=None)
    filter_key = mongoengine.StringField(max_length=209, default=None)
    count = mongoengine.IntField(default=0)
    last_change = mongoengine.StringField()
    value = mongoengine.IntField(default=0)

    meta = {
        'indexes': ['filter_key', ('filter_key', '-value')],
        'ordering': ['-value']
    }
```

With the whole structure in place, we just have to connect to Twitter:

```python
class TwitterStreaming(object):
    '''
    Twitter Streaming
    '''

    def __init__(self, stream_listener=MongoDBStreamListener, auth=TwitterAuth):
        self.twitter_auth = auth()
        self.stream_listener = stream_listener


    def run(self, tracking=None, locations=None):
        '''
        Run the streaming

        tracking: list of topics
        '''

        sapi = tweepy.streaming.Stream(self.twitter_auth.get_auth(), self.stream_listener())
        sapi.filter(track=tracking, locations=[])
```

Now we only need to query the data and show it: [http://talkingbigdata-dollbox.rhcloud.com](http://talkingbigdata-dollbox.rhcloud.com/)

## Conclusion

Openshift is a very good option when you want to try a PaaS for free. The only problem is that Openshift stops your server if it doesn't receive any requests for a long time, but I have to remember that my account is free.

Python was a very good choice because it is easy and has a lot of libraries that help you a great deal, for example tweepy.

MongoDB was another good choice because it is very simple to use when your data has no relations. Besides, Twitter's data comes in JSON, which is the format MongoDB uses for its documents.

Django is a very powerful framework, but it was not key to this project. Moreover, the Django ORM does not support MongoDB, and I missed many of the things Django gives you for free, so I had to work with [MongoEngine](http://mongoengine.org/), which is a very good library and feels like working with the Django ORM or SQLAlchemy. All things considered, I would have used [Flask](http://flask.pocoo.org/) for this project.

The project is on [github](https://github.com/mendrugory/tweetsanalyzer).
