---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "My New Server"
subtitle: ""
summary: ""
authors: []
tags: []
categories: [hardware]
date: 2020-02-10T18:25:59+01:00
lastmod: 2020-02-10T18:25:59+01:00
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

My old HP ProLiant MicroServer N54L is getting a little slow for me and I wanted to build a new server.

So lets get down the rabbit whole, shall we?

## Requirements:

* More cores! The two cores without HyperThreading and no Turboboost where just to slow for the docker containers I am running on that machine
* IPMI, I do want to have the possibility to debug the machine without attaching a Monitor with VGA
* PCI Express slots to use a simple HBA
* Space for 2 SSDs for the OS in mirror raid
* Extra space for at least 7 x 3.5" sata drives (does not need to be hotswap)

## Research

I decided to go with used enterprise hardware just because of price.
Even if you can get a lot of cores nowadays in consumer hardware as well today with the new AMD CPUs I did not want to spend to much on this.

Initially I wanted to go with the Haswell-based Xeons because the motherboards for this generation with the [LGA 1150](https://en.wikipedia.org/wiki/LGA_1150) socket do have USB 3.0 build in and more modern display connections and not only VGA.
Also the prices on ebay and the local classifieds were okay for this hardware.

## But then I got lucky

Unfortunalty I got this hardware for just ~20€:

* Motherboard: Supermicro X9SCM-F
* CPU: Intel XEON E3-1240 v2
* Memory: 4x ATP 8GB DDR3-1600 ECC
* Cooler: Cooler Master Hyper TX3i
* Powersupply: Seasonic SS-650HT Active PFC F3
* Extra: A huuuuuge case with some fans and a raid controller which I do not want to use (does not support HBA IT Mode)

Pretty neat and way better than my current setup.

The things I had to do for where:

- [ ] Reset the IPMI password
- [x] Renew the cmos battery
- [x] Remove the old dust
- [x] Buy a new CPU fan, the old one was broken
- [ ] Update IPMI firmware
- [ ] Update the BIOS to newest version
- [ ] Get a a new smaler case which I had to do anyway