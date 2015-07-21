---
layout: post
title:  "XInput to DS4 mapping for xboxdrv on Linux"
date:   2015-06-22 13:00:00
categories: linux xinput programming
---

This is a complete list of mappings for the DS4 controller to work with xboxdrv on Linux. Tested on Ubuntu 14.04.

{% highlight text %}
ABS_X=x1
ABS_Y=y1
ABS_Z=x2
ABS_RZ=y2
ABS_HAT0X=dpad_x
ABS_HAT0Y=dpad_y
ABS_RX=lt
ABS_RY=rt
-Y1=Y1
-Y2=Y2
BTN_A=x
BTN_X=y
BTN_B=a
BTN_C=b
BTN_TL2=back
BTN_TR2=start
BTN_Y=lb
BTN_Z=rb
BTN_SELECT=tl
BTN_START=tr
{% endhighlight %}

This is the command I run to get the emulation started. Please keep in mind that the --evdev argument will change per-system, as not everyone's DS4 is going to come up as event2.

To make sure Steam and Big Picture (and other games) see the XInput device instead of the DS4, after this command is running, I chmod the event2 input device to 400. It seems that Steam and most games will simply listen for input on the first event input device they can read. As 400 allows no reading, the virtuall (XInput) device is the one which is picked up. Enjoy!

{% highlight bash %}
sudo xboxdrv \
  --evdev /dev/input/event2  \
  --evdev-absmap ABS_X=x1,ABS_Y=y1,ABS_Z=x2,ABS_RZ=y2,ABS_HAT0X=dpad_x,ABS_HAT0Y=dpad_y,ABS_RX=lt,ABS_RY=rt \
  --axismap -Y1=Y1,-Y2=Y2 \
  --evdev-keymap BTN_A=x,BTN_X=y,BTN_B=a,BTN_C=b,BTN_TL2=back,BTN_TR2=start,BTN_Y=lb,BTN_Z=rb,BTN_SELECT=tl,BTN_START=tr \
  --mimic-xpad \
  --silent
{% endhighlight %}
