---
layout: post
title: Geofence Application&#58; Command Handling 
---

It's now time to add some commands to our sketch. 

We would like a way for the client application to fetch the current location of the WiFire, so we aren't limited to just the most recent reading. It would also be good to allow the user to configure the the logging perdiod and to retrieve what the current logging period is.

First we need to 
{% highlight c++ %}
#include <FlowCommandHandler.h>
{% endhighlight %}

We will define the three commands as 

{% highlight c++ %}
#define GET_LOCATION ("GET LOCATION")

#define SET_PERIOD ("SET PERIOD")
#define GET_PERIOD ("GET PERIOD")
{% endhighlight %}

We can then map these commands to functions, by adding the following in the setup.

{% highlight c++ %}
// Attach the various command handlers
FlowCommandHandler.attach(GET_LOCATION, getLocation);
FlowCommandHandler.attach(SET_PERIOD, setPeriod);
FlowCommandHandler.attach(GET_PERIOD, getPeriod);
{% endhighlight %}

And implement the functions. Because we used a general-purpose method for reading the GPS location to a XML node we can reuse this for returning the location.

{% highlight c++ %}
// command handler for fetching the current location
void getLocation(ReadableXMLNode &params, XMLNode &response)
{
	if (gps.location.isValid())
	{
		XMLNode &reading = response.addChild("gpsreading");
		readGPSToXML(reading);
	}
}

// command handler for fetching the current logging period
void getPeriod(ReadableXMLNode &params, XMLNode &response)
{
	XMLNode &period = response.addChild("period");
	period.setContent(loggingPeriod);
}

// command handler for setting the period
void setPeriod(ReadableXMLNode &params, XMLNode &response)
{
	loggingPeriod = params.getChild("period").getIntegerValue();
}
{% endhighlight %}

We also need to change to using an integer loggingPeriod for how long to wait between each save, and set this to some default value.

This version can be seen in [commit f3da9ef](https://github.com/FlowCloud/geofence/tree/f3da9effc37aa6ba9b6256512261867b3c69e5a2).