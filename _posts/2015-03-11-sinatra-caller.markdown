---
layout: post
title:  "Asterisk/Lua: Auto-call both ways using Sinatra"
date:   2015-03-11 13:00:00
categories: lua asterisk programming
---
Easy way to automate calls in both directions hitting a web API.

{% highlight ruby %}
require 'sinatra'

get '/from/:ext/to/:outbound/:context' do
  file = []
  file << "Channel: SIP/#{params[:ext]}"
  file << "MaxRetries: 2"
  file << "RetryTime: 60"
  file << "WaitTime: 30"
  file << "Context: #{params[:context]}"
  file << "Extension: #{params[:outbound].gsub(/\s/,'')}"
  file << "Priority: 1"

  File.open("./tempcall.call", "w") { |fh| fh.write file.join("\n") }

  %x{mv ./tempcall.call /var/spool/asterisk/outgoing}
end
{% endhighlight %}

Using Ruby and [Asterisk][asterisk].

[2015-05-04] Update: Allow call context to be passed in via web.

[asterisk]: http://www.asterisk.org/
