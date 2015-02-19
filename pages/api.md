---
layout: page
title: Flow Integration API
---

This page describes various classes and variables that are used to ease the integration of FlowCloud with Arduino. 

<style type="text/css">
.el {
  font-weight: bold;
  color: #268BD2;
}
.doxygen tbody tr td{
  background-color: #fff;
}
.doxygen tbody tr:nth-child(3n+1) td {
  background-color: #F9F9F9;
  line-height: 1px;
}
table tbody { 
  border: 0px solid #fff;
}
</style>

## XML Manipulation
<table class="doxygen" id="xmlnode">
  <tbody>
    <tr class="heading">
      <td colspan="2">
      <h2 style="padding-top: 0px; margin-top: 0px;"><font size="3em" color="#777">class</font>&nbsp;XMLNode</h2>
      <h3 style="padding-top: 0px; margin-top: 0px;">A simple, writable node in an XML document.</h3>
      <pre style="padding: 0px; margin: 0px;">XMLNode.h</pre>
      </td>
    </tr>
    <tr>
      <td style="text-align: right;"> &nbsp;</td>
      <td style="text-align: left;" valign="bottom"><b>XMLNode</b> (char *_name)</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Construct a XML node with the specified name.</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>
    <tr>
      <td style="text-align: right;"><a class="el" title="A simple, writable node in an XML document." href="#xmlnode">XMLNode</a>&amp;&nbsp;</td>
      <td style="text-align: left;" valign="bottom"><b>addChild</b> (char *childName)</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Create and add a new <a class="el" title="A simple, writable node in an XML document." href="#xmlnode">XMLNode</a> as a child.</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>
    <tr>
      <td style="text-align: right;"> void&nbsp;</td>
      <td style="text-align: left;" valign="bottom"><b>addAttribute</b> (char *name, char *value)</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Add an attribute with the specified name and value.</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>
    <tr>
      <td style="text-align: right;"> void&nbsp;</td>
      <td style="text-align: left;" valign="bottom"><b>addAttribute</b> (char *name, int value)</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Add an attribute with the specified name and integer value.</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>
    <tr>
      <td style="text-align: right;"> void&nbsp;</td>
      <td style="text-align: left;" valign="bottom"><b>addAttribute</b> (char *name, double value, int places)</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Add an attribute with the specified name and decimal value, truncated to the specified number of decimal places.</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>
    <tr>
      <td style="text-align: right;"> void&nbsp;</td>
      <td style="text-align: left;" valign="bottom"><b>setContent</b> (char *content)</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Set the content to the specified content.</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>
    <tr>
      <td style="text-align: right;"> void&nbsp;</td>
      <td style="text-align: left;" valign="bottom"><b>setContent</b> (int content)</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Set the content to the specified content.</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>
    <tr>
      <td style="text-align: right;"> void&nbsp;</td>
      <td style="text-align: left;" valign="bottom"><b>setContent</b> (double content, int places)</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Set the content to the specified content.</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>
    <tr>
      <td style="text-align: right;"> StringBuilder&nbsp;</td>
      <td style="text-align: left;" valign="bottom"><b>appendTo</b> (StringBuilder builder)</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Append the string representation of this XML node with all attributes, contents and children to the specified StringBuilder.</td>
    </tr>
  </tbody>
</table>

<table class="doxygen" id="readablexmlnode">
  <tbody>
    <tr class="heading">
      <td colspan="2">
        <h2 style="padding-top: 0px; margin-top: 0px;"><font size="3em" color="#777">class</font>&nbsp;ReadableXMLNode</h2>
        <h3 style="padding-top: 0px; margin-top: 0px;">A simple, readable node in an XML document.</h3>
        <pre style="padding: 0px; margin: 0px;">XMLNode.h</pre>
      </td>
    </tr>
    <tr>
      <td style="text-align: right;"> &nbsp;</td>
      <td style="text-align: left;" valign="bottom"><b>ReadableXMLNode</b> (char *_name)</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Construct a readable XML node with the specified name.</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>
    <tr>
      <td style="text-align: right;"> &nbsp;</td>
      <td style="text-align: left;" valign="bottom"><b>ReadableXMLNode</b> (TreeNode _wrap)</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Construct a readable XML node wrapping access to the FlowCloud TreeNode type.</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>
    <tr>
      <td style="text-align: right;"><a class="el" title="A simple, readable node in an XML document." href="#readablexmlnode">ReadableXMLNode</a></td>
      <td style="text-align: left;" valign="bottom"><b>getChild</b> (char *path)</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Retrieve a ReadableXMLNode from the specified path. A single name (e.g. "foo") look for a child of this node named foo. Alternatively '/' can be used to specify multiple levels of children - "foo/bar/baz" looks for a child named baz that this the child of a node named bar, which is a child of this node named foo.</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>
    <tr>
      <td style="text-align: right;">const char&nbsp;*</td>
      <td style="text-align: left;" valign="bottom"><b>getName</b> ()</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Return the name of this node.</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>
    <tr>
      <td style="text-align: right;">const char&nbsp;*</td>
      <td style="text-align: left;" valign="bottom"><b>getValue</b> ()</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Get the string value of this node.</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>
    <tr>
      <td style="text-align: right;"> int&nbsp;</td>
      <td style="text-align: left;" valign="bottom"><b>getIntegerValue</b> ()</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Get the value of this node, parsed as an int (%d).</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>
    <tr>
      <td style="text-align: right;"> double&nbsp;</td>
      <td style="text-align: left;" valign="bottom"><b>getFloatValue</b> ()</td>
    </tr>
    <tr>
      <td>&nbsp;</td>
      <td style="text-align: left;">Get the value of this node, parsed as a floating-point number (double - %lf).</td>
    </tr>
  </tbody>
</table>


