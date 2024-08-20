---
title: "Dualboot: Ubuntu 12.04 & Windows 8"
date: "2012-11-23"
categories: 
  - "allgemein"
tags: 
  - "dualboot"
  - "gparted"
  - "partitionierung"
  - "ubuntu"
  - "win8"
  - "windows-8"
---

Nachdem ich nun schon seit ein paar Monaten unter meinem Windows 7 System eigentlich ausschließlich Ubuntu in einer VM zum Entwickeln benutzt hatte und ich zusätzlich auf Windows 8 umsteigen wollte, wurde es nach 3 Jahren mal Zeit mein System neu zu installieren.

<!--more-->

# Long Story

Ich hab mich für Ubuntu als Hauptsystem und Windows 8 als Zweitsystem im Dualboot entschieden (Auf einer Festplatte) und bin auf einige Probleme gestoßen...

- Bei der ersten Windows 8 Installation hab ich vergessen auf der Platte Platz für Ubuntu zu lassen
- Nachdem ich Windows und Ubuntu schließlich auf eine Platte installiert hatte, konnte [GRUB2](http://wiki.ubuntuusers.de/GRUB_2 "Grub2 Ubuntuwiki") Windows 8 nicht korrekt booten und nach dem das Windows 8 Logo kurz zu sehen war, startete sich mein PC neu

Das zweite Problem hat mich recht lange beschäftigt. Wie ich schlussendlich herausgefunden habe, lag es an der "Reihenfolge" meiner Festplatten und an der Windows 8 Systempartition (Bei Win7 100MB, Win8 350MB groß - Sie enthält Bootloader und Bitlocker Routinen)

Das Windows Setup hat die blöde Angewohnheit die Systempartition immer auf die ERSTE ([Screenshot Laufwerk 0](http://blog.grrbrr.de/wp-content/uploads/2012/11/winsetup_before_system_partition.jpg)) Platte zu hängen. Zu erst hab ich gedacht das Windows Setup würde die Reihenfolge anhand des SATA-Controllers erlangen - Im BIOS habe ich nachgeschaut und tatsächlich hatte ich meine kürzlich erworbene Festplatte an den SATA Channel "Master 2" geklemmt. Meine alten Festplatten hingen an Master 3 und Slave 3.

Ich hab die Kabel also kurz geändert und bin wieder in das Windows 8 Setup eingestiegen - Keine Änderung. Meine kleinste Festplatte auf der ich Windows und Ubuntu installieren wollte, hing korrekt an Master 1, meine neue Festplatte an Master 2. Ich hab absolut keine Ahnung wie Windows die Laufwerksnummern bestimmt ([Und Laut diesem MSDN Eintrag weiß Microsoft das auch nicht so richtig...](http://support.microsoft.com/kb/937251/en-us))

Dem Windows Setup ist es auch ziemlich egal ob ich Laufwerk 1 als Installations-Ziel für Windows 8 auswähle, die Systempartition wird trotzdem auf Laufwerk 0 erstellt. Um zu verhindern das meine Systempartition auf einem ganz anderem Laufwerk erstellt wird als schlussendlich meine Window-Installation liegen wird, habe ich die Festplatte die aus mir unerklärlichen Gründen immer als Laufwerk 0 erkannt wurde, von meinem Motherboard getrennt. Beim nächsten start des Windows-Setup hatte ich die korrekte Platte als Laufwerk 0. Es ist wichtig das die die Partitionen (Systempartition, Installationspartition) auf einem Laufwerk sind, ansonsten erhält man den Fehler den ich weiter oben in Punkt 2 beschrieben habe. GRUB kommt damit nicht gut klar.

# Vorgehensweise

1. Als allererstes kann ich nur empfehlen alle Platten bis auf die Zielfestplatte vom Motherboard zu entfernen, um zu verhindern dass das Windows Setup ungewollter Weise seine Systempartition auf dem falschen Laufwerk anlegt
2. Falls nicht bereits vorhanden, [Ubuntu LiveCD runterladen](http://www.ubuntu.com/download/desktop) und brennen
3. Um Festplatten zu Partitionieren, von der LiveCD booten und im Setup "Ubuntu ausprobieren" auswählen
4. Auf dem Ubuntu Desktop oben Links das Dashboard öffnen, ein Terminal starten und das Programm [Gparted](http://wiki.ubuntuusers.de/GParted) mit dem Befehl "sudo gparted" starten

\[caption id="attachment\_722" align="alignleft" width="150"\][![](/blog/images/gparted_partition-150x150.jpg "gparted_partition")](http://blog.grrbrr.de/wp-content/uploads/2012/11/gparted_partition.jpg) Meine Partitionierung\[/caption\]

**Achtung:** Das Foto ist nach der Installation von Windows und Ubuntu entstanden, die erste Partition "System Reserviert" wird von Windows selbstständig angelegt und wurde nicht von mir erstellt - Bitte diese Partition NICHT manuell erstellen

**GRUB-boot:** Es wichtig, ext3 als Filesystem zu verwenden **Linux-Swap**: Faustregel für die Größe = RAM \* 2

Alle anderen Festplatten habe nur eine NTFS Partition - So kann ich mit beiden System auf die Daten zugreifen.

\[caption id="attachment\_726" align="alignleft" width="150"\][![test](/blog/images/winsetup_before_system_partition-150x150.jpg "winsetup_before_system_partition")](http://blog.grrbrr.de/wp-content/uploads/2012/11/winsetup_before_system_partition.jpg) Vor der Partitionierung\[/caption\]

Nachdem die Partitionen erstellt wurden, sollte man unbedingt als erstes Windows 8 installieren. Der Bootloader von Ubuntu erkennt das Windows 8 bereits installiert ist und nimmt ihn auf. Wird Ubuntu zuerst installiert und danach Windows, überschreibt das Windows Setup einfach den MBR und Ubuntu kann nicht mehr gestartet werden.

Auf dem Foto kann man sehen, wie das Windows 8 Setup die Partitionen von GParted auflistet ... Mehr schlecht als recht ^^ Bei unbekannten Dateisystemen wird nur die Größe angezeigt.

\[caption id="attachment\_728" align="alignleft" width="150"\][![](/blog/images/winsetup_after_system_partition-150x150.jpg "winsetup_after_system_partition")](http://blog.grrbrr.de/wp-content/uploads/2012/11/winsetup_after_system_partition.jpg) Nach der Partitionierung\[/caption\]

Nachdem eine Partition als Installations-Partition ausgewählt wurde, möchte Windows gerne seine System-Partition erstellen und verkleinert dazu den vorhandenen Platz. Die System-Partition wird immer auf Laufwerk 0 erstellt.

5. Sobald Windows 8 erfolgreich installiert wurde, kann man nun erneut von der Ubuntu LiveCD booten
6. Im Setup "Ubuntu installieren" auswählen und kurz warten; Folgende Auswahl wird angezeigt:
    - Ubuntu neben Windows 8 installieren
    - Windows 8 mit Ubuntu ersetzen
    - **Etwas anderes**
7. Bitte **"Etwas anderes"** auswählen

\[caption id="attachment\_735" align="alignleft" width="150"\][![](/blog/images/ubuntu_mountpoints-150x150.jpg "ubuntu_mountpoints")](http://blog.grrbrr.de/wp-content/uploads/2012/11/ubuntu_mountpoints.jpg) Ubuntu Installation\[/caption\]

Ich habe für das Betriebsystem und mein Home-Verzeichnis verschiedene Partitionen als Einhängepunkt (Mounting) ausgewählt.

**Achtung:** Unbedingt bei "Gerät für die Bootloader-Installation" die Festplatte an sich auswählen, nicht irgend eine Partition.

\[caption id="attachment\_736" align="alignleft" width="150"\][![](/blog/images/grub2_ubuntu_win8-150x150.jpg "grub2_ubuntu_win8")](http://blog.grrbrr.de/wp-content/uploads/2012/11/grub2_ubuntu_win8.jpg) GRUB 2 bei Systemstart\[/caption\]

Wenn alles soweit geklappt hat und Ubuntu erfolgreich installiert wurde, stellt GRUB2 dieses Menü beim Systemstart zur Verfügung.

# Nützliche Links

[Ubuntuusers - Dualboot](http://wiki.ubuntuusers.de/Dualboot) [Infos zu GParted](http://wiki.ubuntuusers.de/GParted) [Tipps zur Partitionierung](https://help.ubuntu.com/community/PartitioningSchemes)
