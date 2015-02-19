---
layout: post
title: Geofence Application&#58; Geofence Alerts 
---

To send an alert to that the geofence has been escaped we'll use the [FlowMessaging Point to Point API](http://uploads.flowworld.com/libflow/docs/c/2.0/index.html#Point2Point). Using [SendMessageToUser](http://uploads.flowworld.com/libflow/docs/c/2.0/d5/d14/group__Notifications.html#gab625f15f7449b611bcc135e91ad34e32) we can send a message to the user who owns the WiFire.

To use this API we will include `flowmessaging` like so
{% highlight c++ %}
extern "C" {
	#include "flow/flowmessaging.h"
}
{% endhighlight %} 

Because Flow APIs are C code and we are using C++ we need to import the headers inside a block of C code.

We can define alerts to be XML messages emitted when we want to alert a user or device of some event.
{% highlight xml %}
<alert>
    <sent>
        2015-01-29T05:08:16Z
    </gpsreadingtime>
    <type>
    	ALERT TYPE
    </type>
</alert>
{% endhighlight %}

We'll define the string `GEOFENCE ESCAPED` for our alert type
{% highlight c++ %}
#define GEOFENCE_ESCAPED ("GEOFENCE ESCAPED")
{% endhighlight %} 

It would be possible to add structured (XML) details to alerts later, but we only need basic alerts at this time.


In order to avoid noise and continual alerts we only want to generate an alert if the escape from the fence is for a certain time. We also should only generate an alert if the device has been inside the fence for a period of time before leaving. This can be achieved by keeping track of when we were last inside and when we were last outside the fence, allowing us to work out how long we have been inside/outside the fence. If this time exceeds a defined threshold then we consider the device to have entered/exited and can change the state for monitoring for the opposite even, along with generating an alert when a "exiting" event is detected.

{% highlight c++ %}
static long lastInsideFence = 0;
static long lastOutsideFence = 0;
#define FENCE_TIME_THRESHOLD (1*60*1000)
if (gps.location.isValid() && fence.radius > 0)
{
	// check for escaped geofence
	double distance = gps.distanceBetween(
							fence.latitude, 	fence.longitude, 
							gps.location.lat(),	gps.location.lng()
					  );
	if (distance > fence.radius)
	{
		// escaped
		lastOutsideFence = millis();
		if (insideFence && 
			lastOutsideFence - lastInsideFence > FENCE_TIME_THRESHOLD)
		{
			insideFence = false;
			sendAlert(GEOFENCE_ESCAPED);
		}
	}
	else 
	{
		lastInsideFence = millis();
		digitalWrite(PIN_LED1, LOW);
		if (!insideFence && 
			lastInsideFence - lastOutsideFence > FENCE_TIME_THRESHOLD)
		{
			insideFence = true;
		}
	}
}
{% endhighlight %} 

To send an alert we can construct the alert XML using
{% highlight c++ %}
void sendAlert(const char *type)
{
	XMLNode alert("alert");

	XMLNode &sent = alert.addChild("sent");
	setDatetimeContent(sent);

	XMLNode &typeNode = alert.addChild("type");
	typeNode.setContent(type);

	StringBuilder alertStringBuilder = StringBuilder_New(512);
	alertStringBuilder = StringBuilder_Append(alertStringBuilder, "<?xml version=\"1.0\" encoding=\"UTF-8\"?>");
	alert.appendTo(alertStringBuilder);

	FlowMessaging_SendMessageToUser(
		g_OwnerID, 
		"text/plain", 
		(char *)StringBuilder_GetCString(alertStringBuilder), 
		StringBuilder_GetLength(alertStringBuilder), 
		120
	);

	StringBuilder_Free(&alertStringBuilder);
}
{% endhighlight %} 

Note specifically `FlowMessaging_SendMessageToUser` from the FlowMessaging library.
To identify the target we use the [FlowCloud variable for the owner's ID](/flow-on-arduino/pages/api#globalvariables). 
To send messages to other devices we could use `FlowMessaging_SendMessageToDevice`. In this case the device IDs could be hard-coded or fetched programmatically using the [FlowDevices API](http://uploads.flowworld.com/libflowcore/docs/c/2.0/db/da6/classFlowDevices.html). 

We've now finished our geofence application on the WiFire. The final code can be found in the [GitHub repository](https://github.com/IMG-FlowCloud/geofence).