<div id="flowcommandhandler"></div>
## Command Handler
<table class="doxygen">
  <tbody>
    <tr class="heading">
      <td colspan="2">
        <h2 style="padding-top: 0px; margin-top: 0px;"><font size="3em" color="#777">class</font>&nbsp;FlowCommandHandler</h2>
        <h3 style="padding-top: 0px; margin-top: 0px;">A mapping of command strings to callback functions.</h3>
        <pre style="padding: 0px; margin: 0px;">FlowCommandHandler.h</pre>
      </td>
    </tr>
    <tr>
      <td style="text-align: right;">bool</td>
      <td style="text-align: left;" valign="bottom"><b>attach</b> (char *command, <a class="el" title="void (ReadableXMLNode &amp;, XMLNode &amp;)" href="#commandcallbackfunction">CommandCallbackFunction</a> callback)</td>
    </tr>
    <tr>
      <td></td>
      <td style="text-align: left;">
        Register a new callback for the specified command string
        <ul>
        <li><b>char *command</b>&nbsp;&nbsp;the string to trigger the callback</li>
        <li><b><a class="el" title="void (ReadableXMLNode &amp;, XMLNode &amp;)" href="#commandcallbackfunction">CommandCallbackFunction</a> callback</b>&nbsp;&nbsp;the function to call when the command string is encountered</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>
    <tr>
      <td style="text-align: right;">bool</td>
      <td style="text-align: left;" valign="bottom"><b>handleCommand</b> (char *command, <a class="el" title="A simple, readable node in an XML document." href="#readablexmlnode">ReadableXMLNode</a> &amp;params, <a class="el" title="A simple, writable node in an XML document." href="#xmlnode">XMLNode</a> &amp;response)</td>
    </tr>
    <tr>
      <td></td>
      <td style="text-align: left;">
        Handle the command, by passing it to a mapped command handler, or returning false if there is no mapped command handler.
        <ul>
        <li><b>char *command</b>&nbsp;&nbsp;the command to handle</li>
        <li><b><a class="el" title="A simple, readable node in an XML document." href="#readablexmlnode">ReadableXMLNode</a> params</b>&nbsp;&nbsp;the function to call when the command string is encountered</li>
        <li><b><a class="el" title="A simple, writable node in an XML document." href="#xmlnode">XMLNode</a> response</b>&nbsp;&nbsp;the function to call when the command string is encountered</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

<div id="commandcallbackfunction"></div>
<table class="doxygen">
  <tbody>
    <tr>
      <td colspan="2">
        <h2 style="padding-top: 0px; margin-top: 0px;"><font size="3em" color="#777">function</font>&nbsp;CommandCallbackFunction</h2>
        <h3 style="padding-top: 0px; margin-top: 0px;">A function to be called to handle a specific command.</h3>
        <pre style="padding: 0px; margin: 0px;">FlowCommandHandler.h</pre>
      </td>
    </tr>
    <tr>
      <td style="text-align: right;">typedef</td>
      <td style="text-align: left;"><b>CommandCallbackFunction</b></td>
    </tr>
    <tr>
      <td></td>
       <td style="text-align: left;">void (ReadableXMLNode &amp;, XMLNode &amp;)</td>
    </tr>
  </tbody>
</table>


<div id="controlvariables"></div>
## Control variables

<table class="doxygen">
  <tbody>
    <tr class="heading">
      <td colspan="2">
        <h2 style="padding-top: 0px; margin-top: 0px;"><font size="3em" color="#777">global variables</font></h2>
        <pre style="padding: 0px; margin: 0px;">global</pre>
      </td>
    </tr>
    <tr>
      <td style="text-align: right;">bool</td>
      <td style="text-align: left;"><b>g_EnableConsole</b></td>
    </tr>
    <tr>
      <td></td>
       <td style="text-align: left;"> Enable the FlowCloud library console. This will print status messages and logging over the default serial port.</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>

    <tr>
      <td style="text-align: right;">bool</td>
      <td style="text-align: left;"><b>g_EnableConsoleInput</b></td>
    </tr>
    <tr>
      <td></td>
       <td style="text-align: left;"> Enable console input to the FlowCloud library. This will allow input to the FlowCloud CLI. This should only be enabled when Arduino code is not using console input (e.g. Serial.read etc.)</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>

    <tr>
      <td style="text-align: right;">bool</td>
      <td style="text-align: left;"><b>g_EnableUIControlLED</b></td>
    </tr>
    <tr>
      <td></td>
       <td style="text-align: left;">Enable access to the 4 LEDs from the FlowCloud library. The FlowCloud library uses the LEDs to show the status, but in doing so it will override state set by the Arduino code.</td>
    </tr>
  </tbody>
</table>

<div id="globalvariables"></div>
## FlowCloud variables

<table class="doxygen">
  <tbody>
    <tr class="heading">
      <td colspan="2">
        <h2 style="padding-top: 0px; margin-top: 0px;"><font size="3em" color="#777">global variables</font></h2>
        <pre style="padding: 0px; margin: 0px;">global</pre>
      </td>
    </tr>
    <tr>
      <td style="text-align: right;">char&nbsp;*</td>
      <td style="text-align: left;"><b>g_ClientID</b></td>
    </tr>
    <tr>
      <td></td>
      <td style="text-align: left;">The FlowCloud unique identification for this device.</td>
    </tr>
    <tr>
      <td colspan="2">&nbsp;</td>
    </tr>

    <tr>
      <td style="text-align: right;">char&nbsp;*</td>
      <td style="text-align: left;"><b>g_OwnerID</b></td>
    </tr>
    <tr>
      <td></td>
      <td style="text-align: left;">The FlowCloud unique identification for the owner (user) of this device.</td>
    </tr>
  </tbody>
</table>
