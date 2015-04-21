---
layout: post
title: Geofence Application&#58; The Geofence 
---

It's finally time to actually implement our Geofence.
We'll store a single active geofence on the device, containing a location and a radius. 
If the device goes outside this radius then we will send an alert to the client device.

Setting the radius to `0` will signify that there is no geofence active.

We will define the serialised geofence with the following XML.
{% highlight xml %}
<geofence>
	<location>
		<latitude>-41.21</latitude>
		<longitude>174.90</longitude>
	</location>
	<radius>200</radius>
</geofence>
{% endhighlight %}


To store the geofence on the device we can create the following struct

{% highlight c++ %}
struct Geofence
{
	double latitude;
	double longitude;
	double radius;
};
{% endhighlight %}

We can then create our variable to hold the geofence, defaulting to no geofence set.

{% highlight c++ %}
Geofence fence = {0};
{% endhighlight %}

We then need to add command handlers for getting and setting the fence,

{% highlight c++ %}
#define SET_GEOFENCE ("SET GEOFENCE")
#define GET_GEOFENCE ("GET GEOFENCE")
{% endhighlight %} 

define the commands and
{% highlight c++ %}
FlowCommandHandler.attach(SET_GEOFENCE, setGeofence);
FlowCommandHandler.attach(GET_GEOFENCE, getGeofence);
{% endhighlight %} 

map the commands to the functions.

We can then implement these functions with 

{% highlight c++ %}
// command handler for setting the geofence
void setGeofence(ReadableXMLNode &params, XMLNode &response)
{
	#define LAT_LOCATION "geofence/location/latitude"
	fence.latitude = params.getChild(LAT_LOCATION).getFloatValue();

	#define LNG_LOCATION "geofence/location/longitude"
	fence.longitude = params.getChild(LNG_LOCATION).getFloatValue();

	fence.radius = params.getChild("geofence/radius").getFloatValue();
}

// command handler for getting the existing geofence
void getGeofence(ReadableXMLNode &params, XMLNode &response)
{
	XMLNode &geofence = response.addChild("geofence");
	XMLNode &location = geofence.addChild("location");
	location.addChild("latitude").setContent(fence.latitude, 9);
	location.addChild("longitude").setContent(fence.longitude, 9);
	geofence.addChild("radius").setContent(fence.radius, 5);
}
{% endhighlight %} 

Note the use of `/` in getChild to navigate the XML we defined earlier. This works in a similar way to the forward-slash separator used in XPATH.

This version can be found in [commit 58cd12e](https://github.com/FlowCloud/geofence/tree/58cd12ecd7a501255c2fe08d499001c1fb88791a).

We now have a geofence stored on our device and we can get on to generating an alert for when the GPS reading is outside the geofence.