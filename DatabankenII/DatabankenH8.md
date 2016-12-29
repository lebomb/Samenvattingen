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
##### Thanks Sofie

Er zijn 2 ontwikkel methodologieën
* **Inmon**:
    creatie van een data model gebasseerd op alle gegevens van de organisatie &rarr; Enterprise Data Warehouse (EDW)
    * Van hieruit worden data-marts voor elk departement gedistilleerd
    * Gebruik van ERD en tabellen in normaalvorm voor de beschrijving van het EDW
* **Kimball**: identificatie van informatie behoeften en business processen van de organisatie &rarr; Data Warehouse Business Matrix
    * Selectie en ontwikkeling van een eerste data mart voor de behoeften van een groep gebruikers.
    * Via integratie van data marts komen we tot het EDW
    * Gebruik van sterschema en varianten
    
### Kimball's Business Dimensional Lifecycle
**Focus** op het voldoen aan de informatiebehoeften van de organisatie via het bouwen van enkele, geïntegreerde, makkelijk bruikbare en selle informatie structuur. Deze structuur wordt op een incrementele, iteratieve manier gebouwd.

**Doel**: opleveren van een volledige oplossing waarbij
* DWH
* ad-hoc query tools
* reporting applicaties
* geavanceerde analytische tools
* training en support voor de gebruikers 

zijn inbegrepen.

**3 tracks**
* technologie
* data track
* business intelligence applications

### Dimensionality modelling
> **Dimensionality modeling** is een techniek om een logisch ontwerp te maken. 

Men streeft er naar om de data te presenteren in een standaard, intuïtieve vorm, die toegankelijk is met een hoge performantie.

#### Dreamhome DWH: voorbeeld
Management wil analyse van verkoop van huizen 

**Voorbeeld queries**
* wat was het totaal aan inkomsten van verkoop van eigendommen in het derde kwartaal van 2008?
* welke zijn de drie meest populaire gebieden in elke stad voor het verhuren van eigendommen in 2008; en hoe is dit in vergelijking met de resultaten van de twee vorige jaren?
* wat zou het effect zijn van een verhoging van de juridische kosten met 3.5% en een verlaging van de belastingen met 1.5% op de verkoop van eigendommen die meer waard zijn dan 200000 Euro?
* welke soorten eigendommen worden verkocht voor prijzen die boven de gemiddelde prijs van eigendommen in de grootste steden liggen en hoe hangt dit samen met de demografische gegevens
* wat is het verband tussen de jaarlijkse inkomsten van elke kantoor en het aantal verkoopsmensen die in elk kantoor werken?

