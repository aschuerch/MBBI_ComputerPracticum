---
layout: page
title: "Instructor Notes"
---

## Lock/unlock user accounts

Login to klif2.uu.nl

unlock with
```
sudo su
for i in {1..20}; do passwd -u mbbi$i; done
```

lock with
```
sudo su
for i in {1..20}; do passwd -l mbbi$i; done
```

## Print cheatsheet and list with usernames, passwords

At https://aschuerch.github.io/MBBI_ComputerPracticum/files/cheatsheet.pdf

## Update collaborative document

Before starting the practicum, remove previous content at https://pad.carpentries.org/MBBI_ComputerPracticum after line 36


## Share screen with participants

Share instructor screen with shellshare https://shellshare.net/
```
wget -qO shellshare https://get.shellshare.net
python shellshare -r workshop
```

## Whitelist users with too many login attempts


1. Find out about the IP address of the learners' computer by asking them to go to https://www.whatsmyip.org/

2. Login to klif2.uu.nl

3. Allow incoming connections from 192.168.0.1 (replace by users IP address)

```
iptables -A INPUT -s 192.168.0.1 -j ACCEPT
```
4. Allow outgoing connections to 192.168.0.1

```
iptables -A OUTPUT -d 192.168.0.1 -j ACCEPT
```
