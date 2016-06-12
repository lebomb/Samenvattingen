# Databanken II
## Hoofdstuk 8: Datawarehousing & Business Intelligence
### Inleiding
---
Data warehousing zit in de lift
* Groeiende nood aan flexibele business
rapportering toegankelijk voor de business
* Data die in DWH zit groeit exponentieel
    * terabytes aan data in een DW is 'gewoon' geworden
* Applicaties die gebruik maken van die data worden
complexer
    * traditionele rapportage
    * geavanceerde analyses
* Traditionele DBMS bieden allemaal DWH faciliteiten aan

### Data Warehouse
> Een **data warehouse** is een geïntegreerde, subject georiënteerde,
tijd variante en niet vluchtige verzameling van data ter
ondersteuning van beslissingen die genomen moeten worden op
management niveau

### Eigenschappen: 
**Geïntegreerd**
* Data is afkomstig uit verschillende bronnen (data uit verschillende bronnen is dikwijls inconsistent, in het DW is alles consistent geïntegreerd).

**Tijd**
* Data in het DW is accuraat en geldig op een bepaald punt in de tijd of
over een bepaald tijdsinterval
* Data wordt over heel lange periodes bijgehouden
* Tijd kan expliciet of impliciet geassocieerd zijn met de data
    * vb. Eenheidsprijs van een product kan in de tijd (historisch) worden bijgehouden of
    kan een momentopname zijn
    * DW zal, door geregelde kopieën te nemen, de historiek opbouwen
* Mogelijkheid om terug te gaan naar een momentopname
	
**Niet vluchtig (non volatiel)**
* Data wordt niet real-time ge-update maar op regelmatige basis
bijgewerkt met data uit operationele systemen
* Nieuwe data komt incrementeel bovenop oude data

**Geaggregeerde data**
* Afkomstig van bijv. GROUP BY

### Doelstellingen DWH
* Rapportering
* Analyse van events in verleden of heden
* Voorspellingen op basis van trendanalyse, historisch
* Multidimensionele rapportering
* Eindgebruiker vereenvoudigde rapporteringsomgeving bieden
(empowerment)
* Data mining

### Voordelen

**Hoge Return on Investment (ROI)**: het opzetten van een Datawarehouse is een zware investering maar levert een hoge ROI na een relatief korte periode.

**Concurrentieel voordeel:** doordat beleidsmakers toegang hebben tot voorheen niet beschikbare, ongekende of ongebruikte informatie over bv. klanten, trends, ...

**Verhoogde productiviteit:** de beleidsmaker krijgt één grote consisente view op de onderneming doordat het DW data uit verschillende bronnen integreert tot een consistent geheel, dat subject georiënteerd is en waar historiek zit ingebakken. Ze kunnen ook meer substantiële, nauwkeurigere en consistentere analyses maken (via tools kan de data automatisch tot bruikbare informatie omgevorm worden).

### Vergelijk On-Line Transaction Processing en DW
#### aka OLTP en OLAP (On-Line Analytical Processing) 


| Eigenschap | OLTP | DW |
| --- |--- | --- |
| Hoofddoel | ondersteuning van operationele verwerking | ondersteuning analytische verwerking |
| Leeftijd van de data | actueel | historisch (maar trend naar meer actueel) |
| Data wachttijd | real-time| afhankelijk van hoe frequent het DW wordt bevoorraad (maar trend naar real-time) |
| Data granulariteit |  detail  |  detail, maar ook in kleine of hoge mate samengevat  |
| Data processing | voorspelbare insert, delete, update en select opdrachten, veel transacties per tijdseenheid   |onvoorspelbare selects, relatief laag aantal transacties per tijdseenheid |  
| Rapportage | voorspelbaar, eendimensionaal, relatief statisch | onvoorspelbaar, multidimensioneel, dynamisch|
| Gebruikers | groot aantal gebruikers op operationele niveau| klein aantal gebruikers opmanagement niveau (maar trend naar ondersteuning voor analytische behoeften van operationele gebruikers |