**ERD**
![alt text](http://puu.sh/ppz4F/a1cba72b39.PNG "Voorbeeld Dimensionaal model")

#### Sterschema
> Een **ster schema** is een dimensioneel model die een feitentabel heeft, die omgeven is door gedenormaliseerde dimensietabellen.

![alt text](http://puu.sh/ppz5x/94409d7e78.PNG "Voorbeeld sterschema")

* **fact table** met samengestelde primaire sleutel
* **dimension tables** met een niet samengestelde primaire sleutel

Elke PK van een dimension table komt overeen met een deel van de PK van de fact table. Elk deel van de PK van de fact table is een FK die naar één van de dimensies refereert.

***Opmerking***  
* de natuurlijke sleutels uit het operationeel systeem worden opgenomen maar niet als sleutel in het sterschema gebruikt
* surrogaatsleutels zijn simpele integer-sleutels
* ze zorgen voor onafhankelijkheid van data tussen OLTP en DWH


##### Feitentabel
bevat data over feiten
bv. feitelijke data over de verkoop van een eigendom.  
Feiten worden gegenereerd oor gebeurtenissen die zich hebben voorgedaan. Ze zullen hoogstwaarschijnlijk nooit veranderen, ongeacht de manier waarop men ze analyseert.
* grote bulk van data in DWH dus tabel kan extreem groot zijn
* feitelijke data wordt beschouwd als read-only data, die niet verandert in tijd
* bevatten één of meerdere numerieke waarden, feiten die voor elk record toepasbaar zijn  
Meestal zijn feiten additief, ze worden zelden maar voor 1 record geraadpleegd

 **Voorbeeld**

![alt text](http://puu.sh/ppz5T/d49dacfa72.PNG "Voorbeeld feitentabel")


##### Dimensietabellen
bevat de referentie-informatie; beschrijvende, op tekst gebaseerde, informatie  
bv. het eigendom, de koper, de verkoper, ...
* attributen worden gebruikt als constraints bij DWH queries  
  bv. queries die gaan over verkopen van eigendommen in 'Glasgow'
  
Sterschema's kunnen query performantie aanzienlijk verhogen door referentie-informatie te denormaliseren en bij te houden in één enkele dimensietabel  
zie bv. city, region, country

**Voorbeeld**

![alt text](http://puu.sh/ppz69/ced07c54bc.PNG "Voorbeeld dimensietabellen")

#### Sneeuwvlok schema
> Een  **sneeuwvlokschema** is een variant op het sterschema waarbij dimensies worden bijgehouden in genormaliseerde dimensietabellen

![alt text](http://puu.sh/ppz58/b6605c5742.PNG "Voorbeeld sneuuwvlokschema")

***Opmerking***  
ook andere dimensietabellen zullen nu refereren naar City en Region tabellen

Indien een combinatie van genormaliseerde en niet genormaliseerde dimensietabellen wordt gebruikt spreekt men van een **ster-vlok schema**

### Voordelen
de voorspelbare en standaardvormvan het dimensioneelmodel levert enkele voordelen op:
* efficiëntie
    * de consistenteDB-structuur laat toe dat tools op efficiëntere maniertoegang tot de data kunnen krijgen
* veranderendebehoeften
    * het model kan zich aanpassen aan veranderende behoeften daar elke dimensie equivalent is t.o.v. de feitentabel
    * goed model voor ad-hoc queries
* uitbreidbaarheid
    * toevoegen van nieuwe feiten(met de juiste granulariteit)
    * toevoegen van nieuwe dimensies
    * toevoegen van attributen aand imensies
    * dimensies naar een kleinere granulariteit overzetten vanaf een bepaald punt in de tijd
* mogelijkheid om standaard business situaties te modelleren
* voorspelbare query processing
    * de manier waarop de tabellen gebruikt worden in queries is voorspelbaar(de queries niet!)

### DM en ER modellen
* Entity Relationship Diagrammen
    * gebruikt om de DB voor OLTLP systemen te ontwerpen
    *  basis: de relaties tussen entiteiten modelleren, met als doel redundantie weg te werken
        * redendantie is nadelig voor OLTP systemen
    *  ad-hoc queries kunnen moeilijker behndeld worden
        * leiden tot enoerm veel joins van enorm veel tabellen
        
* Dimensionaal Modeleren
    * gebruikt om de DB van een DWH (of datamart) te ontwerpen
    * intuïtieve opslag met een hoge graad aan performantie bij raadpleging van de gegevens
    
Eén ERD wordt uitgesplitst over meerdere DM-en, deze DM-en hangen samen via gedeelde dimensies

### Dimensional Modeling Stage
**DOEL**  
* creatie van een DM voor een data mart
* dimensionaliseren van het relationeel model van een bestaande OLTP DB

Dit gebeurt in 2 fasen:
1. creatie van een high-level DM
2. toevoegen van detail aan het model via identificatie van attributen voor de dimensies

### FASE 1: creatie van een high-level DM
![alt text](http://puu.sh/ppAzL/798001cdc4.PNG)

##### STAP 1: selecteer een business process
Het business process refereert naar het thema voor een welbepaalde datamart  
Voor de erste datamart kiezen we liefst één die
* hoogstwaarschijnlijk op tijd zal kunnen opgeleverd worden,
* binnen het budget valt,
* een antwoord geeft op commercieel belangrijke business vragen  
Dikwijls is deze gerelateerd aan verkoop financiën.
##### STAP 2: bepaal de granulariteit
Dit bepaalt wat een feit in de feitentabel exact gaat voorstellen:
* elk individueel record overnemen uit operationele gegevens of
* groeperen

Leidraad:
* hoe kunnen we tegemoetkomen aan de business requirements?
* wat is mogelijk met de beschikbare data source?

Granulariteit bepaalt de dimensies (volgende stap) en de granulariteit van de dimensies.
##### STAP 3: kies de dimensies
Dit bepaalt de context binnen dewelke we de feiten in de feitentabel zullen kunnen bevragen.  
Elke dimensie die meer dan 1 DM, dus meer dan 1 datamart, voorkomt, noemen we een **conformed dimension**.  
Conformed dimensions zijn exact gelijk aan elkaar of de een is een subset van de andere.

##### STAP 4: identificeer feiten
De granulariteit van de feitentabel bepaalt welke feiten er in de datamart kunnen gebruikt worden.  
Feiten zijn nummeriek en additief.  
Onbruikbare feiten zijn:
* niet numerieke waarden
* niet additieve waarden
* feiten met een granulariteit die verschilt van de granulariteit van de andere feiten in de feitentabel

### FASE 2: identificieer alle attributen voor de gekozen dimensies
Tekstuele omschrijving: intuïtief, zelfverklarend

Bruikbaarheid van de datamart hangt grotendeels af van de scope en aard van de attributen die in de dimensietabellen zitten.

### Aandachtspunten
* de duur van de DB bepaalt hoe ver terug in de tijd de fettentabel gaat
* langzaam veranderende dimensies  
  wanneer een dimensie verandert moet je er mogelijks voor zorgen dat je de nieuwe waarden niet gebruikt bij analyses an oudere transacties.
    * type 1: het attribuut dat verandert wordt gewoon overschreven
    * type 2: wanneer een attribuut verandert wordt een nieuw record in de dimensietabel toegevoegd
    * type 3: zorg dat de oude en de nieuwe waarde voor het attribuut beschikbaar zijn in het record
    
Op het einde van deze life-cycle zullen we een datamert hebben die voldoet aan 1 van de business requirements. Deze datamart zal geïntegreerd worden met andere datamarts om te komen tot een enterprise wide DWH. 

Een DM waarbij meer dan 1 feitentabel 1 of meer dimensies deelt noemen we een **feitenconstellatie**.
### Voorbeeld Dimensional Modeling Stage
**DreamHome**
##### STAP 1: selecteer een business process
Wat zijn de business processen?
* verkoop van eigendommen
* verhuur van eigendommen
* tonen van eigendommen
* adverteren van eigendommen
* onderhoud van eigendommen

ERD van DreamHome met de belangrijkste entiteiten voor elk business process ingekleurd
![alt text](http://puu.sh/ppYcV/1bf0a2bdd5.PNG "ERD")

We kiezen een business process:  
**verkoop van eigendommen**

![alt text](http://puu.sh/ppYiF/031bea87e3.PNG "business processen")

##### STAP 2: bepaal de granulariteit
keuze: PropertySale - verkoop van elke eigendom  
Nu kunnen we dimensies kiezen:
* Branch, Staff, ClientBuyer, PropertyForSale, Promotion
* extra kern dimensie steeds aanwezig in een DM is tijd

##### STAP 3: kies de dimensies
![alt text](http://puu.sh/ppYoJ/57b1466499.PNG)

#### STAP 4: identificeer feiten
PropertySale: 
* offerPrice
* sellingprice
* saleCommission
* saleRevenue
