---
layout: post
permalink: http://blog.stangroome.com/2017/10/04/lessons-from-digitalocean-networking/
title: Lessons from DigitalOcean Networking
description: None
date: 2017-10-04 08:24:14 -0000
last_modified_at: 2017-12-12 22:01:02 -0000
publish: true
pin: false
categories:
- Uncategorized
tags:
- DigitalOcean
---
**Update:** On 2017-DEC-13, [DigitalOcean announced that private networking will be isolated to each account](https://www.digitalocean.com/community/tutorials/digitalocean-private-networking-faq) beginning February 2018.

* * *

If you've come from running virtual machines on AWS, Azure, or Google Cloud, you will be familiar with the idea that the VMs can have a public Internet-facing IP address and a private IP address, or some combination or multiple of the two options. DigitalOcean offers something similar, but just different enough to throw you when you're accustomed to the networking models of the other cloud providers. When you create a DigitalOcean Droplet via their Control Panel, or [via their API](https://developers.digitalocean.com/documentation/v2/#create-a-new-droplet), you have the option to enable "Private networking" but when you read [the official documentation](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-use-digitalocean-private-networking), this feature is actually called "Shared private networking" and it is a very important distinction. Where private networking in AWS, Azure, or Google Cloud gives your VM a private interface to a network shared only with your VMs, the shared private networking in DigitalOcean is, according to [this DigitalOcean tutorial](https://www.digitalocean.com/community/tutorials/how-to-isolate-servers-within-a-private-network-using-iptables), "accessible to other VPSs in the same datacenter--which includes the VPSs of other customers in the same datacenter". And I have verified that statement is true. To clarify, if you enable private networking on a DigitalOcean VM in their SFO2 region, every other VM in the SFO2 region from every other DigitalOcean customer can route packets to your VM's private network interface. While I advocate the use of strict firewall configurations in any cloud hosting environment, the importance of doing so correctly is much higher on DigitalOcean, even for non-Production environments where firewalls have a history of being more relaxed. The bright side of all this is that DigitalOcean's [tag-based Cloud Firewall](https://www.digitalocean.com/community/tutorials/how-to-organize-digitalocean-cloud-firewalls#using-tags) applies to both the public and private network interfaces and implements a deny-by-default behaviour. By using tags to restrict which other droplets are permitted to communicate on specific ports and protocols you can achieve a very similar level of isolation as offered by other cloud providers. There is another caveat though: to improve the security of this shared private networking environment, [DigitalOcean do not allow](https://www.digitalocean.com/community/questions/nat-gateway-on-digital-ocean-s-droplet-possible?answer=13896) VMs to send packets with a source IP address that does not match their assigned private IP address. This prevents you, for example, from operating one DigitalOcean VM as a Virtual Private Network gateway for your other DigitalOcean VMs to connect through to another non-DigitalOcean private network. In summary, while DigitalOcean is providing a great service, and adding new features seemingly every quarter, it offers a conceptual model slightly out of sync with the big name cloud companies, and you need to by mindful of this, but the same would be true I guess for people experienced with DigitalOcean moving to AWS or Azure.
