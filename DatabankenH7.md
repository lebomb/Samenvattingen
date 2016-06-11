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


### Locking 
> Locking is een methode gebruikt om **concurrente
toegang** tot data te beheren.

Wanneer een transactie toegang heeft tot de data kan er via een lock voor gezorgd worden dat **andere transacties toegang tot de data geweigerd** worden. 

**Locking = Meest gebruikte manier** om de consistentie te garanderen

##### Basisregels locking
* Een transactie kan een **data item lezen of schrijven** enkel en alleen als het een **lock heeft verworven** voor dat data element, en het bovendien deze lock nog niet heeft vrijgegeven.

* Als een transactie een lock op een data item verwerft, dan moet het later ook **lock terug vrijgeven**

##### Soorten Locks
* Shared lock (S-lock)
    * Transactie met shared lock op data item kan dat data item *lezen* maar niet wijzigen
    * Meerdere transacties kunnen gelijktijdig een shared lock op eenzelfde data item bezitten.
* Exclusive lock (X-lock) 
    * Transactie met exclusive lock op data item kan dat data item **lezen en wijzigen**
    * Op elk ogenblik kan hoogstens 1 transactie een exclusive lock op een data item hebben.

**_Opmerking_:** een data item kan verwijzen naar een veld van een record, maar ook naar een tabel,
of de volledige db - zie verder: granulariteit
    
##### Praktisch
* De transactie die toegang wil tot een data item vraagt een lock aan op dat
item
	* shared lock voor lezen, exclusive lock voor lezen/schrijven
	* wanneer er nog geen lock op dat item bestaat, wordt de aanvraag ingewilligd
	* Wanneer er reeds een lock op het item is, gaat het dbms
nagaan of de aangevraagde lock compatibel (zie matrix hieronder) is
        * aanvraag shared lock op item met shared lock: granted
        * aanvraag exclusive lock op item met lock: wait
            * de transactie moet wachten tot de lock op het data item wordt vrijgegeven
* De transactie geeft een lock vrij
	* expliciet, of
	* impliciet als de transactie eindigt (via abort of commit)

**Compatibiliteitsmatrix:**

|  | Aangevraagde S-Lock | Aangevraagde X-Lock |
| :---: | :---: | :---: |
| **Bestaande S-Lock** | Grant | Wait |
| **Bestaande X-Lock** | Wait | Grant |

### Deadlock
> Een deadlock is een ** impasse** die kan ontstaan wanneer twee of meerdere transacties elk **wachten** op het vrijgeven van locks die de andere transactie heeft

##### Oplossen Deadlock
* **Abort en restart** van 1 of meerdere transacties
* Voorbeeld:

