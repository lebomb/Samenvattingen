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

## Architectuur
---
### Architectuur van een DWH
![alt text](http://puu.sh/pppKy/67445e577f.jpg "Architectuur schema")
* Operationele data
* Bronnen van data
	* mainframe (hiërarchisch, netwerk)
	* departementale data in bestanden en
RDBM systemen
	* privé data op werkstations en privé
servers
	* externe systemen
        * internet
        * commerciële DB
        * DB die bij de klanten of leveranciers gebruikt worden
* Operationele data source
* Repository
	* huidige, geïntegreerde data
	* voorbereidende stap in ontwikkeling van
het DWH, of,
	* ondersteuning van reporting services bij
legacy systemen
* ETL manager
	* ondersteunt alle operaties voor ETL
(Extraction, Transformation and Load) van data
        * rechtstreeks vanuit operationele
        gegevensbronnen
        * vanuit de operationele data store
* Warehouse manager
	* voor het beheer van data in het DWH
        * analyse van data om consistentie te
        garanderen
        * transformatie en samenvoegen van
        brongegevens van tijdelijke opslag in DWH
        tabellen
        * creatie van indexen en views
        * eventuele denormalisatie
        * aanmaken van aggregaten (samenvoegen
        van data)
        * back-up en archivering van de data
* Query manager
	* beheer van gebruikersqueries
	* gebruik van juiste tabellen
	* uitvoeren/schedulen van queries
	* generatie van profielen
	* voorstellen voor aggregaten en indexen
maken
* Gedetailleerde data
	* dikwijls niet online opgeslagen maar
beschikbaar door aggregatie van de data
in een hoger niveau van detail
	* deze data wordt op regelmatige basis aan
het DWH toegevoegd
* Samengevatte data
	* voorgedefinieerde samengevatte data
	* onderhevig aan veranderingen zodat kan
ingespeeld worden op verschillende
soorten queries
	* zorgt voor verhoogde performantie bij
query-uitvoering
* Archive/back-up data
	* voor zowel gedetailleerde als
samengevatte data
        * samengevatte data kan bv. een langere
        levensduur kennen dan de gedetailleerde  data
* Meta data
	* nodig voor ETL
	* nodig voor de DWH manager
	* nodig door de Query manager
* Meerdere kopieën van metadata die elk
afgestemd zijn op een bepaald proces
* Laten steeds toe de herkomst van een
item in het DWH volledig te bepalen
* End user access tools
	* reporting and querying
	* application development tools
	* OLAP tools
	* data mining tools

### Data Marts
> Een DB die bestaat uit een **deelverzameling van bedrijfsgegevens** ter ondersteuning van de behoeften van een bepaalde bedrijfsunit om **analyses** te kunnen uitvoeren, of, om gebruikers te ondersteunen die dezelfde behoeften hebben om bepaalde **bedrijfsprocessen** te analyseren

![alt text](http://puu.sh/ppqhT/763b11889d.jpg "Data Marts")

**Waarom een datamart?**
* om gebruikers toegang te geven tot de data die ze meest analyseren
* om data aan te bieden in een vorm die overeenkomt met de collectieve view van een groep van
gebruikers in een departement of van een groep gebruikers binnen eenzelfde bedrijfsproces
* om response tijd te verhogen door kleinere volumes van data aan te bieden
* om data aan te bieden in een vorm die past bij de tools die de eindgebruikers gebruiken (OLAP,
data-mining tools)
* reductie van complexiteit in het ETL proces
* reductie van de kost tov het opzetten van een enterprise wide DWH

### Data Mining Applications
* **Data mining** applicaties worden gebruikt
om:
    * what-if analyses te doen
    * voorspellingen te doen
    * het beslissingsproces te faciliteren
* Data mining applicaties maken gebruik van
gesofesticeerde statische en wiskundige
technieken
* Rapporten zijn minder kritisch

### Problemen met DWH
* **Kosten voor ETL worden onderschat**
	* extraction, transformation en loading van data in het DWH neemt groot deel van ontwikkeltijd in
beslag
	* Projecten duren meestal jaren
* **Verborgen problemen met de bron of ETL systemen**
	* worden soms pas na jaren ontdekt
	* oplossen gebeurt in DWH en/of operationele DB
	* vb. velden die null-waarden toelaten, in sommige kantoren laten ze die altijd op null staan,
hoewel de gegevens wel beschikbaar en nuttig zijn
* **Nodige data wordt niet bijgehouden**
	* verandering in huidige systeem of apart systeem voor deze data
	* vb. de datum waarop een klant zich registreert wordt momenteel niet bijgehouden
* **Verhoogde eisen van de eindgebruikers**
	* hoe meer gebruikers zich bewust worden van de mogelijkheden van het systeem,
hoe meer ze zullen eisen
        * meer druk op Information System personeel
        * vraag naar meer gebruiksvriendelijke, krachtige, gesofistikeerde tools
        * betere training voor eindgebruikers
* **Data homogenisatie**
	* men poogt gelijkenissen tussen data te accentueren en dit kan het nut van de data verlagen
	* bv. gelijkenissen tussen verkoop en verhuur van eigendommen
* **Nood aan meerdere (historische) versies naast mekaar**
	* Vb. als DWH wordt gebruikt voor financiële en operationele rapportering -> consistentie
	* Het beheer van diverse historische versies naast mekaar is een uitdaging
	* Grote DB volumes -> performantie
	* Archivering…
* **Hoge vraag naar resources**
	* bv. disk space
* **Data ownership**
	* data die voorheen gevoelig was, die voorbehouden was voor bepaalde afdelingen kan nu ook
aangeboden worden aan andere gebruikers
	* Beheer rechten in DWH vraagt bijzondere aanpak
* **Hoog onderhoud**
	* elke verandering in de business processen of in de bronsystemen heeft invloed op het DWH
(zowel DWH structuur als ETL)
* **Lange duur**
	* ontwikkeling kan jaren duren
	* wordt soms opgelost via de ontwikkeling en integratie van data marts
* **DWH ontstaat uit verwachting om de gebruikers te ‘empoweren’:**
	* Zelf rapporten, analyses maken
	* Meer onafhankelijkheid van IT
	* Nood aan metadata dictionary die data in DWH beschrijft en toegankelijk maakt
	* Maar toch grote afhankelijkheid van enkele specialisten.
* **Complexiteit van de integratie**
	* hoe kunnen alle nodige DWH tools zinvol en efficiënt samenwerken
* **Complex change en versie management**
	* Consistentie in rapportering
	* Impact op oude versies
* **DWH kan fungeren als input voor een management decision system**
	* Vb. historische verkoopsforecast met ingave van nieuwe data
	* Combinatie van rapportering en online data
* **Nacht dikwijls te kort voor afhandelen DWH ETL**
	* Zeker bij maand- en jaarsluiting
	* Als er iets fout gaat in een ander systeem
	* ETL stoppen of laten doorlopen?
        * Onvolledig DWH versus performantie killer
### DWH Technologieën
* Microsoft:
	* Microsoft reporting server
	* DTS (Data Transformation Services)
* Cognos (nu IBM)
	* ETL
	* Rapporteringstools
* Business Objects (nu SAP)
	* Rapportering
	* ETL
* SAP Business Warehouse
	* Kubus
* Datastage ETL
* Cliqview rapportering

## Ontwerp
---

TE MOE IEMAND VERRAS ME PLS FINISH IT






