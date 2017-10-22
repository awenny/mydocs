****************
SSL Certificates
****************

If you want to setup a webserver, you might want to set it up will SSL support.
But unfortunately, SSL certificates usually cost a regular fee.

Luckily, there is a service, you can obtain SSL certificates from for free.
Although, they must be renewed every 3 months.

Let's Encrypt
=============

This nice services (https://letsencrypt.org) provides you with free
certificates. But there are two important things to know:

#. With the tool ``certbot`` you should obtain certificates from your command
   line
#. The URL(s) you're registering must be available from the internet, as Let's
   Encrypt tries to connect to it.

Certbot
=======

Is a tool, you can obtain SSL certificates from different authorities with.
You can find it `here`_ or via your Linux package manager.

It will ask you some questions, and very easily directly install and configure
your preferred web server software. I've used it very easily with ``nginx``.

.. _here: https://certbot.eff.org
