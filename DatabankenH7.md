# Databanken II
## Hoofdstuk 7: Transactiebeheer
### Inleiding
---
Een **DBMS** ondersteunt:
* Transaction Support
* Concurrency control services
* Recovery Services

En is bedoeld om de DB in een **betrouwbare** en **consistente** toestande te houden bij zowel software als hardwarde failures, en bij gelijktijdig gebruik door meerdere gebruikers.

### Transacties
---
> Een **transactie** is een actie, of opeenvolging van acties op een db die **1 logsich geheel** vormen.

**Actie:** lezen of wijzigen van de inhoud van de DB.

Een transactie wordt uitgevoerd door een gebruiker of door een programma.

Het is een logische hoeveelheid van werk: volledig programma, deel van een programma, enkel opdracht. (Programma = 1 of meerdere transacties, met daartussen niet-db verwerking.

##### Voorbeelden: 
**Transactie:** updaten van het salaris van een werknemer.
![alt text](http://puu.sh/poRut/2310b75d44.png "Voorbeeld read/write")

Opeenvolging van 'high level' acties: 
* Notatie: read(...) en write(...)
    * read: lees een data item uit de DB in een lokale variabele
    * write: schrijf de waarde van een lokale variabele weg naar de DB.
* Er zijn 2 DB operaties en niet 1. 
---

**Transactie:** verwijderen van een werknemer en zijn properties aan een andere werknemer toekennen.

![alt text](http://puu.sh/poREW/a34962c08e.png "Voorbeeld verwijderen")

Deze transactie brengt de DB van de ene consistente toestand naar de andere consistente toestand. **! Tijdens** de transactie is de DB mogelijks in een inconsistente toestand.  (bv. een aantal properties staan reeds bij newStaffNo, de andere nof bij staffNo)

### Resultaat van een Transactie
##### Commited / Aborted
**Commited:**  
* de transactie kent een **succesvolle afloop**.  
* de transactie *commits* en de DB heeft een consistente toestand bereikt.
* commited transactie **kan nooit aborted worden**, wanneer de commited transactie vergissing blijkt, kan een andere compenserende transactie gebruikt worden.

**Aborted**
* de transactie is **niet succesvol**
* de transactie *aborts* en DB moet teruggebracht worden naar de consistente toestand waarin ze zich bevond voor de transactie werd gestart --> **rollback of undo**
* aborted transactie **kan herstart** worden: **na de rollback** kan een **restart** van de transactie gebeuren. Bij succesvolle uitvoering zal dit nu leiden tot een committed transactie.
* 
### Aanduidingen
We moeten duidelijk maken aan het DMBS welke acties deel uitmaken van een transactie.

Keywords: 
```SQL
- BEGIN TRANSACTION
-- soms impliciet de eerste DB actie

– COMMIT
– ROLLBACK
```
Zonder genige expliciete aanduiding kan het ganse programma beschouwd worden als 1 transactie. 

### ACID Eigenschappen
**Atomicity:** 
* de opdrahten van een transactie worden als één ondeelbaar geheel beschouwd. --> Alles of niets
*  DBMS verantwoordelijkheid: recovery subsysteem

**Consistency:** 
* een transactie brengt de DB van de ene consistente toestand naar een andere consistente toestand. 
* DBMS verantwoordelijheid én programma verantwoordelijkheid

**Isolation:**
* transacties worden onafhankelijk van elkaar uitgevoerd
* transacties mogen niet ongewenst interfereren met elkaar.
* transacties mogen geen uitkomsten aan andere transacties presenteren voor de commit.
* DBMS verantwoordelijkheid: concurrency-control subsysteem.

**Durability:** 
* Na een COMMIT zijn de aangebrachte wijzigingen gegarandeerd permanent.
* Na een storing kunnen deze wijzigingen steeds worden gerecupureerd.
* DBMS verantwoordelijkheid: recovery subsystem.

### Databank Architectuur
![alt text](http://puu.sh/poTCC/55ec584625.jpg "Architectuur schema")
##### Transaction Subsysteem
1. **Transaction manager**
    * Coördineert transacties van programma's
2. **Scheduler**
    *  implementeert strategie voor concurrency control
    * aka lock manager bij een op locking gebaseerde strategie
    * doel: concurrency maximal
3. **Recovery manager**
    * bij failures brengt deze de db terug in consistente toestand (~vóór de transactie gestart werd)
4. **Buffer manager**
    *  verantwoordelijk voor efficiënte transfer van data tussen main memory en
disk storage
## Concurrency
---
### Concurrency control
> **Concurrency control**
is het **beheer van gelijktijdige** acties op de db zonder
dat ze interfereren met elkaar

Een DBMS laat toe dat **merdere transacties gelijktijdig toegang** hebben tot **gedeelde data**.
* Wanneer verschillende transacties **enkel data lezen** rijzen er **geen problemen**.
* Wanneer minstens 1 transactie **data wijzigt** kunnen **wel problemen** optreden.

**Concurrency** wordt gerealiseerd door de acties van verschillende transacties te verweven.

### Waarom concurrency control?
--> **Voorkom interferentie** wanneer twee of meerere transacties de databanke gelijktijdig aanspreken, en minstens 1 transactie data **wijzigt**

Hoewel twee transacties op zichzelf beschouwd correct kunnen zijn, kan het verweven van hun acties leiden tot een incorrect resultaat.

## Problemen bij concurrency
--- 
### Lost update
> Het **lost update probleem** treedt op wanneer
een schijnbaar compleet succesvolle update van een transactie
wordt **overschreven** door een andere transactie

### Uncommitted dependency
> Het **uncommitted dependency (of dirty read) probleem**
treedt op wanneer
een transactie de **intermediaire resultaten van een andere**,
uncommitted transactie, kan zien.

### Inconsistent Analysis
> Het **inconsistent analysis probleem** treedt op wanneer
een transactie **verschillende waarden uit de db leest**, terwijl een
andere transactie bezig is sommige van **deze waarden te
veranderen**

##### Gerelateerde problemen
* **Non-repeatable (of fuzzy) read**
	* twee keer lezen van eenzelfde item levert twee
verschillende waarden op
        * een transactie leest een data item, die het reeds eerder
uitlas, nogmaals uit en krijgt nu een andere waarde
* **Phantom read**
	* een transactie leest een aantal records die voldoen
aan een bepaalde voorwaarde
	* de transactie herhaalt dit later maar merkt nu dat er
ondertussen andere records zijn bijgekomen via een
andere transactie

### Serializibility & Recoverability
* **concurrency control protocol**
    * doel: transacties zo schedulen zodat ze niet interfereren
    * resultaat: voorgaande problemen treden niet op
* triviale oplossing
    * voer transacties serieel uit
    	* ten allen tijde is er maar 1 transactie in uitvoering
    	* groot nadeel: performantie (geen concurrency of parallellisme)
* **serializability**
    * bestaat er in die uitvoeringen van transacties te identificeren
    waarbij geen interferentie optreedt
    	* uitvoeringen die equivalent zijn aan een seriële uitvoering

### Schedule
> Een schedule is
**een sequentie van acties van een aantal concurrente
transacties** die de volgorde van de acties van de individuele
transacties respecteert

**Note:** Voor elke transactie in een schedule is de volgorde van de operaties in
de schedule, dezelfde als de volgorde van de operaties in de
transactie zelf

### Recoverability
* de databank zelf **schedules** laten opstellen die zonder
interferentieproblemen concurrent kunnen worden uitgevoerd
	* ze garanderen de **consistency en isolation** eigenschap van transacties
	* dit is in de veronderstelling dat geen enkele van de transacties in de
schedule faalt
* **recoverability**
	* wanneer een transactie faalt moeten we de effecten ongedaan kunnen maken
        * **atomiciteit** van transacties
	* wanneer een transactie commit zijn de veranderingen permanent
        * **durability** eigenschap van transacties





