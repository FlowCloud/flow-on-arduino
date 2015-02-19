---
layout: post
title: Handling Commands
---

Now that you have your WiFire and smartphone application connected to the cloud, we can start to work on interfacing between the two.

Our first port of call is to extend our flashing LED to be controlled by our smartphone.

In our sketch we can use the [FlowCommandHandler](/flow-on-arduino/pages/api/#flowcommandhandler) to register functions to run when a specific command is received.

First we need to include `FlowCommandHandler`. Add the line 
{% highlight c++ %}
#include <FlowCommandHandler.h>
{% endhighlight %}
to the top of the sketch.

We also need to determine what command to act on. The MakeItFlow App already has some commands in it, so we'll use one of these. Add the following definition for the command.
{% highlight c++ %}
#define SET_LED1_COMMAND "SET LED #1"
{% endhighlight %}

For our setup code we'll enable the FlowCloud console, enable the LED and register our command.
{% highlight c++ %}
void setup() 
{    
    // enable the serial port at the same baud as the boot log            
    Serial.begin(115200);

    // allow FlowCloud to access the console and accept 
    // commands from it (not required for this example, but useful)
    g_EnableConsole = true;
    g_EnableConsoleInput = true;
  
    // use the 1st LED as an output
    pinMode(PIN_LED1, OUTPUT);   
  
    // register the command defined by SET_LED1_COMMAND to
    // call our function that will set the LED
    FlowCommandHandler.attach(SET_LED1_COMMAND, setLED1);
}

{% endhighlight %}

For more information on what's going on with the serial configuration and console see [this page](/flow-on-arduino/pages/interactive-serial-output).

In order to accurately control the state of the LED we'll need to know the current state. 
Adding 
{% highlight c++ %}
bool led1 = LOW;
{% endhighlight %}
will keep track of the current LED state.

Next we need to define the command handler `setLED1` used in the setup. Command handlers need to match the signature for [CommandCallbackFunction](/flow-on-arduino/pages/api/#commandcallbackfunction), that is, a void function that takes a reference to a ReadableXMLNode and a reference to a XMLNode.

{% highlight c++ %}
void setLED1(ReadableXMLNode &params, XMLNode &response)
{
    led1 = !led1;
    digitalWrite(PIN_LED1, led1);
}
{% endhighlight %}

The `ReadableXMLNode &params` and `XMLNode &response` are for receiving extra parameters - we wont worry about them for now.

Finally we need to make our loop function. Because all our logic is handled by the setLED function we can have an empty loop that does nothing.
{% highlight c++ %}
void loop() 
{
}
{% endhighlight %}

<hr>

The final sketch follows. You can also find this sketch [on github](https://github.com/IMG-FlowCloud/arduino-examples/tree/master/FlowToggleLED).
{% highlight c++ %}
#include <FlowCommandHandler.h>

#define SET_LED1_COMMAND "SET LED #1"

void setup() {                
  Serial.begin(115200);
  g_EnableConsole = true;
  g_EnableConsoleInput = true;
  
  pinMode(PIN_LED1, OUTPUT);   
  
  
  FlowCommandHandler.attach(SET_LED1_COMMAND, setLED1);
}

bool led1 = LOW;
void setLED1(ReadableXMLNode &params, XMLNode &response)
{
    led1 = !led1;
    digitalWrite(PIN_LED1, led1);
}


void loop() {
}
{% endhighlight %}

<hr>

Let's try it out! Let the WiFire boot up until the lights have stopped flashing and turned off. If you connect the Serial Monitor you will see that the WiFire is publishing it's presence to the Cloud so that the App can detect it. Keep in mind that with `JP2` (the jumper just below the reset button) left in, the WiFire will reboot every time the Serial Monitor is brought up, so it's best to leave the monitor up from when you first program the WiFire.

From within the MakeItFlow App we can now select to interact with the WiFire. 

<img src="/flow-on-arduino/images/android-connected-devices.png" width="350"></img>

And then select the "SET LED #1" command by tapping on the command box and then the desired entry.

<img src="/flow-on-arduino/images/android-send-led-command.png" width="350"></img>

Now hit send and watch as the LED on the WiFire turns on.
Send the command again and we can turn it back off.

<hr>

<video src="/flow-on-arduino/images/setled.webm" width="350" controls></video>

<hr>

### Notes

There are a couple of things to remember about the command handler.
Only 50 commands are allowed by default and each command string can only be 31 characters long (+ null byte).

More crucially Command Handlers should return relatively quickly, so don't put large or complex pieces of code in them.
If a command handler needs to trigger expensive computational code, it could instead change variables to pass messages into the mainloop. Note also with sharing variables between the command handler and loop you may need to set the variable as `volatile`, similar to using [volatile variables in interrupt service routines](http://arduino.cc/en/Reference/Volatile) and for the same reasons (avoiding compiler optimizations).

<hr>

Read on for sending data back to the smartphone.