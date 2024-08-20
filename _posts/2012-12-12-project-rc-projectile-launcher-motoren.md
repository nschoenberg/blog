---
title: "Project R/C Projectile Launcher - Motoren"
date: "2012-12-12"
categories: 
  - "allgemein"
  - "entwicklung"
tags: 
  - "rc-projectile-launcher"
coverImage: "floppydrive_disassembly.jpg"
---

Hi,

Ziel des Projektes ist es ein Ferngesteuerten Projektilwerfer zu Entwickeln. Der Projektilwerfer soll in alle Himmelsrichtungen gedreht werden und Gummigeschosse abfeuern können. Am Ende möchte ich gerne mit meinem Handy via Bluetooth oder W-LAN Befehle an ein [BaegleBone](http://beagleboard.org/bone) geben, um den Projektilwerfer zu steuern. In Teil 1 geht es um die Motoren die ich verwenden möchte.

<!--more-->Motiviert durch Arbeitskollegen die mich mit Spielzeugwaffen und dazugehörigen Weichkörperprojektilen abgeschossen haben, hatte ich zuerst überlegt mir einen USB Raketenwerfer zu kaufen, die es auf diversen Seiten zu kaufen gibt. Allerdings waren mir die Teile mit 25€ aufwärts etwas zu teuer - Außerdem habe ich in ein paar Videos gesehen das die Projektile weder gerade, noch besonders weit fliegen.

Ich hab mich dazu entschlossen mich daran zu versuchen so ein Gerät selber zu bauen. Ich habe mir viele Gedanken gemacht zu der Steuerung und welche Elemente ich verwenden möchte. Meine Gedanken gingen zuerst in Richtung Lego Technik und dort vorhandene Motoren und Pneumatik Elemente, dann Eigenbau aus Holz und Metall so wie weitere Ideen. Ich bin noch zu keinem endgültigen Entschluss gekommen.

Was ich aber jedenfalls schon weiß: Ich möchte den Projectile Launcher **mit Motoren in alle Himmelsrichtungen drehen** können - Also woher Motoren nehmen? Legomotoren einzeln kaufen ist schon recht teuer, da hätte ich auch gleich ein fertigen Launcher kaufen können...

\[caption id="attachment\_769" align="alignright" width="150"\][![Disketten-Laufwerk Einzelteile](images/floppydrive_disassembly-150x150.jpg)](http://grrbrr.de/wp-content/uploads/2012/12/floppydrive_disassembly.jpg) Disketten-Laufwerk Einzelteile\[/caption\]

Die günstigste Lösung: Schritt-Motoren aus alten Disketten-Laufwerken. Auf dem Bild ist zu erkennen wie der [Stepping-Motor](http://de.wikipedia.org/wiki/Schrittmotor "Schrittmotor Wikipedia") aussieht. Er dient dazu den Schreib / Lesekopf des Diskettenlaufwerkes vor und zurück zu schieben. Da mein Motor 4 Kabel hat, gehe ich davon aus das es ein 3-Schritt Motor ist (Ein Kabel für Power, 3 für die einzelnen Schritte).

Von diesen kleinen Kerlen brauche ich mindestens Zwei: 1x links rechts und 1x hoch runter. Vielleicht noch einen dritten für den Abschussmechanismus, aber das habe ich mir noch nicht genau überlegt.

Als nächstes werde ich meine Motoren auf einem Schaltbrett digital ansteuern und zum Drehen bringen - Die Ergebnisse präsentiere ich dann als Blog-Eintrag.
