---
title: Building Enterprise Enviroment Lab - 1
date: '2023-07-09'
tags: ['Suricata', 'ELK Stack', 'Kali Linux']
draft: false
summary: 'This post is a technical analysis of the planning and construction of a lab environment; this is meant to replicate modern IT enterprise environments. The technologies being used in the construction of this lab environment includes'
---

# Introduction:

This post is a technical analysis of the planning and construction of a lab environment; this is meant to replicate modern IT enterprise environments. The technologies being used in the construction of this lab environment includes:

- [Virtual Box](https://www.virtualbox.org/)
- [Windows Server 2019](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019)
- [Pfsense](https://www.pfsense.org/download/)
- [Ubuntu Linux](https://ubuntu.com/)
- [Microsoft Windows](https://www.microsoft.com/en-us/software-download/windows10ISO)
- [Suricata](https://suricata.io/) _Note: This was orignally Sec Onion, Later Changed_
- [ELK Stack](https://www.elastic.co/elastic-stack/?ultron=B-Stack-Trials-AMER-US-W&gambit=Stack-ELK-EXT&blade=adwords-s&hulk=paid&Device=c&thor=elk%20stack&gclid=Cj0KCQjwtamlBhD3ARIsAARoaEw1n2s3hpD5HCCM7wf3yiIHsl_uEmCm43SZm6KnkEeYnuoH0Wz9x1IaAjNaEALw_wcB)
- [Kali Linux](https://www.kali.org/)

## Topology:

![Topology](/static/images/enttop.PNG)

## Purpose:

The purpose of constructing a small-scale enterprise environment is to garner experience of configuring and deploying technologies. Moreover, this architecture will provide a baseline for future endeavors such as experimenting with modern `TTPs` (tactics, techniques, and procedures) of various actors.

As technology evolves, modern business has transitioned to a more hands off approach to the IT systems that help drive business goals. Instead of installing and configuring their IT to a physical location, they have abstracted the need for physical hardware by using cloud services such as Amazon Web Services (AWS) and Microsoft’s Azure. Furthermore, many businesses needed software are being abstracted off the user endpoints environment to software that is also hosted on the cloud. Having everything hosted on the cloud establishes a new paradigm to security and IT. However, although trend analysis predicts efforts should go into cloud security engineering, gaining a broader deeper approach to operating systems, networks, and servers still show to be fruitful; these are the building blocks for the cloud. The objective of this lab is to provide hands on technical training and stimulate system analysis ability. Not only will this lab put my theoretical knowledge to the test but provide a pathway for more in depth training

## Part 1: Construction

The first step of the construction of this lab is to architect the environment. This lab will use `VirtualBox`, oracle proprietary virtual machine software, to build the landscape for multiple hosts. Not only does VirtualBox provide a means to run software and operating systems in their own sovereign environments but connects all of these systems through virtual networking.

### Technical Implications

Deciding to embark on this quest, building this mini enviorment, invovles a few considerations - specifically networking. The construction is being done on a `Type 2 Hypervisor`. This gives me legroom for less network configurations than having physical routers, servers and switches. That being said, the layer 3 switch which will act as a public facing router for this lab; there will be some subnetting and portforwarding invovled. `VirtualBox` has features out of the box that will automatically configure my host machines to my home's network. However, i rather keep this lab quarantined for security implications, _Pfsense will allow me to do so_. After reading [Virutal Box Networking Doc](https://www.virtualbox.org/manual/ch06.html), the best way to go about this is to setup each machine using a bridged mode, not NAT (default networking mode on VB). Although every device on my homenetwork is invovled with the proccess of netowrk address translation, as i only have have one public facing IP; the proccess on VirtualBox makes it so all of my machines will go through the hosts network stack. Using bridge mode allows me ignore this, which will help me with regards to avoiding any host network setting configurations. Another consideration was to make sure I had enough IPs to give out to these virtual machines.

Using CMD, i was able to confrim my assumption that I had a `Class A Netowrk` - giving more more addresses than I'll ever need. (_Note: My home router, which will be for the most part invisible in this lab, other than using it as a mean to install dependecies, is configured to subnet my network to a CIDR of `/24`_)

#### Command Line Interface - Confirm Network Settings

```CMD
ipconfig
```

![CMD](/static/images/cmd1.PNG)
