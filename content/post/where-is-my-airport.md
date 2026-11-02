---
title: "Where is my Airport?"
date: 2014-10-26
url: /post/where-is-my-airport
index: true
---

I have to say that I love maps, and for a long time I have wanted to try out two complementary tools: [Leaflet](http://leafletjs.com/) maps and NoSQL geo-functions. So I decided to build a location app that helps people find their closest airports.

I evaluated [ArangoDB](http://docs.arangodb.org/SimpleQueries/GeoQueries.html) and [MongoDB](http://docs.mongodb.org/manual/applications/geospatial-indexes/) as NoSQL databases. ArangoDB worked very well, but its Python driver was not as good as I expected, so I chose MongoDB.

Once the app is finished, running it returns the 10 airports closest to me:

![Where is my airport](/img/4356943_orig.png)

## Backend

The first task was finding all the data related to airports. It was very easy thanks to [Openflights](http://openflights.org/), which has a lot of information about flights, airports and so on.

Once I had the data, I needed to reformat it so that MongoDB could import it. The most important part was transforming the given latitude and longitude into the [GeoJson Point format](http://geojson.org/geojson-spec.html#id2).

```json
{ "type": "Point", "coordinates": [100.0, 0.0] }
```

The *"coordinates"* field has to contain the longitude first and then the latitude.

MongoDB's geo-function *$near* returns a list of the stored locations (documents) sorted by distance to a given location. However, to take advantage of this functionality, a geo-index is needed:

```javascript
db.collection_name.ensureIndex({"loc":"2dsphere"})
```

Once all our data was ready, I used [Flask](http://flask.pocoo.org/) as the backend technology and [MongoEngine](http://mongoengine.org/) as the Object-Document Mapper. I tried to use the near function implemented in MongoEngine, but I could not get it to work, so I used the raw-query functionality instead.

models.py:

```python
from settings import db


class Airport(db.Document):
    city = db.StringField()
    name = db.StringField()
    country = db.StringField()
    iata_faa = db.StringField(default=None)
    tz = db.StringField(default=None)
    icao = db.StringField(default=None)
    dst = db.StringField(default=None)
    loc = db.PointField(default=None)

    @staticmethod
    def get_closest_airports(lonlat, limit=10):
        """
        Using a raw query it returns the "limit" closest airports.

        :param latlon list:
        :return list of airports:
        """
        return Airport.objects(
            __raw__={"loc": {"$near": {"$geometry": {"type": "Point", "coordinates": lonlat}}}}).limit(limit)

    def __dict__(self):
        return {
            "city": self.city,
            "name": self.name,
            "country": self.country,
            "lat": self.loc["coordinates"].pop(),
            "lon": self.loc["coordinates"].pop()
        }
```

## Frontend

Then I started working with the Leaflet library to locate the user and display the locations.

```javascript
function customMark(lat, lon, message, icon)
{
    var marker = L.marker([lat, lon], {icon: icon}).addTo(map)
    if (message != null)
    {
        marker.bindPopup(message);
    }
    return marker
}

function onLocationFound(e)
{
    var radius = e.accuracy / 2;

    L.marker(e.latlng).addTo(map)
    .bindPopup("You are within " + radius + " meters from this point").openPopup();

    L.circle(e.latlng, radius).addTo(map);
}

function markAirports(airports)
{
    var airportIcon = L.icon({
    iconUrl: 'http://excellencyairportlimousine.com/pics/airport.png',
    iconSize: [50, 50],
    iconAnchor: [25, 25],
    popupAnchor: [-3, -76],

    });

    for(var i = 0; i < airports.length; i++)
    {
        var airport = airports[i];
        var city = airport["city"];
        var name = airport["name"];
        var lat = airport["lat"];
        var lon = airport["lon"];
        customMark(lat, lon, name, airportIcon);
    }
}

function onLocationClosest(e)
{
    var handleResponse = function (status, response) {
        alert(response)
    }

    var handleStateChange = function(xmlhttp)
    {
        return  (xmlhttp.readyState == 4) ? handleResponse(xmlhttp.status, xmlhttp.response) : null;
    }

    var xmlhttp = new XMLHttpRequest();
    xmlhttp.open("POST", "/", false);
    xmlhttp.setRequestHeader('Content-Type', 'application/json');
    xmlhttp.onreadystatechange = handleStateChange(xmlhttp);

    seen = [];
    xmlhttp.send(JSON.stringify(e,function(key, val)
    {
        if (val != null && typeof val == "object") {
            if (seen.indexOf(val) >= 0)
                return
            seen.push(val)
        }
        return val
    }));

    var response = JSON.parse(xmlhttp.response);
    markAirportsAndYourPosition(response, e);
}

function closestAirports()
{
    map.on('locationfound', onLocationClosest);
    map.locate();
}
```

## Conclusion

I really enjoyed this project. The Flask + MongoDB + Leaflet combination was a very good choice. I hope to use ArangoDB in the future.

I would like to thank Openflights again. Without their data, this PoC would not have been possible.

The app should be running at [http://airports-dollbox.rhcloud.com/](http://airports-dollbox.rhcloud.com/).

The project is on [github](https://github.com/mendrugory/Airports).
