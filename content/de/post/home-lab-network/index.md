---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "HomeLab Network"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2021-03-24T16:51:51+01:00
lastmod: 2021-03-24T16:51:51+01:00
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


## vlan


https://wiki.mikrotik.com/wiki/Vlans_on_Mikrotik_environment

In switching technology, we have three modes of ports: Access, Trunk and Hybrid.

An access port should be used only with untagged packets. This kind of port is where you connect your PC to the switch.

An trunk port is capable of receiving and forwarding packets from multiple vlans. This one is to interconnect switchs.

An Hybrid port is a special mode that allow untagged and tagged packets on the same port. Imagine that you have a Voip desktop phone, you will connect your PC to the phone and the phone to the switch. We will have a vlan for voip and untagged data for the PC.

### Trunk port erstellen

Interfaces > + > VLAN

```
name eth1-vlan10
VLAN ID 10
Interface
```

### Access Port erstellen

vlN 10 BRIDGE 

bridge erstellen
bridge > + > bridge
bridge-vlan10

assign port to vlan bridge

Bridge > Ports Tab > + 

```
interface
Bridge
```

## wifi

Rules for wifi: (https://administrator.de/contentid/239551#comment-926324)

1. 802.11n Bandbreite fest auf 20 Mhz im AP Setup unter dem 2,4 Ghz Radio Setup !! Hier darf NIEMALS 40 Mhz oder "Auto" stehen.
Das ist genau das worauf die Zyxel Hotline Antwort etwas verklausuliert eingeht. Mit dem Auto oder 40 Mhz Betrieb wird die Bandbreite verdoppelt, die Sendeleistung bzw. Feldstärke damit abgeschwächt und die Störungen der Nachbar WLANs oder der eigen APs nehmen massiv zu bei doppelter Bandbreite. Andere in der Umgebung erkennen das ja nicht und funken dazwischen !
2. Die Bandbreite darf lediglich im 5 Ghz Bereich auf 40 Mhz eingestellt sein und nur da. Auch das solltest du im Setup entsprechend FEST einstellen !

power on AP
push reset button for 10 seconds to start in cap mode
go to switch where I want o

https://blog.effenberger.org/2017/08/27/wlan-zentral-verwalten-mit-capsman/


**What TX-power values can I use?**
The tx-power default setting is the maximum tx-power that the card can use and is taken from the cards eeprom. If you want to use larger tx-power values, you are able to set them, but do it at your own risk, as it will probably damage your card eventually! Usually, one should use this parameter only to reduce the tx-power.

In general, tx-power controlling properties should be left at the default settings. Changing the default setting may help with some cards in some situations, but without testing, the most common result is the degradation of range and throughput. Some of the problems that may occur are:

overheating of the power amplifier chip and the card which will cause lower efficiency and more data errors;
overdriving the amplifier which will cause more data errors;
excessive power usage for the card and this may overload the 3.3V power supply of the board that the card is located on resulting in voltage drop and reboot or excessive temperatures for the board.

## country code 


**channel-width** (

Use of extension channels (e.g. Ce, eC etc) allows additional 20MHz extension channels and if it should be located below or above the control (main) channel. Extension channel allows 802.11n devices to use up to 40MHz (802.11ac up to 160MHz) of spectrum in total thus increasing max throughput.
Channel widths with XX and XXXX extensions automatically scan for a less crowded control channel frequency based on the number of concurrent devices running in every frequency and chooses the “C” - Control channel frequency automatically.

20/40/80/160mhz-Ceeeeeee | 
20/40/80/160mhz-XXXXXXXX | 
20/40/80/160mhz-eCeeeeee | 
20/40/80/160mhz-eeCeeeee | 
20/40/80/160mhz-eeeCeeee | 
20/40/80/160mhz-eeeeCeee | 
20/40/80/160mhz-eeeeeCee | 
20/40/80/160mhz-eeeeeeCe | 
20/40/80/160mhz-eeeeeeeC | 
20/40/80mhz-Ceee | 
20/40/80mhz-eCee | 
20/40/80mhz-eeCe | 
20/40/80mhz-eeeC | 
20/40/80mhz-XXXX | 
20/40mhz-Ce | 
20/40mhz-eC | 
20/40mhz-XX | 
40mhz-turbo | 
20mhz | 
10mhz | 
5mhz; 
Default: 20mhz)



channel.control-channel-width (40mhz-turbo | 20mhz | 10mhz | 5mhz; Default: )	Defines set of used channel widths.


## 5 Ghz WLAN

Kanalbreite 20 Mhz

36 - 5180 - nur Indoor!
40 - 5200 - nur Indoor!
44 - 5220 - nur Indoor!
48 - 5240 - nur Indoor!
52 - 5260
56 - 5280
60 - 5300
64 - 5320

100 - 5500
104 - 5520
108 - 5540
112 - 5660
116 - 5580
120 - 5600 - Wetter
124 - 5620 - Wetter
128 - 5640 - Wetter
132 - 5660
136 - 5680
140 - 5725

5 Ghz WLAN mit Einschränkungen


## signalstärke

Je höher der angezeigte Wert bei der Signalstärke, desto schlechter das Signal (-0 dBm wäre ideal).
Werte zwischen -30 dBm und -40 dBM sind für ein WLAN gut, Werte jenseits von -85 dBm sind kritisch.




## internal local dns

## ipv6

## port forwarding


# opensense



# Resources

https://mikrotik.com/product/cap_ac
https://mikrotik.com/product/wap_ac

https://forum.mikrotik.com/viewtopic.php?t=155114#:~:text=Re%3A%20CAP%20AC%20WiFi%20throughput&text=Both%20devices%20can%20go%20up,12Mbps~54Mbps%20as%20supported%20rate.

https://wiki.mikrotik.com/wiki/Manual:Simple_CAPsMAN_setup

https://mum.mikrotik.com/presentations/ZA13/uldis.pdf
https://forum.mikrotik.com/viewtopic.php?t=83330

https://wiki.mikrotik.com/wiki/Manual:CAPsMAN#Master_Configuration_Profiles

https://forum.mikrotik.com/viewtopic.php?t=127742

https://wiki.mikrotik.com/wiki/Manual:Interface/Wireless
https://wiki.mikrotik.com/wiki/Manual:CAPsMAN

https://administrator.de/forum/mikrotik-wlan-best-practice-352964.html

https://administrator.de/contentid/239551#comment-926324

https://forum.mikrotik.com/viewtopic.php?t=125026

https://forum.mikrotik.com/viewtopic.php?t=15185

https://wiki.opennet-initiative.de/wiki/WLAN_Frequenzen
https://de.wikipedia.org/wiki/Wireless_Local_Area_Network