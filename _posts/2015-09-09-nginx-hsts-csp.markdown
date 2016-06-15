---
layout: post
title:  "Good default NGINX config with HSTS and CSP"
date:   2015-09-09 13:00:00
categories: programming
---

A good start for an NGINX SSL configuration.

{% highlight c %}
# Both Unicorn and Passenger send HSTS headers.
# Rails/Unicorn/Passenger also appear to send X-Frame-Options SAMEORIGIN, X-Content-Type-Options nosniff, and X-XSS-Protection '1; mode=block'
# When running a Rails app, the CSP header below is all that's needed, and can be customised according to links below
# https://en.wikipedia.org/wiki/Content_Security_Policy and http://www.html5rocks.com/en/tutorials/security/content-security-policy/

server {
  listen 443;
  server_name %HOSTNAME%;

  access_log  logs/access.log  main;

  ssl on;
  ssl_certificate %CERT%;
  ssl_certificate_key %KEY%;

  ssl_session_timeout 5m;

  ssl_protocols TLSv1;
  ssl_ciphers ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:ECDH+3DES:DH+3DES:RSA+AESGCM:RSA+AES:RSA+3DES:!aNULL:!MD5:!DSS;
  ssl_prefer_server_ciphers on;

  add_header X-XSS-Protection "1; mode=block";
  add_header X-Content-Type-Options nosniff;
  add_header X-Frame-Options SAMEORIGIN;
  add_header Strict-Transport-Security "max-age=31536000; includeSubdomains";
  add_header Content-Security-Policy "default-src 'self'; script-src 'self'; img-src 'self'; style-src 'self'; font-src 'self'; object-src 'none'";

  root /var/www;
  #root /var/www/app/current/public;   # <--- be sure to point to 'public' if you're serving static assets here
}
{% endhighlight %}
