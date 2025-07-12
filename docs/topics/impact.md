---
tags:
  - Reporting
  - Scopus
  - SJR
  - Web of Science
---

# Impact Faktoren

Die Impact Faktoren (IF) sind auch in der heutigen Zeit noch für viele Institutionen und Forschende von Bedeutung, um die Qualität und den Einfluss von Publikationen zu bewerten. In OSIRIS werden die Impact Faktoren automatisch zu den Journals hinzugefügt, sofern sie verfügbar sind. Es ist aber anzumerken, dass es sich dabei nicht um den viel verwendeten Journal Impact Factor von Web of Science (Clarivate) handelt, sondern um den 2-Year Citedness Index von Skopus, der über den [Scimago Journal Rank](https://www.scimagojr.com/journalrank.php) (SJR) zur Verfügung gestellt wird. Dieser Index ist eine alternative Metrik, die auf der Anzahl der Zitationen in den vergangenen beiden Jahren basiert und von vielen Forschenden als nützlich erachtet wird.

## Warum nicht den Journal Impact Factor verwenden?

OSIRIS ist Open Source und setzt auf Transparenz und Offenheit. Der Journal Impact Factor von Web of Science ist ein proprietärer Index, der kostenpflichtig ist und nicht für alle Journale verfügbar ist. Selbst wenn euer Institut eine Lizenz für Web of Science hat, ist der automatische Zugriff (über eine API) auf den Journal Impact Factor oftmals nicht möglich, da man dafür eine weitere kostenpflichtige Lizenz benötigt. Für eine Open Source-Lösung wie OSIRIS, die besonders für kleinere Institute und forschungsnahe Organisationen gedacht ist, ist dies nicht praktikabel und entspricht auch nicht unseren Prinzipien der Offenheit und Transparenz.

Im Gegensatz dazu ist der Scimago Journal Rank frei zugänglich und bietet eine transparente Berechnungsmethode, die auf öffentlich zugänglichen Daten basiert. Daher haben wir uns entschieden, den Scimago Journal Rank als Standard-Impact-Faktor in OSIRIS zu verwenden. Allerdings gibt es die Einschränkung, dass auch Scimago keine API bietet, um die Daten automatisiert abzurufen. Ein Export als CSV-Datei ist aber möglich, weshalb wir einmal im Jahr die Daten von Scimago herunterladen und in die öffentliche OSIRIS-API importieren. Dadurch stehen die Impact Faktoren für alle Journale, die in Scimago gelistet sind, auch für eure OSIRIS-Instanz kostenlos zur Verfügung. Dies ist ein Service, der durch die OSIRIS Solutions GmbH bereitgestellt wird und für alle OSIRIS-Nutzer zugänglich ist.

