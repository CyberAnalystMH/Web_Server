# Setting up a Web Server Overview

# Summary

One of the most used cases for Linux Servers is a web server, making the backend code is one thing but deploying the actual web server is a large part of the task for creating a usable and stable web server. 

**This repo will update overtime:**

**Last Update:** 6/8/2026
# Setup

**Theme/Scenario:** Let's say I'm famous and everyone wants to hire me/my company, I made a website where companies puts first name, last name, email and phone number in order to contact me and my small team, that data is stored in a DB. 
 
Now, I don't know any HTML code outside of "Hello World" but Linux Admins are usually supplied with a great HTML or CSS code and we're gonna imagine that I got supplied with one.

Here's the exact simple HTML code I'm using: https://codepen.io/terf/pen/bGRavY

**My made-up tasks are to:** 
- Build a Web Server using HTTP
- Creating a DB for the Web Server
- Securing a Web Server (Only Server side and not code side)
- Applying correct permissions 
- Adding other users for a collaborative projects 

**Hardware and System** 
- Rocky Linux (No GUI)
- 40GB of Storage
- 3 CPUs
- 6GB 
- VM on Proxmox

**Basic Configurations:** 
- For Hostname set-up: hostnamectl hostname-set **webserver** (Since this offline, no domain is specified)
- Static IP: nmcli to see IP, Then: nmtui (Assign static IP)
- Reboot (To ensure it works)

**Tools Used:**
- HTTPD
- MySQL
- MariaDB
- PHP
- Lynis
- Gocryptfs
- Firewalld
- SELinux
- Wazuh
- Suricata
- PAM
- Crontab
- VSFTPD
- Rsync

**Tools Used but not mentioned:**
- Libre Office - For documenting steps and procedures
- Fish - To speed up command workflow
- Btop - To get a nice visual monitoring on system
- GPG for file encryption 
