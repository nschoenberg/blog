---
title: "Continues Integration - ASP.Net"
date: "2011-12-05"
categories: 
  - "net"
tags: 
  - "asp-net"
  - "hudson"
  - "jenkins"
---

![](https://wiki.jenkins-ci.org/download/attachments/327683/JENKINS?version=1&modificationDate=1302750804000 "Jenkins")Zur Zeit beschäftige ich mich mit dem Continues Integration Server [Jenkins](http://jenkins-ci.org/ "Jenkins CI"). Nachdem ich 2 WinForms Anwenunden erfolgreich integriert hatte, wollte ich noch ein ASP.Net Projekt hinzufügen. Bei den WinForms Anwendungen war die Integration wirklich simpel - einfach das Project File mit MSBuild aufrufen, Unit Tests laufen lassen, nochmal StyleCop drüber rennen lassen und alles brav als Zip-Archiv in dem Deployment Ordner ablegen. Nach ein paar Stunden Einarbeitung waren die Projekte angelegt.

Nun also das ASP.Net Projekt... Leider war das nicht ganz so einfach. MSBuild hat mir diverse Fehler um die Ohren gehauen. Ich beschäftigte mich dann intensiver mit custom MSBuild Scripts und bin unteranderem [auf dieses Video von Microsoft](http://www.asp.net/web-forms/videos/aspnet-35/how-do-i/how-do-i-use-msbuild-to-automate-the-aspnet-compiler-and-merge-utilities "MS How Do I Video") gestoßen. Es erklärt sehr schön was genau MSBuild eigentlich macht, wenn ein ASP.Net Projekt gebaut werden soll. Ich konnte das Beispiel sehr gut für meine Zwecke nutzen und habe es somit in Jenkins integriert.

Danke, Microsoft :)
