---
layout: post
title:  "Asterisk/Lua: Queues"
date:   2014-12-19 13:00:00
categories: lua asterisk programming
---
How to queue and fallthrough in a Lua script

{% highlight lua %}
extensions = {
  ["from-pstn"] = {
    ["your incoming number"] = function()
      channel["numTries"]:set("0")
      app.Queue("example_queue")
      -- After timeout set below
      app.Playback("custom/no-answer")
      app.Hangup()
    end;
  };
}
{% endhighlight %}

{% highlight ini %}
[general]
autofill=yes
shared_lastcall=yes

[StandardQueue](!)
musicclass=default
strategy=ringall
joinempty=no
leavewhenempty=yes
ringinuse=no

[example_queue](StandardQueue)
servicelevel=300
wrapuptime=20
timeout=300
{% endhighlight %}

Using [Lua][lua] and [Asterisk][asterisk].

[lua]: http://www.lua.org/
[asterisk]: http://www.asterisk.org/
