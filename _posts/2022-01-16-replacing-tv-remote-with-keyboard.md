---
layout: post
title: "TV Remote → Keyboard"
date: 2022-01-16 22:00:00
---

tl;dr - I replaced my TV remote with a keyboard.

<img src="/images/usb_uirt_demo.gif" style="display: block; margin: auto;">

## But why?

This may sound like a stupid idea, but first hear me out. In our living room we have a TV which is predominantly connected to a PC for watching YouTube, Netflix, web browsing, and couch gaming. We have a wireless keyboard with trackpad that is surprisingly effective.

```
 ┌───────────┐           ┌──────────┐
 │           │    IR     │          │
 │ TV REMOTE │ ───────►  │    TV    │
 │           │           │          │
 └───────────┘           └──────────┘
                              ▲
                              │
                              │ HDMI
                              │

 ┌───────────┐           ┌──────────┐
 │           │ BLUETOOTH │          │
 │ KEYBOARD  │ ───────►  │    PC    │
 │           │           │          │
 └───────────┘           └──────────┘
```

This is the setup for a very cliché problem: **where the !@#$ is the TV remote**? Then the deeper question is, why do I even need a TV remote? I am happy to report that for daily use, I have now replaced the TV remote with my keyboard. Thankfully this is also much harder to lose in the couch cushions :P This project requires very little technical know-how so it's a very approachable DIY solution for others. If you're interested, read on!

<img src="/images/usb_uirt.jpg" style="display: block; margin: auto;">

That is a USB-UIRT from [usbuirt.com](http://www.usbuirt.com/). The acronym stands for Universal Serial Bus (USB) - Universal Infrared Receiver/Transmitter (UIRT). Put simply, it's a programmable TV remote. It can record signals from a TV remote, store them, and transmit them to your TV. There are several devices like it but I chose this one because it has a good reputation and support for hobby projects. By connecting it to my PC I can issue a command from my keyboard to send out IR signals to mimic my TV remote. This enables the magic by creating the below dataflow architecture:

```
                         ┌──────────┐
                         │          │        IR
                         │    TV    │ ◄───────────────┐
                         │          │                 │
                         └──────────┘                 │
                              ▲                       │
                              │                       │
                              │ HDMI                  │
                              │                       │

 ┌───────────┐           ┌──────────┐           ┌──────────┐
 │           │ BLUETOOTH │          │    USB    │          │
 │ KEYBOARD  │ ───────►  │    PC    │ ───────►  │ USB-UIRT │
 │           │           │          │           │          │
 └───────────┘           └──────────┘           └──────────┘
 ```

## Setup Guide

The rest of this post will focus on how to set this up for yourself. Once you have a USB-UIRT the setup will take less than 30 min.

The first step is to install drivers. The USB-UIRT website provides detailed instructions on the [Getting Started](http://usbuirt.com/getstart.htm) page. Afterward a full reboot is required to enable the changes. The next step is to install some automation software to support a keyboard shortcut trigger initiating an IR transmission. Several forums and the USB-UIRT website recommend a suite called [EventGhost](http://eventghost.net/) so I went with that one. I don't really need advanced capabilities so I didn't really shop around.

EventGhost builds on a several concepts called Plugins, Actions, Events, and Macros:

* **Plugin**: Connects a specific device or functionality to provide actions and events.
* **Macros**: A collection of events and actions used to perform a workflow of set of behaviors.
* **Event**: A trigger which can be used to cause an action. For example, pressing the "A" button is modeled as an event.
* **Action**: The desired behavior of a macro which is triggered by an event. For example, sending an IR "power on" transmission.

Upon installing EventGhost you need to add 2 plugins through the `Configuration -> Add Plugin...` flow:

* **Keyboard**: Generates keypress events
* **USB-UIRT**: Generates IR transmittion actions

After doing that you can create macros for each TV remote button you want to map to keyboard. For example, here is how I added a TV Power toggle button:

1. Add Macro
1. Name it `TV Power Shortcut`
1. Press the keyboard shortcut you would like to use. You should see it displayed in the left  column log of EventGhost.
1. Drag and drop it over to your Macro
1. Right click your macro and click `Add Action...`
1. Select `USB-UIRT -> Transmit IR`
1. Click `Learn an IR Code...`
1. Point your TV remote at the USB-UIRT and press the power button until the code is learned. I noticed that the learning dialog closes and EventGhost can hang a few seconds when it does this.
1. Click Ok to save your action.

And there you have it. Try pressing your keyboard shortcut now and you should see it displayed in the log, along with your macro and action. The USB-UIRT should also be flashing red as it transmits the signal and maybe now your TV is off! Like magic.

<img src="/images/eventghost.png" style="display: block; margin: auto;">

## Caveats and Closing

So far the only drawback I've noticed is that when the PC is powered off or locked then I need to revert back to the TV remote. Kinda obvious. I'm not too bothered by the locked behavior because the PC remains permanently unlocked. If I find more I'll update this section.

And that's everything. Thank you to everyone that left bread crumbs throughout reddit and random HTPC forums with pointers for how to set this up. I'm doing my small part in return here by documenting my simple set up. If you have any questions please hit me up on Twitter.