---
draft: false
date: 2024-07-21
category: "Portfolio"
authors: 
    - jkoblitz
tags: [Portfolio, Webseite]
---

# Der Weg zu OSIRIS Portfolio

... und woher es seinen Namen hat

Seitdem wir OSIRIS entwickelt haben werden wir regelmäßig gefragt: wie kommen die Daten auf unsere Webseite. Denn sind wir mal ehrlich: OSIRIS ist eigentlich ein Forschungsinformations**management**system. Es dient hauptsächlich der intuitiven **Verwaltung** von Forschungsinformationen. Um die Forschung nach außen zu präsentieren, benötigt man ein weiteres System. 

<!-- more -->

Dazu werden in den meisten Einrichtungen die eigenen (meist von Fremdfirmen entwickelten) Webseiten verwendet. Es gibt unterschiedlichste Möglichkeiten, die Daten aus einem Forschungsinformationssystem (CRIS) auf die Webseite zu bekommen. Die am häufigsten verwendeten sind:

- Die Daten werden von der Webseite direkt über die API des CRIS abgefragt. Der Vorteil ist, dass die Daten immer aktuell sind. Der Nachteil ist, dass dies Entwicklungsarbeit erfordert, da die Webseite speziell an die API angepasst werden muss. Außerdem müssen bei Erweiterungen des CRIS eventuell auch Anpassungen am System erfolgen, was schnell kostspielig werden kann.
- Die Daten werden von einem zusätzlichen Skript zwischen CRIS und Webseite *vermittelt*. Dabei wird ein eigenes Skript geschrieben, dass die Daten in regelmäßigen Abständen (z.B. mittels CRON-Job) überträgt. Der Vorteil ist, dass bei Änderungen an der Webseite und am CRIS nur das kleine Skript angepasst werden muss. Der Nachteil ist, dass die Daten nicht immer aktuell sind, da die Übertragung meist zu festen Zeitpunkten geschieht.

Bereits letztes Jahr im Herbst hatte ich deshalb die Idee, eine Platform zu entwickeln, die eine vollumfängliche Lösung für die Webdarstellung bietet. Quasi ein perfekt abgestimmtes Forschungsportal, das mit OSIRIS verwoben ist und dadurch die perfekte Synergie bietet.

Und erneut war die **Findung eines Namens** schwieriger als die Entwicklung der Idee. Am Ende haben wir uns für OSIRIS Portfolio entschieden, denn der Name spiegelt alles wieder, wofür die Plattform steht:
1. Ein Portfolio ist der Definition nach eine Zusammenstellung von Dingen, die die Arbeitsleistung einer Person oder einer Institution darstellen. Beispielsweise ein Künstlerportfolio, ein Produktportfolio, etc.
2. Der Wortstamm *Port* steht im Zusammenhang mit IT für Schnittstellen. Portfolio greift auf die Schnittstellen von OSIRIS zu, um die relevanten Daten zu empfangen.
3. Der Wortstamm enthält außerdem die Buchstaben O und R, die ebenfalls die charakteristischen Buchstaben aus dem OSIRIS-Logo sind. Dadurch können wir ein Logo kreieren, dass direkt an OSIRIS erinnert.

![OSIRIS Portfolio](https://osiris-app.de/img/portfolio_logo.svg)

Entwickelt haben wir Portfolio in den vergangenen Monaten in enger Zusammenarbeit mit der [DSMZ](https://dsmz.de), die das System so bald wie möglich bei sich einsetzen will, um den Bereich "Research" auf ihrer Webseite komplett zu überarbeiten. Zuerst ist Portfolio wie OSIRIS auch in PHP entstanden. Doch die Limitation dieses Prototypen wurde schnell deutlich. Deshalb habe ich es in meinem Sommerurlaub komplett in ein modernes JavaScript-Framework umgeschrieben (Vue.js). Dadurch ist Portfolio modern, interaktiv und rasend schnell. Es greift auf eine eigens für Portfolio entwickelte REST-API zu, die natürlich auch gern für andere Zwecke genutzt werden kann. Dazu habe ich in der Dokumentation von OSIRIS einen neuen Reiter namens *Portfolio API Docs* hinzugefügt, in der alle Endpunkte mit Beispieldaten beschrieben sind. Diese API hat den Vorteil, dass sie nur das ausliefert, was wirklich gezeigt werden soll. Ihr könnt also auch von der Entwicklung profitieren, wenn ihr Portfolio nicht selbst nutzen wollt.

> Was genau Portfolio alles kann werde ich in den nächsten Wochen in weiteren Blog-Posts beschreiben und bald gibt es dann auch den Link zum Github. 


