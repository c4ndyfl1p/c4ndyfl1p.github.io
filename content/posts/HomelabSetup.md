+++
author = "c4ndy"
title = "Homelab setup (in progress)"
date = "2022-08-29"
description = "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
tags = [
    
    
]
categories = [
    "self hosting",
    "servers",
    "homelab"
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]

+++

**getting a server**

Natural habitats of servers are data-cantres, and they live off of power and internet. However, amongst other things, i wish to have my own cloud and happen to be a perpetually broke college student. While I host a lot of services on my VPS for 400 bucks a month, storage required to save all my media is expensive. So I'm gonna use a raspberry pi with a hard disk to host my next cloud.

**Part 1: Get your server ready**

1. Flash an ubuntu server image using balena etcher on a Raspberry Pi 4
2. Connect it to ethernet(i can never get it working in headless mode :/)
`ssh ubuntu@<address of pi>`
password: ubuntu
change passworsd to something else
3. VVV imp: update stuff and get security updates
`sudo apt update`
`sudo apt dist-upgrade`
4. Connect it to wifi if you have not. Ideally if you can, run it off ethernet. (and pray know one knocks it over randomly)
