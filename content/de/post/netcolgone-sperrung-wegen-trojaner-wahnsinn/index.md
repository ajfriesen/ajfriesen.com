---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Netcolgone Sperrung Wegen Trojaner Wahnsinn"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2020-04-13T02:04:46+02:00
lastmod: 2020-04-13T02:04:46+02:00
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



# Internet Sperrungen 20.03.2020

Am 20.03.2020 sitzte ich im Home Office und halte gerade eine kleine Team interne Schulung.
Plötzlich flieg ich aus Microsoft Teams raus.
Dachte mir nichts.
Der Linux Client ist alles andere als knorke und hab gewartet.
Bis ich dann verstanden hab, dass mir Netcologne schon wieder das Internet gesperrt hat!
Die Sperr SMS kamm dann auch um 13:40.

Ich habe sofort die IP bei abuse@netcologne.de angefragt.
Die IP Adresse mit dem Timestamp kam sofort.

> Hallo Herr Friesen,
> 
> Im folgenden senden wir Ihnen den aktuellen Log zu der Vireninfektion.
> 
> Der Zeitstempel im Log ist in UTC angegeben.
> 
> Log:
> 
> timestamp     ip_addr asn     port    proto   confidence      cc      notes   category        family
> 2020-03-19 05:58:14     89.0.116.134    8422    41003   6       50      DE      destaddr: 145.14.144.114/32; destport: 80       bot     azorult
> 
> 
> 
> Mit freundlichen Gruessen,
> 
> NetCologne Abuse Team <abuse@netcologne.de>

Durch den Zeitstempel und der Destination IP konnte ich dann das Gerät ausfinding machen.
Es handelt sich um das Nokia 6.1 meiner Freundin.
Aktuelles Android 10 mit aktuellem Patch Level.
Kein root oder andere Appstores installiert.

