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
https://www.everythingcli.org/ssh-tunnelling-for-fun-and-profit-autossh/
https://raymii.org/s/tutorials/Autossh_persistent_tunnels.html

On local PC
	Run simple http server
		python3 -m http.server
	Start ssh remote tunnel
		autossh -N -f -M 0 -o "ServerAliveInterval 30" -o "ServerAliveCountMax 3"  -R 1234:localhost:8000 user@remote_server_ip
		autossh -N -M 0 -R 1234:localhost:8000 user@remote_server_ip &

On remote server 
	Edit/etc/ssh/sshd_config
		#AllowTCPForwarding = yes
		GatewayPorts  = yes
	Check port status
		netstat -tln

* Now on connection to remote_server_ip port 1234 will be forward to your local PC port 8000
