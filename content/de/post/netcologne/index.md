---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: ""
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2020-05-24T23:22:43+02:00
lastmod: 2020-05-24T23:22:43+02:00
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: "HDD toaster, no food was wasted"
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

In diesem Blogpost geht es um Jan Böhmermanns Podcast Fest und Flauschig, Trojaner, NetCologne und eine Internetsperre.
Das scheint im Zusammenhang keinen Sinn zu machen, wird es aber nach dem lesen.

Über ein Jahr lang hat NetCologne mir mein Internet abgedreht.
Manchmal wenige Stunden, manchmal auch mehrere Tage.
Auch gerne mal übers Wochenende.
In total unregelmäßigen Abständen.
Mal war Monate alles okay, mal direkt ein paar Tage hintereinander.
Das macht dann auch besonders Spaß, weil ja mitlerweile alles übers Internet geht.
Selbst das Radio über den Smart Speaker.

Hier erfahrt ihr, wie ich mich gegen den Provider wehren musste, um mein Internet doch zu bekommen.

##

Der Grund der 
Am Anfang hab ich einfach die 2 Windows PCs im Haushalt neu aufgesetzt.
Kann ja schneller passieren so ein Virus oder Trojaner Befall.

Herauszufinden, was der Grund der Sperre ist, war schomal nicht so einfach.
Angeblich ein Virus oder Trojaner auf meinem PC.
Der Support hat sich hier auch mit sehr vielen Gegensätzlichen Aussagen geäußert.
Mal sollte es die FritzBox sein, dann das Handy, dann mein PC oder man könne das ja gar nicht sagen.
Ich hab nichts dagegen, wenn jemand etwas nicht weiß, allerdings einfach zu Lügen, um sich die Blöße nicht zu geben, dass man etwas nicht weiß geht gar nicht.
Vor allem hat es mir die Arbeit erschwert herauszufinden, was das Problem denn ist.

Dann jedoch 

## 




## Gespräch mit Antivien Spezialisten

## Wer hat denn Schuld

Die Situtation im allgemeinen ist für den Kunden schlicht weg einfach nur ne Karre Mist.
NetCologne als Provider ist verpflichtet Anschlüsse zu sperren, die unerwünschte Aktivitäten nachgehgen.

Wer bestimt, dass es sich um diese "bösen" Aktivitäten handelt?
Zum einen das BSI, dieses prüft, ob man von zu Hause aus zum Beispiel Spam verschickt, da man einen schlecht konfigurierten Server bei sich stehen hat.

Aber es gibt auch Dritte, also irgendwelche Firmen, die diese Aktivitäten prüfen und dann die Info an die Provider weitergeben.

Der Provider sperrt dann einfach.
In meinem Fall auch komplett ohne zu prüfen und ohne Beweise.


## Die Beweislast umkehren

Da 

## Erwischt

#### **# Internet Sperrungen 20.03.2020**

Am 20.03.2020 sitzte ich im Home Office und halte gerade eine kleine Team interne Schulung.
Plötzlich flieg ich aus Microsoft Teams raus.
Dachte mir nichts.
Der Linux Client ist alles andere als knorke und hab gewartet.
Bis ich dann verstanden hab, dass mir Netcologne schon wieder das Internet abgedreht hat!
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
> timestamp   ip_addr asn   port  proto  confidence   cc   notes  category    family
> 2020-03-19 05:58:14   89.0.116.134  8422  41003  6    50   DE   destaddr: 145.14.144.114/32; destport: 80    bot   azorult
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
