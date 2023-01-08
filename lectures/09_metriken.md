<!--

author:   Andreas Heil

email:    andreas.heil@hs-heilbronn.de

version:  0.1

language: de

narrator: DE German Male

tags: devops, lecture, vorlesung, metriken
comment:  

-->


# DevOps - Ansible

<!-- data-type="none" -->
| Parameter | Kursinformationen |
| --- | --- |
| **Veranstaltung:** | `262062 DevOps`|
| **Semester** | `SEB4` |
| **Hochschule:** | `Hochschule Heilbronn` |
| **Inhalte:** | `DevOps Metriken` |
| Startseite | [https://liascript.github.io/course/?https://raw.githubusercontent.com/aheil/devops/master/README.md#1](https://liascript.github.io/course/?https://raw.githubusercontent.com/aheil/devops/master/README.md#1) | 
| **Link auf den GitHub:** | [https://github.com/aheil/devops/blob/main/lectures/09_metriken.md](https://github.com/aheil/devops/blob/main/lectures/09_mwtriken.md) |
| **Autoren** | @author |

## Ziele und Kompetenzen 

- Metriken für den Einsatz in DevOps **kennenlernen** 

## Motivation: Quantifizierung von DevOps

**Diskussion: Wie lassen sich die kulturellen und technologischen Änderungen (diese können etws verbessern oder auch verschlechtern) messen**

Anders ausgedrückt: Wie  kann der Erfolg von DevOps gemessen werden? Wie kann die Veränderung an das Management reported werden? Wie können Skeptiker überzeugt, Investoren oder Sponsoren informiert werden?

## DORA - DevOps Research and Assesment 

- Umfangreiches Studienmateril und Empfehlungen zur Umsetzung von DevOps 
- Wertet seit 2014 regelmäßig Umfragen in Bezug auf DevOps in Unternehmen aus 

  - gelebte organisatorische & technologische Praxis  

- Resultiert in den vier Metriken 

  - Deployment Frequency 
  - Lead Time for Change 
  - Time to Restore Service 
  - Change Failure Rate

  {{1}}
	************************************

  **Gruppierung von DevOps Teams**

- Low 
- Medium 
- High 
- Elite 

	************************************


  {{2}}
	************************************
	
  **Metriken, Lebel und Anforderungen von DORA [^1]**

|Metrik|Level Elite     |Level High|Level Medium|Level Low|
|Deployment Frequency   |on demand/mehrere Deployments pro Tag|einmal pro Woche bis einmal pro Monat|einmal pro Monat bis halbjährlich|seltener als halbjährlich|
|Lead Time for Changes  |unter einer Stunde|zwischen einem Tag und einer Woche|zwischen einem und sechs Monaten|mehr als sechs Monate|
|Time to Restore Service|unter einer Stunde|unter einem Tag|zwischen einem Tag und einer Woche|mehr als sechs Monate|
|Change Failure Rate    |0-15%|16-30%|16-30%|16-30%|

Video zum Report 2022: https://www.youtube.com/watch?v=gHQC7mClmzE

  ************************************

## Teamentwicklung 

Bassierend auf den gemessenen KPIs lassen sich die Teams einordnen und konkrete Maßnahmen definieren

{{1}}
************************************

**Level Low**

- Continuous Testing 
- Cloud-Infrastruktur
- Cloud-Verwaltung

************************************

{{2}}
************************************

**Level MEdium**

- Continuous Integration
- Copntinuous Delivery
- Software-Architektur 

************************************

{{3}}
************************************

**Level High**

- Lernkultur
- Begrenzung des Arbeitsumfangs -> WiP Limit
- Trunk-based Development [^2]

************************************

{{4}}
************************************

**Level Elite**

- DevOps-Kultur/Westrum-Organisationskultur [^3]
- Monitoring/Observability 
- Arbeitsaufteilung (Small Batches)

************************************




## Referenzen 

[^1]: Patrick Weimer, KPI-driven DevOps - das Resultat zählt, ix, Ausgabe 10 2022 S. 106 - 108  
[^2]: DevOps tech: Trunk-based development: https://cloud.google.com/architecture/devops/devops-tech-trunk-based-development  
[^3]: DevOps culture: Westrum organizational culture: https://cloud.google.com/architecture/devops/devops-culture-westrum-organizational-culture  
[^4]: Dave Farley, Did Microservices Break DORA?, https://www.youtube.com/watch?v=gHQC7mClmzE
