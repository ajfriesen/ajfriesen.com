---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "My New Home Server"
subtitle: ""
summary: ""
authors: [Andrej Friesen]
tags: [linux, supermicro,xeon]
categories: [Hardware]
date: 2020-02-10T18:25:59+01:00
lastmod: 2020-04-13T00:41:59+01:00
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: "New Server"
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

* More cores: The two cores without HyperThreading and no Turboboost where just to slow for the docker containers I am running on that machine
* IPMI: I do want to have the possibility to debug the machine without attaching a Monitor with VGA
* PCI Express slots to use a simple HBA
* Space for 2 SSDs for the OS in mirror raid
* Extra space for at least 7 x 3.5" sata drives (does not need to be hotswap)

## Research

I decided to go with used enterprise hardware just because of price.
Even if you can get a lot of cores nowadays in consumer hardware as well today with the new AMD CPUs I did not want to spend to much on this.

I also did find a really good resource for home server builds on [https://www.serverbuilds.net/](https://www.serverbuilds.net/).
The [serverbuilds forum](https://forums.serverbuilds.net/) is worth a look and especially the guides like the [NAS Killer 4.0 - fast, quiet, power efficient, and flexible - starting at $125](https://forums.serverbuilds.net/t/guide-nas-killer-4-0-fast-quiet-power-efficient-and-flexible-starting-at-125/667).
So much good information to get started.
It helped me a lot since I personally work with __the cloud__ and do not with hatdware and that got me up to speed.

Initially I wanted to go with the Haswell-based Xeons because the motherboards for this generation with the [LGA 1150](https://en.wikipedia.org/wiki/LGA_1150) socket do have USB 3.0 build in and more modern display connections and not only VGA.
Also the prices on ebay and the local classifieds were okay for this hardware.

## But then I got lucky

Unfortunalty I got this hardware for just ~20€, but I had to carry it by myself in the tram.
That thing was **really heavy**!

{{< figure src="big-server.jpg" title="Big Server" lightbox="true" >}}

* Motherboard: [Supermicro X9SCM-F](https://www.supermicro.com/products/motherboard/Xeon/C202_C204/X9SCM-F.cfm)
* CPU: [Intel XEON E3-1240 v2](https://ark.intel.com/content/www/de/de/ark/products/65730/intel-xeon-processor-e3-1240-v2-8m-cache-3-40-ghz.html)
* Memory: 4x ATP 8GB DDR3-1600 ECC
* Cooler: Cooler Master Hyper TX3i
* Powersupply: Seasonic SS-650HT Active PFC F3
* Extra: A huuuuuge case with some fans and a raid controller which I do not want to use (does not support HBA IT Mode)

Pretty neat and way better than my current setup.

The things I had to do:

- Reset the IPMI password
- Replace the cmos battery
- Clean the board and other components
- Replace thermal paste
- Buy a new CPU fan, the old one was broken
- Update IPMI firmware
- Update the BIOS to newest version
- Get a a new smaler case which I had to do anyway
  - I did choose the [Fractal Design Node 804](https://www.fractal-design.com/products/cases/node/node-804/black/)
  - It looks nice on the desk and has a lot of space for drives (10 x 3.5")
- Get rid of the old case (way to big for my flat)

## Buildprocess

Plenty of space in the left chamber.

{{< figure src="left-chamber.jpg" title="Left chamber" lightbox="true" >}}

The two drive cages can hold up to 4 x 3.5" HDDs.
The HDDs do have enough space for airflow inbetween but the cable was somewaht tight!

{{< figure src="hdd-cable.jpg" title="HDD drive cage cable management" lightbox="true" >}}

