---
layout: page
title: Interactive Serial Output
---

It may be useful to have access to the FlowCloud command line interface (CLI) for debugging purposes and to reconfigure it.
It is suggested that you use a dedicated program for accessing the CLI such as PuTTY (see the page on [console setup](/pages/console-setup) for how to connect PuTTY to the WiFire) however MPIDE's Serial Monitor may suffice.
The FlowCloud framework in Arduino uses the same serial port as is used in the Arduino sketches, so once it has finished using the port during configuration and boot it will relinquish control back to Arduino.

To re-allow Flow to use the serial we first need to bring it up from within our sketch.
Adding the line 
{% highlight c++ %}
Serial.begin(115200);
{% endhighlight %}
in the `setup()` method will allow us to write to the serial port.
Adding
{% highlight c++ %}
g_EnableConsole = true;
{% endhighlight %}
will then allow FlowCloud to also use the same port and
{% highlight c++ %}
g_EnableConsoleInput = true; 
{% endhighlight %}
will allow FlowCloud to take input to the command handler. More information on what you can do with the command handler can be found [here](http://flow.imgtec.com/developers/help/wifire/cli) or in the [FlowCloud specification](http://uploads.flowworld.com/Wifire_fe.Command_line_interface_specification_(v1_0_7).pdf)

Writing to the serial port will be shared, and messages from both FlowCloud and our sketch will be sent over the serial port.

## The Baud Rate
Using 
{% highlight c++ %}
Serial.begin(115200);
{% endhighlight %}
we run the serial port at 115200 bytes per second. This is not required and some tutorials use other rates such as 9600. 

115200 has the advantage in this case that it is the same as what is used during the boot process, so we can see the boot log at the same time as anything printed out after the sketch started.

## Console Input

Issuing 
{% highlight c++ %}
g_EnableConsoleInput = true; 
{% endhighlight %}
Allows the FlowCloud framework to receive input, but if we want to use functions such as
{% highlight c++ %}
Serial.read()
{% endhighlight %}
and other commands that read from the serial port we can add
{% highlight c++ %}
g_EnableConsoleInput = false; 
{% endhighlight %}
beforehand and then re-enable FlowCloud's access afterwards. 

Alternately we could simply never enable console input in the first place.