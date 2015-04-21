---
layout: post
title: Returning Data
---

In the last page, we saw how command handlers had two arguments passed to them. First we had a 
`ReadableXMLNode` and second a `XMLNode`.

The first parameter is used for receiving extra data from a client and the second is used for sending extra data.

## Displaying output in MakeItFlow

While the version of MakeItFlow we have been using so far allows us to send commands and get back status codes, 
it does not support displaying more advanced returned data. Luckily, MakeItFlow is open source. 
The official download of the Android source can be found on 
[the FlowCloud developer website](http://flow.imgtec.com/developers/develop/mobile), 
but I have created a mirror on github [here](https://github.com/FlowCloud/make-it-flow-arduino). 

In the branch [enhanced-ouput](https://github.com/FlowCloud/make-it-flow-arduino/tree/enhanced-output) I have made the required changes to display 
response parameters. I have also added colorisation to the message log and ensured that the XML response parameters 
are pretty-printed (e.g. indented nicely). To use this version of MakeItFlow you can download the sources from 
github and build the app (simply import it into Android Studio and use Gradle to build). Alternately you can download the compiled apk [from the releases page](https://github.com/FlowCloud/make-it-flow-arduino/releases/tag/enhanced-output-v1).

This version of the app can also serve as a base for your own GUI with messages handled in the background.  

If we use the modified version of MakeIfFlow just for our `SET LED #1` command we will see the following.

<video src="/flow-on-arduino/images/enhanced-output.webm" width="350" controls></video>

Because we don't add anything to the XMLNode there are no response parameters.

## Adding response data

It's now time to return some data. Let's start with the all-time classic "Hello World".

The `XMLNode` passed into the command handler allows us to set its content with the method `setContent`. 
Let's change our setLED1 command handler to do this.

{% highlight c++ %}
void setLED1(ReadableXMLNode &params, XMLNode &response)
{
    led1 = !led1;
    digitalWrite(PIN_LED1, led1);

    response.setContent("Hello FlowCloud!");
}
{% endhighlight %}

If we upload this sketch and run the same command as before, we can see our message printed to the message list.
This sketch can be found on github [here](https://github.com/FlowCloud/arduino-examples/tree/master/FlowReturnHello).

<img src="/flow-on-arduino/images/hello-flow.png" width="352"></img>

## Advanced responses

We are not just limited to responding with a text message. It's easy to add structured properties to the response parameters.
Let's add another command handler called "GET STATUS".

First, let's add the new definition to the top of the sketch, just below our set LED command definition.
{% highlight c++ %}
#define GET_STATUS_COMMAND "GET STATUS"
{% endhighlight %}

We also need to register this handler in the setup, just like with set LED.
{% highlight c++ %}
FlowCommandHandler.attach(GET_STATUS_COMMAND, getStatus);
{% endhighlight %}

In order to send multiple pieces of data, or structured data we can add "child" nodes to 
the parent response node. These child nodes can also have children added and have their content set.

This example function can tell us whether the LED is on or not, 
but also has some example nodes added for the sake of an example.

The format that we will have our response take follows.
{% highlight xml %}
<status>
  <ledstatus>
    <led1>
      ON
    </led1>
  </ledstatus>
  <uptime>
    2323
  </uptime>
  <pi>
    3.14159
  </pi>
</status>
{% endhighlight %}

To achieve this result we need the following in our getStatus handler.

{% highlight c++ %}
void getStatus(ReadableXMLNode &params, XMLNode &response)
{
     XMLNode &statusNode = response.addChild("status");
  
     XMLNode &ledStatusNode = statusNode.addChild("ledStatus");
     
     // add a child-of-a-child node then set the content to
     // a string depending on a boolean check
     XMLNode &led1Node = ledStatusNode.addChild("led1");
     if (led1 == HIGH)
     {
       led1Node.setContent("ON");
     } 
     else 
     {
       led1Node.setContent("OFF");
     }
     
     
     // set an integer value as the content of another node
     // (millis() gives the number of millis the Arduino skecth
     // has been running)
     XMLNode &uptimeNode = statusNode.addChild("uptime");
     uptimeNode.setContent(millis());
     
     
     // calculate an aproximation of PI (using Zu's ratio)
     // then add a decimal number to the pi node, specifying
     // that we want this value to 5 decimal places
     
     // see http://en.wikipedia.org/wiki/Mil%C3%BC
     double approxPi = 355.0/113.0;
     
     XMLNode &piNode = statusNode.addChild("pi");
     piNode.setContent(approxPi, 5);
}
{% endhighlight %}

For more information on how XMLNodes can be used check out the documentation <a href="/flow-on-arduino/pages/api/#xmlnode">here</a>.


Finally we should remove the "Hello Flow!" message from the set LED command, as this command is called "set LED" not "say hello".

The final sketch is available [on github](https://github.com/FlowCloud/arduino-examples/tree/master/FlowReturnStatus).

When we send the `GET STATUS` command we can now see the structured data returned as XML.

<img src="/flow-on-arduino/images/show-status.png" width="350"></img>

<hr>

Read on for how we can send our own parameters to the WiFire.