![alt text](http://puu.sh/pp2F3/9bba9bfce6.png "Voorbeeld Deadlock")

##### Technieken om met deadlocks om te gaan:
* **Timeouts:** een transactie die een lock aanvraagt **wacht voor een bepaalde vooraf gedefinieerde tijd op die lock**, indien lock niet toegekend is tijdens dit interval: 
    * Assumptie dat er een deadlock is (hoewel dit niet noodzakelijk zo is...) 
    * **Abort en restart** van de transactie
* **Deadlock prevention:** gebruikmakend van **transaction time-stamps** ( speciale time-stamp voor deadlock detection). T wacht op locks die U vastheeft:
    * **Wait-die algoritme**: 
        * Als T ouder is dan U dan zal T wachten
        * Zoniet dan sterft T en is er een abort/restart met dezelfde timestamp.
    * **Wound-wait algoritme**
        * Als T ouder is dan U dan zal het U 'verwonden' (--> meestal betekent dit een abort/restart van U) 
        * Zoniet zal T wachten 
* **Deadlock detection and recovery:** 
    * **Deadlock detection**
        * gebruik makend van een **wait-for graph** met transactie afhankelijkheden
        * wanneer de wait-for graph een lus bevat is er een
    deadlock
        * op regelmatige tijdstippen wordt deze graph
    getest op lussen
    * **Recovery van deadlock detection**
    	* 1 of meerdere transacties worden ge-abort
    * Problemen
    	* **Victim selection**
        	* abort die transactie waarvoor de abort een **'minimale kost'** met zich meebrengt
        * Enkele parameters die kunnen gebruikt worden:
        	* tijd dat de transactie al aan het runnen was
        	* aantal data items dat reeds werd gewijzigd door de transactie
        	* aantal data items die nog moeten worden gewijzigd door de transactie --> deze parameter is echter niet altijd gekend
    * Hoe ver moet een transactie een rollback doen
        * dit is niet noodzakelijk de volledige transactie
    * Voorkomen van starvation
        * komt voor wanneer steeds dezelfde transactie als victim
    wordt geselecteerd
        – bijhouden van een teller

## Transacties in SQL SERVER
---
### DB Transacties

* **Impliciete** transacties: INSERT, UPDATE, DELETE en elke transact SQL-opdracht
* **Expliciete** transacties: zelf aangeven waar transactie begint, wanneer de transactie kan
afgesloten worden, of hoe je op je stappen terugkeert om fouten op te vangen
    ```SQL    
        BEGIN TRANSACTION
        COMMIT TRANSACTION
        ROLLBACK TRANSACTION
    ```
* Alle info ivm transacties worden weggeschreven in de **transactielog**
 
### Stored Procedures en Transacties
```SQL
CREATE PROCEDURE usp_Customer_Insert
    @customerid varchar(5),
    @companyname varchar(25)
    @orderid int OUTPUT
AS
    BEGIN TRANSACTION
        INSERT INTO customers(customerid, companyname)
        VALUES(@customerid, @companyname)
        if @@error <> 0 BEGIN
            ROLLBACK TRANSACTION
            RETURN -1
        END
        INSERT INTO orders(customerid)
        VALUES(@customerid)
        if @@error <> 0 BEGIN
            ROLLBACK TRANSACTION
            RETURN -1
        END
        COMMIT TRANSACTION
        SELECT @orderid=@@IDENTITY
```

### Isolation Levels
Bepalen het gedrag van concurrent users die data lezen of schrijven. 
* **Reader**: 
    * Statement dat data leest, m.b.v. een shared lock
    * Kun je **niet beinvloeden** voor wat betreft de locks die ze nemen en de duur van de locks
* **Writer**:  
    * Statement dat data schrijft, m.b.v. een exclusive lock.
    * Kun je **wel excpliciet beinvloeden** m.b.v. **isolation levels**, hierdoor hebben ze ook impliciete invloed op het gedrag van writers.

    **Isolation level** = setting op de sessie-niveau of query-niveau. 
    
##### 4 Isolation Levels in SQL Server

![alt text](http://puu.sh/pp5sf/051d29f1fa.png "Isolation Levels Slide")

1. **Read Uncommitted**
    *  laagste isolatieniveau
    * **reader vraagt geen shared lock**
    * reader nooit in conflict met writer (die
exclusieve lock heeft)
    * reader leest uncommitted data (= dirty read)
2. **Read Committed**
    * default isolaton level, zie demo
	* laagste niveau dat dirty reads verhindert
	* reader leest enkel committed data
	* **reader vraagt** hiervoor shared **lock**
	* **als bij deze vraag een writer een
exclusive lock heeft, moet reader wachten op
shared lock**
	* reader houdt shared lock tot data verkregen is,
niet tot einde van zijn transactie
        *  **nogmaals lezen van de data in zelfde transactie kan
ander resultaat opleveren**
= non-repeatable reads of inconsistent analysis
        * acceptabel voor veel toepassingen, maar niet altijd
3. **Repeatable read**
    * reader vraagt shared lock en **houdt deze tot
einde van de transactie**
    * andere transactie kan geen exclusive lock
verkrijgen tot einde van de transactie van de
reader
    * **repeatable read = consistent analysis**
    * vermijdt ook lost update (kan wel bij 1 & 2)
door bij begin transactie shared lock te
nemen (m.b.v. SELECT want writers kun je
niet beïnvloeden, readers wel)
4. **Serializable**
    * Repeatable read lockt enkel rijen gevonden
bij eerste SELECT
    * Zelfde SELECT in zelfde transactie kan
nieuwe rijen geven (toegevoegd door andere
transactie) = phantoms
    * **Serializable** vermijdt phantoms
    * Lockt alle keys die beantwoorden aan
WHERE-clause, ook toekomstige
 
### Isolation levels: Sessie Niveau
```SQL
    SET TRANSACTION ISOLATION LEVEL READ COMMITED
```
### Isolation levels: Query Niveau
* override isolation level met "table hint":
    ```SQL
    SELECT * FROM ORDERS WITH (READUNCOMMITTED);
    -- of
    SELECT * FROM ORDERS WITH (NOLOCK);
    ```
*  dit vermijdt dat langlopende ad-hoc query's op
een productie-systeem updates in andere
transacties laten wachten bij READ
COMMITTED en hoger

##### Voorbeeld xTreme: oplossing m.b.v transacties


.

```SQL
    alter procedure vb @productclassname nvarchar(50)
    as
    declare @id int
    SET TRANSACTION ISOLATION LEVEL SERIALIZABLE
    BEGIN TRANSACTION
    -- Zorg voor shared lock op volledige tabel.
    -- Samen met level "repeatable" zorgt dit ervoor
    -- er geen records kunnen bijkomen tijdens de transactie.
    -- Repeatable volstaat niet:
    -- houdt enkel lock op reeds gelezen data
    select @id=max(productclassid) from productclass
    set @id = @id + 1
    insert into productclass values(@id,@productclassname)
    COMMIT
    SET TRANSACTION ISOLATION LEVEL READ COMMITTED
    return @id
```

### Triggers en Transacties
* een trigger maakt deel uit van de transactie die de
triggerende opdracht bevat
* binnen de trigger kan de transactie geROLLBACKed
worden
```SQL
CREATE TRIGGER delSpeler ON Speler
FOR delete
AS
IF (SELECT COUNT(*)
    FROM deleted JOIN SpelerPloeg
    ON SpelerPloeg.snr = deleted.snr) > 0
BEGIN
    ROLLBACK TRANSACTION
    PRINT ‘Je mag geen speler verwijderen als hij behoort tot een ploeg.' 
```

## Recovery
---
> **Recovery** is het proces waarbij
een **DB wordt teruggebracht naar een correcte toestand**
wanneer er zich een failure voordoet

**Waar zit de data?:**
- Main Memory, - Magnetic Disk, - Tape, - Optical Disk

![alt text](http://puu.sh/ppcTx/0a28767312.png "Schema Data")

- **Stabiele opslag:** replicatie op verschillende plaatsen met onafhankelijke failure modes.

### Soorten Failures
* **system crash**
	* hardware of software errors
	* verlies van gegevens in main memory
* **media failure**
	* vb. disk head crash
	* verlies van gegevens in secondary storage
* **software error in application**
	* vb. logische fout die transactie doet falen
* **natuurlijke 'rampen'**
	* vb. brand, aardbeving
* **slordigheid**
	* vb. onopzettelijk wissen van gegevens door gebruiker of db administrator
* **sabotage**
	* vb. opzettelijk wissen of corrupteren van gegevens of infrastructuur (sw/hw)

### Transactions en recovery
* eenheid voor recovery is een transactie
* **recovery manager** staat in voor
    * atomiciteit (**A**CID)
    * duurzaamheid (ACI**D**)
* moeilijkheid
    * schrijven naar een db is **niet atomair**
        * transactie kan committen zonder dat alle effecten al
(permanent) in de db geregistreerd zijn

### High level vs low level operaties
- **Voorbeeld**

![alt text](http://puu.sh/ppdHr/93640a3f0f.png "High level vs low level schema")

### Undo en redo
* Enkel bij een 'flush' van de buffer is data
permanent
	* flushing: data van primary storage overhevelen
naar disk storage
* **Expliciete** flush
	* force-writing
	* dit gebeurt via een commando, bv. bij commit
* **Impliciete** flush
	* wanneer de buffers vol zijn
* Wat bij failure tussen schrijven naar buffer
en flushing?
    * transactie was **reeds ge-commit**
        * durability: **redo** de wijzigingen
        * aka rollforward
    * transactie was **nog niet ge-commit**
        * atomicity: **undo** de wijzigingen
            * partiële undo: undo van 1 transactie
            * globale undo: undo van alle actieve transacties
        * aka rollback
* **Voorbeeld:**
![alt text](http://puu.sh/ppesT/df8ddd38c4.png "Voorbeeld undo en redo")

### Buffer management
* Buffer management omvat het **beheer van
transfer van buffers** tussen main memory en
disk
* Praktisch:
	* inlezen tot buffer vol
	* **replacement strategie** voor force-write
* FIFO first-in-first-out
* LRU least recently used
* **_Merk op:_** een pagina aanwezig in een buffer wordt nooit gelezen van disk

### Recovery faciliteiten
Het DBMS biedt volgende diensten aan
* **Back-up** mechanisme
    * periodische back-ups van de db
* **Logging** mogelijkheden
    * op de hoogte blijven van de huidige toestand van
transacties en db wijzigingen
* **Checkpoint** mogelijkheden
    * om lopende wijzigingen in de db permanent te maken
* **Recovery manager**
    * om de db in een consistente toestand te brengen na een failure

### Back-up mechanisme
* Op regelmatige basis worden er
automatisch **reservekopieën** van de db en
de logfile aangemaakt
    * dit gebeurt zonder systeem te moeten stoppen
    * de kopieën worden bewaard op offline storage
* Mogelijke benaderingen
    * **complete** back-up
    * **incrementele** back-up
### Logging
* **Log** bevat mogelijks
	* **Transaction records**
        * transaction id
        * type van log record (transaction start, insert, update, delete, abort (rollback), commit)
        * id van het gewijzigde data item (enkel bij insert, update, en delete operaties)
        * before-image
        	* waarde data item vóór wijziging (enkel bij update en delete operaties)
        * after-image
        	* waarde data item na wijziging (enkel bij update en insert operaties)
        * log management info
        	* bv. pointers naar previous/next record van die transactie
	* **Checkpoint records**
* Log wordt ook gebruikt voor andere doeleinden
	* performance monitoring, auditing
	* hiervoor is nog **extra info** in de log opgeslagen
* Log is heel belangrijk
	* wordt in **twee- of drievoud** bijgehouden
	* ook offline
* Log moet snel toegankelijk zijn
	* minor failures moeten direct opgelost worden
	* bevindt zich dus liefst ook op **online storage**
        * indien de grootte dit toestaat
* Voorbeeld log: ![alt text](http://puu.sh/ppgmP/e3a0d207bc.png "Voorbeeld Logging")

### Checkpointing
> Een **checkpoint** is een **synchronisatiepunt** tussen de databank en de log, op dit punt worden alle buffers ge-flushed

Checkpoints worden voorzien op vooraf ingestelde intervallen (bv. Om de 15 minuten). 

Een checkpoint omvat:
1. Alle log records in main memory wegschrijven naar disk 
2. De gewijzigde delen van de buffers wegschrijven naar disk 
3. Een checkpoint in de log registreren. ( Dit record bevat id van alle transacties die actief zijn op het moment van checkpointing).

**Voorbeeld:** recovery met checkpointing 
![alt text](http://puu.sh/ppgWo/b4ffb00a5d.png "Voorbeeld checkpointing")

## Recovery Technieken
---
Soort recovery procedure die gevolgd wordt hangt af van de ernst van het probleem:
* Serieuze (fysische) problemen
    * back-up restoren
    * wijzigingen van committed transacties (sedert de backup) die verloren
gingen, herstellen adhv de log
* Problemen van inconsistentie
    * kan zonder back-up
    * undo/redo adhv de log's before en after images

### Deferred update
> Bij een **deferred recovery protocol** worden
wijzigingen van een transactie niet weggeschreven naar de DB zolang de transactie niet het commit-punt bereikt

1. **Start** van de transactie: registreer de transactie start record in log
2. Bij een **write** actie: registreer dit in een log-record (**enkel after-image**, geen before image nodig). **Registreer dit niet in de DB**
3. **Commit** van de transactie: registreer transactie commit record in log. Schrijf alle log records weg naar disk (**flush voor de volgende stap!**) bij eerstvolgende checkpoint. Commit **update de DB adhv de log records.**
4. **Abort** van de transactie: **negeer de log records, schrijf niets naar disk**

### Immediate update
> Bij een **immediate update recovery protocol** worden
wijzigingen van een transactie direct weggeschreven naar de db

1. **Start** van de transactie: registreer transaction start record in log
2. Bij een **write** actie: schrijf het **log-record naar disk** (het log-record bevat **before-image, en after-image**), bij flush van de buffers (bij eerstvolgende checkpoint) wordt de wijziging naar de db
geschreven
3. **Commit** van de transactie: schrijf een transaction commit log record naar disk
4. **Abort** van de transactie: gebruik **before images voor undo**

###### **Voorbeeld**
![alt text](http://puu.sh/ppioj/b2ccf0f897.png "Voorbeeld Recovery Technieken")


|| Deferred | Immediate |
| :---: | :---: | :---: |
| T1 | niets | UNDO via Before-Images |
| T2 | niets | niets |
| T3 | niets | niets |
| T4 | REDO adhv After-Images | REDO adhv After-Images |
| T5 | REDO adhv After-Images | REDO adhv After-Images |
| T6 | niets | UNDO via Before-Images |
