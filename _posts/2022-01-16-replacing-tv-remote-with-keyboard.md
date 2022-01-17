---
layout: post
title: "TV Remote → Keyboard"
date: 2022-01-16 22:00:00
---

tl;dr - I replaced my TV remote with a keyboard.

Now that may sound like a stupid idea, but first hear me out. In our living room we have a TV which is predominantly connected to a PC for watching YouTube, Netflix, web browsing, and couch gaming. We have a wireless keyboard with trackpad that is surprisingly effective.

This is the setup for a very cliché problem: **where the !@#$ is the TV remote**? Then the deeper question is, why do I even need a TV remote? I am happy to report that for daily use, I have now replaced the TV remote with my keyboard. Thankfully this is also much harder to lose in the couch cushions :P This project requires very little technical know-how so it's a very approachable DIY solution for others if you're interested.

<img src="/images/usb_uirt.jpg" style="display: block; margin: auto;">

That is a USB-UIRT from [usbuirt.com](http://www.usbuirt.com/). The acronym stands for Universal Serial Bus (USB) - Universal Infrared Receiver/Transmitter (UIRT). Put simply, it's a programmable TV remote. It can record signals from a TV remote, store them, and transmit them to your TV. There are several devices like it but I chose this one because it has a good reputation and support for hobby projects.

To configure it on the TV PC I first needed to install drivers. The USB-UIRT website provides detailed instructions on the [Getting Started](http://usbuirt.com/getstart.htm) page. It took about 10 minutes including a full reboot.

The next step was to install some automation software to support a keyboard shortcut trigger initiating an IR transmission. Several forums and the USB-UIRT website recommend a suite called [EventGhost](http://eventghost.net/) so I went with that one. I don't really need advanced capabilities so I didn't really shop around.

EventGhost builds on a several concepts called Plugins, Actions, Events, and Macros:

* **Plugin**: Connects a specific device or functionality to provide actions and events.
* **Macros**: A collection of events and actions used to perform a workflow of set of behaviors.
* **Event**: A trigger which can be used to cause an action. For example, pressing the "A" button is modeled as an event.
* **Action**: The desired behavior of a macro which is triggered by an event. For example, sending an IR "power on" transmission.

Upon installing EventGhost I first added 2 plugins through the `Configuration -> Add Plugin...` flow:

* **Keyboard**: Generates keypress events
* **USB-UIRT**: Generates IR transmittion actions

After doing that I could create macros for each TV remote button I wanted to map to my keyboard. For example, here is how I added a TV Power toggle button:

1. Add Macro
1. Name it "TV Power Shortcut"
1. Press the keyboard shortcut you would like to use. You should see it displayed in the left  column log of EventGhost.
1. Drag and drop it over to your Macro
1. Right click your macro and click "Add Action..."
1. Select "USB-UIRT -> Transmit IR"
1. Click "Learn an IR Code..."
1. Point your TV remote at the USB-UIRT and press the power button until the code is learned. I noticed that the learning dialog closes and EventGhost can hang a few seconds when it does this.
1. Click Ok to save your action.

And there you have it. Try pressing your keyboard shortcut now and you should see it displayed in the log, along with your macro and action. The USB-UIRT should also be flashing red as it transmits the signal and maybe now your TV is off! Like magic.

<img src="/images/eventghost.jpg" style="display: block; margin: auto;">

Thank you to everyone that left bread crumbs throughout reddit and random HTPC forums with pointers for how to set this up. I'm doing my small part in return here by documenting my simple set up. If you have any questions please hit me up on Twitter.