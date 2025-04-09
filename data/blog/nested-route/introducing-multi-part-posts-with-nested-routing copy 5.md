---
title: Observing Nmap
date: '2025-04-09'
tags: ['nmap', 'security', 'research']
draft: false
summary: 'This post is a deep dive into nmap packets.'
---

# What is NMAP

![NMAP](/static/images/nmap.png)

Network Mapper (Nmap) is a tool developed in early 1997 by Gordon Lyon, otherwise known as Fyodor. Today, Nmap is a standard utility in a system administrator’s toolbelt—allowing users to perform port scans, map networks, and carry out various other tasks.

Nmap is special because, instead of going through the normal network stack, it uses its own custom packet engine. This allows it to send and receive malformed packets, which standard network stacks might reject. Furthermore, the kernel's networking module can interfere with unusual TCP flag combinations—another reason why Nmap’s low-level control is so valuable.

This article promises to explore how Nmap actually interacts with the target host.

## Okay, But Why Do You Care To Deep Dive into NMAP?

For the past few years, I’ve been using Nmap as my go-to enumeration tool while penetrating networks. But the more I use it, the more I get this itch in the back of my mind: how am I actually interacting with other computers over the network? I want to see it with my own eyes—what does the target machine actually see when I run my scans?

## Lab Preface

I had a Raspberry Pi sitting inside my PC for over a year, and a System76 machine hiding in my closet. I just couldn’t bring myself to set either of them up. So, I opted to go cloud—after all, we’re living in the 2020s.

Going with the cloud turned out to be a solid choice. It took just seconds to get up and running, and I had a public IP address in no time. I wanted to simulate a real-world environment—and that’s exactly what I got.

Now that I’ve wrapped up this little tinkering session, I’m definitely curious to compare how AWS machines stack up against DigitalOcean ones—but that’s a conversation for another time.

## Results of standard scan.

First, I was curious to see what a standard Nmap scan would reveal about my freshly built droplet. I wanted to simulate a Red Hat-based environment, so I chose Rocky Linux—since it’s probably the closest free distribution to RHEL that’s still actively maintained (last I heard, CentOS is no longer keeping pace with upstream RHEL).

In theory, Nmap’s default scan (as root) performs a stealth scan. This means the client—Nmap—does not complete the full TCP handshake. If a service replies with a SYN/ACK, Nmap immediately sends a RST to tear down the connection. Ironically though, this scan doesn’t feel all that “stealthy,” since I was still able to capture my own IP address in the PCAP.

From a quick ChatGPT consult, I also learned that successful connection attempts are often logged, even if the connection is short-lived.

(Note: After visually observing how Nmap interacts with a target host, I’m really intrigued by the idea of writing detection logic for Nmap scans—or even tricking someone who’s scanning your network. I plan to explore this further.)

As theory predicted, my PCAP file showed exactly what I expected. My client sent a series of SYN requests to a wide range of ports on the target machine. I noticed that:

The source ports were randomized, and

The order in which Nmap scanned ports was also randomized.

(Tip: Open the image in a new tab or save it to your camera roll for better resolution.)

Another note, I actually did not run this command with root privledges, so inspecting deeper, i noticed that the full handshake occures with an open port, ending with my client sending a rst, ACK packet.

![WireShark](/static/images/wireshark.png)

## Implications

Being able to observe Nmap at the packet level openened many implications when looking at system admin, from a security perspective. Depending on the use case for the system will most definitaly shape how you plan to setup detections and preventions. After a light look into possible solutions, it looks like you can actually manipluate how your OS stack replies to certain packets. For example, there seems to be software already buit by the secuiryt computer, known as a middlebox, where after the server replies with a SYN ACK packet, the middle box can quickly inject a fake RST packet. The fundemental idea is you can essentially create trip wires, to confuse nmap, and by association the attacker, as to the ports state. This is defintiley something im going to look into.

## Whats Next

Next, I’m going to dive into how OS detection works. I want to capture the traffic during a scan and see exactly what Nmap sends — and what its fingerprint database looks like behind the scenes.

Nmap is such a fundamental tool in a system administrator’s toolbelt that I believe it deserves hours of focused research. My goal is to understand precisely how the tools I use interact with the systems I’m scanning.

This was a super fun and insightful exercise — and just the beginning

---
