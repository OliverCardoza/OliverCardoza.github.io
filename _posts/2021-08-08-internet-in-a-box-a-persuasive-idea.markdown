---
layout: post
title: "Internet In a Box: A Persuasive Idea"
date: 2021-08-08 22:03:00
---

tl;dr - I built an internet in a box.

<img src="/images/iiab_1.jpg">

One day, not too long ago, I stumbled across the following Hacker News post: [Internet in a Box](https://news.ycombinator.com/item?id=27568332). The name is catchy and inspired my click. As I started scrolling through the [homepage](http://internet-in-a-box.org/) the idea took hold of me.

> Internet-in-a-Box brings the power of a free Digital Library of Alexandria into the hands of any school, hospital, or community worldwide.

You can install the *internet in a box* (IIAB) software on all types of computers and then download content packs from Wikipedia, OpenStreetMaps, and more. Once the content is downloaded the computer acts like your very own offline data cache.

> You can install an Internet-in-a-Box "learning hotspot" anywhere in the world — even under solar power, using very diverse hardware.

Now I’m not really the primary market for this kind of thing because I have reliable, fast internet. The real primary users are those that do not have reliable internet access. I have the lived experience of going from no internet, to dial up, DSL, and finally gigabit fiber. I don’t think it’s an exaggeration to say that this had a profound impact on me, and my family. In some places of the world, unfortunately the infrastructure required is not coming fast enough. However, projects like this appear to be making a real change to connect these places through an “internet in a box”.

Even though I’m not the primary audience I still think that there’s a lot to get excited about here. I think most of us have experienced a sense of helplessness when the internet goes down. We can’t work, and we can’t watch Netflix or YouTube. Why not go outside? Well if the weather is too wet, cold, hot, or [on fire](https://en.wikipedia.org/wiki/Wildfires_in_2021) then you might be shit out of luck. A few decades ago this wouldn’t have been a problem. However, now it’s almost unthinkable to live completely off-grid. While I don’t think IIAB truly replaces the connectivity of the internet it does appear to provide a half-decent read-only version.

I decided I’d give this a go and create my own little “internet in a box”. The project documentation provides some recommendations for hardware: [What hardware should I use?](http://wiki.laptop.org/go/IIAB/FAQ#What_hardware_should_I_use.3F). The actual processing and memory requirements don’t seem particularly challenging because they recommend a Raspberry Pi. However, when it comes to storage space it does appear that more is better. You can use a large SD card, USB stick, SSD, or hard drive to add extra storage. I opted for the hard drive because it’s the cheapest per terabyte.

I ended up buying the following:

* [Raspberry Pi 4B - 2GB Starter Kit](https://www.buyapi.ca/product/raspberry-pi-4b-starter-kit/) - $115
* [Seagate Backup Plus 4TB Portable Hard Drive](https://www.canadacomputers.com/product_info.php?cPath=15_213_602&item_id=137779) - $125

<img src="/images/iiab_2.jpg">

The prices above may look a bit steep but they include taxes, shipping, and the weak Canadian dollar. I want to acknowledge that I may have gone a little overboard with the hard drive. If you can acquire a Raspberry Pi at a local electronics store at a good deal you may be able to do this with a budget closer to $50. Even better, if you have an old laptop then you can do this for free!

The installation process for the IIAB software was relatively painless: [Installation Guide](https://github.com/iiab/iiab/wiki/IIAB-Installation). It did require running a few times, and I did accidentally install a lot of unnecessary bloat but it worked. On the other hand, I did experience a massive headache trying to configure the hard drive. In contrast to the IIAB installation which was 1 command (more-or-less), to configure the hard drive correctly it required quite a lot more expertise and Linux tools. A catalog of the work and my failures is listed below:

* **Partitioning**: By default, most external hard drives come with many unnecessary partitions. I corrected the partitions into a simpler scheme using `parted`.
* **File system**: After partitioning the storage, I set up the file system using `mkfs`.
* **Mounting**: IIAB stores most downloaded content in the `/library/` folder. It took me quite a few attempts to correctly mount the hard drive to this path. After one of my attempts I accidentally broke the boot process with bad `/etc/fstab` changes which created more issues. I needed to fix this by manually editing files on the Pi SD card from my other Linux machine.
* **USB Drivers**: While downloading content I noticed that the write speeds to the hard drive were in the single KB/s. After trying several things I eventually discovered a [raspberrypi.org forum post](https://www.raspberrypi.org/forums/viewtopic.php?t=245931) clearly outlining the problem and solution. This required mucking with `/boot/cmdline.txt` quirks to disable a data protocol which apparently some devices (cough Seagate) don’t fully implement causing problems.
* **Power Issues**: At various points I tried to perform configuration locally on the Pi with a connected display, mouse, and keyboard. However, the OS notifications would show the hard drive disconnecting every few seconds before reconnecting. Apparently the Pi can’t provide enough power for the peripherals and the hard drive so I was forced to perform most of the configuration via SSH from another computer.

Finally, I had hit the holy grail of Linux configuration where everything appeared to be in working order. In hindsight I don’t think I’d do anything differently as the experience taught me a lot about several Linux tools. However, I do wonder if some of the heavy lifting could be scripted and bundled into the IIAB setup script to help others.

Once the device is up and configured with extra storage I could then download content to it. This can be done from the admin page by connecting to the device from a browser and hitting the `/admin/` path. Finding the various admin pages and passwords was a bit tricky but eventually I did find them all documented here:

* [List of URL paths to common services on IIAB](http://wiki.laptop.org/go/IIAB/FAQ#What_services_.28IIAB_apps.29_are_suggested_during_installation.3F)
* [List of default passwords for IIAB services](http://wiki.laptop.org/go/IIAB/FAQ#What_are_the_default_passwords.3F)

So far I’ve downloaded the following content packages:

* All of English Wikipedia via [Kiwix](https://www.kiwix.org/en/) and [Wikimedia](https://www.wikimedia.org/) (82 GB)
* 65,000 free eBooks via [Kiwix](https://www.kiwix.org/en/) and [Project Gutenberg](https://www.gutenberg.org/) (63 GB)
* Details Maps and Satellite imagery for all of North America via [OpenStreetMap](https://www.openstreetmap.org/about) (21 GB)
* 45 comprehensive college-level textbooks via [OER2GO](https://oer2go.org/) and [OpenStax](https://openstax.org/) (3 GB)

And that is just scratching the surface. There are also archives of major [Stack Exchange](https://stackexchange.com/) Q&As, education content from Khan Academy, and more.

Once you have this content downloaded to the Pi you can access it via a special WiFi access point that the Pi generates called “Internet in a Box”. Once connected you can access all of the content by navigating to `http://box`.

<img src="/images/iiab_3.png">

As a final test I wanted to see if I could power the Pi and hard drive using a portable power supply. As a Christmas present my mom had gifted me a power supply that comes with an attached solar panel. While I don’t expect it to be especially powerful it did let me test the concept. And guest what, it worked! That means that even if the power goes out I can still access my internet in a box!

<img src="/images/iiab_4.jpg">

So, what did I learn?

* A community has collected the internet’s best free content and packaged it to run on cheap hardware.
* Configuring external hard drives with a Raspberry Pi can be surprisingly challenging.
* The IIAB software is fairly user-friendly and has succeeded in packing together several useful tools and content packs to make it worthy of the “internet in a box” title.

Major kudos to all of the folks that made this possible across IIAB, One Laptop Per Child, Wikimedia, Kiwix, Khan Academy, Project Gutenberg, OpenStreetMap, OpenStax, World Possible, and Raspberry Pi!
