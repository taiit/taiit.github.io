---
title: "Create Free Google Computer Instance."
categories:
  - Cloud 
  - Computer Engine
tags:
  - setup
  - config
---
 Google provices small computer engine f1-micro with 30 GB standard storage without any charge.
 Detais:
 https://cloud.google.com/free/docs/gcp-free-tier
 https://medium.com/@hbmy289/how-to-set-up-a-free-micro-vps-on-google-cloud-platform-bddee893ac09
 
 1. Setup static ip address
 2. Install tools:
 > sudo apt-get install virtualenv nginx
 
3. Config nginx
> sudo service nginx status

> Thing board log
 /var/log/thingsboard/
 
 Raspberry
 -> thingsboard run at port 8080
 ssh -R 8080:localhost:8080 f1-micro-pc
