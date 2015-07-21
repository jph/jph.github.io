---
layout: post
title:  "Asterisk/Lua: Returning correctly from hangup"
date:   2014-12-10 13:00:00
categories: lua asterisk programming
---
This is useful when logic occurs after a playback, such as a ring group or queue. Obviously this can't happen if the caller disappears.

{% highlight lua %}
function incoming(context, extension)
  app.Playback("custom/Sound")

  if channel["PLAYBACKSTATUS"]:get() == "SUCCESS" then
    -- Next step here. Fall through if there's a hangup during playback.
  else
    app.Hangup()
  end
end

default = {
  ["_X."] = incoming
}
{% endhighlight %}

Using [Lua][lua] and [Asterisk][asterisk].

[lua]: http://www.lua.org/
[asterisk]: http://www.asterisk.org/
