---
layout: post
title: Geofence Application&#58; Datastore Logging 
---

We can now use the FlowCloud Datastore API to log our GPS history to the Cloud.
FlowCloud provides DataStores for both users and devices, in this case we will use the device's DataStore, as it is the device that we are logging.

First we will define a small class at the top of the sketch to wrap the FlowDataStore, calling this class DataStore.
This class significantly simplifies access to the FlowDataStore and wraps the C APIs in a C++ object-oriented design.
The class is also re-usable and would easily allow us to access multiple datastores

{% highlight c++ %}
// The Datastore class wraps the Flow datastore in a C++ object and
// allows us easy access to load, clear and to save a XML node
// This class is implemented in datastore.pde
class DataStore
{
public:
    DataStore(char *name);
    bool load();
    bool clear(char *string);
    bool save(XMLNode &node);
private:
    char *name;
    FlowDataStore _datastore;
}; 
{% endhighlight %}
The implementation, datastore.pde can be found [here on GitHub](https://github.com/IMG-FlowCloud/geofence/blob/master/datastore.pde).
A second tab within MPIDE is used to refactor out this functionality. 

This implementation uses the [device datastore API](http://uploads.flowworld.com/libflowcore/docs/c/2.0/da/d26/group__DataStores.html#ga553022e551c0d47837a815654ab52f59) as it is designed to run on a device however it could be modified to support both device and user datastores. It is also missing retrieval functions, which can be added as needed. 

We will also need to add
{% highlight c++ %}
#include <XMLNode.h>
{% endhighlight %}

at the very top of our file so we can use XMLNode.

We can then access a Flow DataStore by creating a DataStore object for it.
{% highlight c++ %}
// Access to the Flow datastore named "GPSReading"
DataStore datastore("GPSReading");
{% endhighlight %}

As the datastore takes `XMLNode`s to save, we need to be able to create a XMLNode representation of the data retrieved from the GPS.

We can serialise the GPS data to xml in the following structure.
{% highlight xml %}
<gpsreading>
    <gpsreadingtime type="DateTime">
        2015-01-29T05:08:16Z
    </gpsreadingtime>
    <location>
        <latitude>-41.213381667</latitude>
        <longitude>174.899859999</longitude>
    </location>
    <readingage>0</readingage>
    <satellites>6</satellites>
    <altitude>7.200</altitude>
    <speed>0.0</speed>
    <course>331.790</course>
    <hdop>350.0</hdop>
</gpsreading>
{% endhighlight %}

The code to create this entity follows

{% highlight c++ %}
// read the current GPS location from the NMEA library to a XML node
void readGPSToXML(XMLNode &xml)
{
    // create a sting for the current datetime
    #define DATETIME_FIELD_LENGTH 32
    char datetimeStr[DATETIME_FIELD_LENGTH];
    time_t currentDateTimeSeconds;
    Flow_GetTime(&currentDateTimeSeconds);
    struct tm *currentDateTimeUTC = gmtime(&currentDateTimeSeconds);
    strftime(   datetimeStr, 
                DATETIME_FIELD_LENGTH, 
                "%Y-%m-%dT%H:%M:%SZ", 
                currentDateTimeUTC
             );

    XMLNode &readingTime = xml.addChild("gpsreadingtime");
    readingTime.addAttribute("type", "datetime");
    readingTime.addAttribute("index", "true");
    readingTime.setContent(datetimeStr);

    XMLNode &location = xml.addChild("location");
    XMLNode &lat = location.addChild("latitude");
    lat.setContent(gps.location.lat(), 9);
    XMLNode &lng = location.addChild("longitude");
    lng.setContent(gps.location.lng(), 9);
    XMLNode &readingAge = xml.addChild("readingage");
    readingAge.setContent(gps.location.age());

    XMLNode &satellites = xml.addChild("satellites");
    satellites.setContent(gps.satellites.value());

    XMLNode &altitude = xml.addChild("altitude");
    altitude.addAttribute("unit", "meters");
    altitude.setContent(gps.altitude.meters(), 3);

    XMLNode &speed = xml.addChild("speed");
    speed.addAttribute("unit", "mps");
    speed.setContent(gps.speed.mps(), 3);

    XMLNode &course = xml.addChild("course");
    course.setContent(gps.course.deg(), 3);

    XMLNode &hdop = xml.addChild("hdop");
    hdop.setContent(gps.hdop.value(), 3);
}
{% endhighlight %}

This function is passed a reference to a XMLNode which it then populates.
This allows it to be re-used later when we may want to construct a XMLNode for sending in a message.

To save a reading we first create the XMLNode.
{% highlight c++ %}
XMLNode reading("gpsreading");
{% endhighlight %}

Then use `readGPSToXML` to populate the node and save it to the datastore.

{% highlight c++ %}
readGPSToXML(reading);
datastore.save(reading)
{% endhighlight %}

Some checks can also be made - it is a good idea to verify that the GPS reading is valid first with
{% highlight c++ %}
gps.location.isValid()
{% endhighlight %}

and also to check that the save succeeded, indicated by `save` returning true.

We can add this in a method called `saveReadingToDatastore`, which will then return the status of the save.

We can also add a way to clear the datastore. The DataStore object `clear` method takes a query string, which specifies the properties that any objects to be removed must match.
Because we include the date and time in each reading we can purge old readings with a query matching all queries with a datetime string older than a specified time.

`@gpsreadingtime >= 'DATETIME STRING'`

This datetime string can be generated using a similar method as with generating the string in `readGPSToXML`, but we subtract the desired seconds from `currentDateTimeSeconds` before formatting it.

Finally we can update the mainloop to do saving at a pre-determined period and periodic clearing of old values.

This version can be found in [commit c910142](https://github.com/IMG-FlowCloud/geofence/tree/c91014244d702703a2930aae3465e40068deb8df).
