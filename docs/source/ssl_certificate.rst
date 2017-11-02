****************
SSL Certificates
****************

If you want to setup a webserver, you might want to set it up will SSL support.
But unfortunately, SSL certificates usually cost a regular fee.

Luckily, there is a service, you can obtain SSL certificates from for free.
Although, they must be renewed every 3 months.

Let's Encrypt
=============

This nice service (https://letsencrypt.org) provides you with free
certificates. But there are three important things to know:

#. With the tool ``certbot`` you should obtain certificates from your command
   line. It makes things easier.
#. The URL(s) you're registering must be reachable from the internet, as Let's
   Encrypt wants to verify them (tries to connect to).
#. They must be renewed every three months. But you will get a reminder in
   advance.

Certbot
=======

Is a tool, you can obtain SSL certificates from *Let's Encrypt* with.
You can find it `here`_ or possibly via your Linux package manager.

It will ask you some questions, and very easily directly install and configure
your preferred web server software. I've used it very easily with ``nginx``.

It's very quick and easy to obtain certificates for multiple URLs with one run.

.. _here: https://certbot.eff.org
