---
title: "Part 2: Using CloudFlare to set up DNS"
description: This is a post describing how to set up DNS on cloudflare.com.
date: 2020-12-14
tags:
  - cloudflare
layout: layouts/post.njk
image: https://gist.githubusercontent.com/0-vortex/8c1147b957f88b7ceaa85d3b22843803/raw/b04507cbedd3e21055df25c28b281e94bb119117/splash-cloudflare.jpg
---

## What is CloudFlare

[![Cloudflare splash logo](https://gist.githubusercontent.com/0-vortex/8c1147b957f88b7ceaa85d3b22843803/raw/b04507cbedd3e21055df25c28b281e94bb119117/splash-cloudflare.jpg)](https://www.cloudflare.com)

[Cloudflare](https://www.cloudflare.com) acts as an intermediary between a client and a server, using a reverse proxy to mirror and cache websites. By storing web content for delivery on the closest edge server, it is able to optimize loading times. That also allows it to modify content, such as images and rich text, for better performance.

This intermediary design is also how Cloudflare offers a level of filtration for security. By sitting between the client and the hosting server, it can detect malicious traffic, intercept distributed denial-of-service attacks, deflect attacks from bots, remove bot traffic and limit spam.

## Using CloudFlare

The most common use case for CloudFlare is to set it up as a DNS manager and reverse proxy static CDN.

### Sign up for a new account

In order to successfully configure a domain to be used with CloudFlare you need to first set up an account.

![sign up](https://gist.githubusercontent.com/0-vortex/8c1147b957f88b7ceaa85d3b22843803/raw/e9b2f4bba1d9023c87881108bed95f8b1490683a/cloudflare-register.png)

It would be advised to enable multi-factor authentication once your account is also verified by going to "My Profile" and then "Authentication".

### Add a new domain

Now it's time to add your new domain to CloudFlare: 

![add site](https://gist.githubusercontent.com/0-vortex/8c1147b957f88b7ceaa85d3b22843803/raw/e9b2f4bba1d9023c87881108bed95f8b1490683a/cloudflare-add-site.png)

You will then be presented with an option to select a plan, "Free" for "$0 / month" is enough:

![add site plan](https://gist.githubusercontent.com/0-vortex/8c1147b957f88b7ceaa85d3b22843803/raw/e9b2f4bba1d9023c87881108bed95f8b1490683a/cloudflare-add-site-plan.png)

### Point IP to GitHub Pages

CloudFlare will then scan your domain which should have some sensible defaults you can safely delete.

Delete all records and then add the GitHub Pages static IPv4 addresses as an ``A`` record for ``@`` (you can leave TTL to auto):
- ``185.199.108.153``
- ``185.199.109.153``
- ``185.199.110.153``
- ``185.199.111.153``

![add site dns](https://gist.githubusercontent.com/0-vortex/8c1147b957f88b7ceaa85d3b22843803/raw/e9b2f4bba1d9023c87881108bed95f8b1490683a/cloudflare-add-site-dns.png)

### Adjust NS

Final step is to change your existing name servers to the ones provided by CloudFlare:

![change name servers](https://gist.githubusercontent.com/0-vortex/8c1147b957f88b7ceaa85d3b22843803/raw/e9b2f4bba1d9023c87881108bed95f8b1490683a/cloudflare-add-site-registrar-ns.png)

Make your adjustments and when ready click continue. If you configured your domain as part of the registration process a summary style page should appear, otherwise you will be taken to your new site dashboard.

![add site summary](https://gist.githubusercontent.com/0-vortex/8c1147b957f88b7ceaa85d3b22843803/raw/e9b2f4bba1d9023c87881108bed95f8b1490683a/cloudflare-add-site-finish.png)

## CDN configuration

Below are some recommended settings for enabling full SSL compatibility with GitHub Pages.

### SSL settings

Click on "SSL/TLS" tab and make sure encryption is set to "Full":

![ssl settings](https://gist.githubusercontent.com/0-vortex/8c1147b957f88b7ceaa85d3b22843803/raw/e9b2f4bba1d9023c87881108bed95f8b1490683a/cloudflare-config-ssl.png)

Next, click on "Origin Server" and install a new certificate:

![certificate settings](https://gist.githubusercontent.com/0-vortex/8c1147b957f88b7ceaa85d3b22843803/raw/e9b2f4bba1d9023c87881108bed95f8b1490683a/cloudflare-config-ssl-certificate.png)

### Caching

Click on "Caching" tab and enable HSTS by setting the "Max Age Header" to a relevant maximum and ideally everything else to "On":

![cache settings](https://gist.githubusercontent.com/0-vortex/8c1147b957f88b7ceaa85d3b22843803/raw/e9b2f4bba1d9023c87881108bed95f8b1490683a/cloudflare-config-cache.png)

### Page rules

Last step is to go to "Page Rules" tab and adding a ``http://*.domain.tld/*`` rewrite rule to always use HTTPS:

![page rules](https://gist.githubusercontent.com/0-vortex/8c1147b957f88b7ceaa85d3b22843803/raw/e9b2f4bba1d9023c87881108bed95f8b1490683a/cloudflare-config-page-rules.png)
