# How to use Proxmox Hypervisor

## What is Proxmox for?
Proxmox VE is a Type 1 (bare metal) hypervisor. This means it is an operating system, designed to run multiple virtual machines or containers. The VE stands for Virtual Environment.

There are many hypervisors to choose from. XCP-ng, Xen, ESXi.

Proxmox is possibly the most popular for small homelabs. The learning curve is one of the least steep, with it coming with its own UI, out of the box.

I won't get into how to install Proxmox. There are many tutorials for that. I'm going to assume you have it installed. 

I've used it for a few years now, and have learned from setup mistakes in the past. Here's how I like to have my Proxox nodes configured.

## How to best configure Proxmox

### IP/networking

Unusually I've recently begun to set up my nodes to use DHCP to get their IP addresses. This way I can manage ALL my home lab's IP addresses via my DHCP server. I use the DHCP server on my router to assign IPs based on their MAC address.

This allows me to reconfigure my networking more easily, without having to access each resource's config to change 10.1.1.10/24 to 10.2.2.10/24.

The same for VMs and containers running on the node; they all use DHCP to get their IPs. I configure all the IP distribution to my router's DHCP server.

If you decide to put multiple Proxmox nodes in a cluster, it might be wise to hardcode a static IP on the node. But for a single node, I've found DHCP is the best way to assign IP addresses

### Storage
I use mini PCs for my homelab. A small HP Tiny PC with a 7th generation i5 Intel CPU. It doesn't have my room for storage upgrades. Only a single NVMe slot and a 2.5" drive slot. My node has a 256GB NVMe drive and a 2TB 2.5" SSD.

To maximize this storage controversially, I install Proxmox on a USB drive, and have the node boot from that drive. That leaves both higher quality drives for the more important work of the VMs and containers. 

I install my VMs and containers to the faster 256GB drive, and leave the slower 2TB SSD as shared storage across all resources.

Using a USB drive is definitely not recommended for high-demand mission critical services, but it's worked great on 4 different Proxmox nodes for 2 years for me. 

### Container (LXC) or VM?
I've found for most services you can get away with an Unpriviliged LXC container. 

The only VMs I run are the UISP network management software, from Ubiquiti, and Home Assistant Operating System.

You can run Home Assistant as a Docker container, but I read somewhere that you're a bit limited that way, vs. running HAOS on its own virtual machine. For example: my understanding is that you can't access the Home Assistant Community Store, aka [HACS](https://hacs.xyz/), without running HAOS.

There are some circumstances where a full VM will give you the fewest issues, vs. a more lightweight LXC.

I've found I first try running a LXC container, and only going to a VM when I hit a roadblock. 

### helper-scripts.com
[Proxmox Helper Scripts](https://helper-scripts.com) is an amazing website for creating VMs or LXCs on Proxmox. 

The site provides a super easy way to get an application running on Proxmox. Apps like Plex, Home Assistant OS, MongoDB and hundreds of others.

You just copy a line of `bash` from the site, and paste into your Proxmox node's shell. The script will create a resource on Proxmox, install the requirements install the application, and get it running. In under 5 minutes you'll have the application running, and ready to use.

I use these scripts for 90% of the commonly used home lab services that I run.

### Coolify
This is one of the first LXCs that I install on my Proxmox nodes. Coolify is a free PaaS. It allows you to easily deploy your web apps. Whether it's NodeJS, or Python, or Docker, you  can setup Coolify to

1. re-deploy whenever there is a push to a configured git branch.
2. expose the application via a globally accessable URL
3. provide valid Let's Encrypt SSL certificates so your site is running with the same strength encryption as any site online.

