# Samenvatting Databanken II
## Indexen en performantie 
---
### Space Allocation Door SQL Server

* SQL Server gebruikt Random Access File
* Space allocation in *extents* en *pages*
    * Page = 8kB blok aaneensluitende space
    * Extent = 8 logisch opeenvolgende pages
        * Uniform extents: voor 1 db-object
        * mixed extents: kunnen gedeeld worden door 8db-objecten. (=tabellen, indexen)
* Nieuwe tabel of index: allocatie in mixed extent
* Uitbreiding > 8 pages: in uniform extent

### Table scan 
> **Heap:** ongeordene verzameling van data-pages zonder clustered index (zie verder) (= Standaard opslag van een tabel)

Toegang via Index Allocation Map (IAM)
Table Scan: als een query pages ophaalt --> Dit is altijd te vermijden!

**Performantie issues met Heap** 
* **Fragmentatie**: tabel staat verspreid over verschillende, niet-opeenvolgende pages
* **Forward pointers**: als een rij met variabele lengte (vb. varchar-velden) wordt geupdatet waardoor ze langer wordt, wordt een forward pointer ingevoegd naar een andere pagina enz... -> Table Scan wordt nog trager.

**--> Oplossing: Indexen**

### Indexen 

**Wat?:** 
een *geordende structuur* die op de records uit een tabel wordt gelegd en *snelle toegang biedt via boomstructuur* (B-tree)

**Waarom?:** kan *toegang tot data versnellen* en kan de *uniciteit van rijen* afdwingen 

**Waarom niet?:** Indexen nemen opslagruimte in beslag *(overhead)* en ze kunnen de performantie ook doen dalen (bv vertragen van select en updates)

##### SQL Optimizer
> Is een **module in elk DBMS** die elk SQL commando dat naar de DB gestuurd wordt, analyseert en herformuleert en beslist op basis van statistische gegevens welke indexen zullen gebruikt worden.

### Clustered Index: 
De fysische volgorde van de rijen in een tabel is deze van een clustered index. Elke tabel kan slechts 1 clustered index hebben, deze legt unieke waarden op en heeft primary key constraint. 

**Voordelen t.o.v. Table Scan:**
* Dubbel gelinkte lijst zorgt voor volgorde bij het lezen van sequentiële records. 
* Geen forward pointers

### Non-Clustered Index
Deze is de **default index** en werkt trager dan de clustered index. Meerdere per tabel mogelijk. Elke *leaf* bevat sleutel waarde en row locator. --> Naar positie in clustered index als die bestaat, anders naar heap. 

Als een query meer velden nodig heeft dan aanweziginde index, dan moeten deze opgehaald worden uit de datapages. 

Bij het lezen via non-clustered index:
* *Ofwel:* RID (Row ID) lookup = bookmark lookups naar de heap adhv RID.
* *Ofwel:* Key lookup = bookmark lookups naar een clustered index. 


### Covering index

Als een non-clustered idnex een query niet volledig afdekt/covert dan moet SQL Server voor elke rij een lookup doen om de data op te halen.

> **Een covering index is** een *non-clustered* index die alle kolommen bevat die nodig zijn voor een bepaalde query.

Met SQL Server kan je extra komommen laten opnemen in de index (waarop niet geïndexeerd wordt)

**Covering index via include:** 
```SQL
CREATE NONCLUSTERED INDEX
[IX_Covering_Person_LastName_FirstName_MiddleName] 
ON [Person].[Person]
(
    [LastName] ASC,
    [FirstName] ASC,
    [MiddleName] ASC
) INCLUDE (Title);
```

 ### 1 index met meerdere kolommen vs. meerdere indexen met 1 kolom?
 
```SQL
CREATE NONCLUSTERED INDEX IX_Person_LastName_FirstName ON
Person.Person (LastName, FirstName)
```
**Versus**
```SQL
CREATE NONCLUSTERED INDEX IX_Person_LastName ON
Person.Person (LastName)
-- en
CREATE NONCLUSTERED INDEX IX_Person_FirstName ON
Person.Person (FirstName)
```
**Regel in SQL Server:**
Bij query (bijv. where-clause) op enkel 2de en/of 3de
,
…veld van de index, wordt de index niet gebruikt.
Dus bij:
```SQL
SELECT LASTNAME,FIRSTNAME
FROM PERSON.PERSON
WHERE FIRSTNAME = 'Chris';
```
wordt de dubbele index niet gebruikt!
**Conclusie: kijk naar de meest gebruikte query's en
stem daar je indexen op af.**

### Creatie van indexen: algemene syntax
```SQL
CREATE [UNIQUE] [CLUSTERED | NONCLUSTERED] INDEX index_naam 
ON tabel (kolom [,...n])
-- UNIQUE: geeft aan dat alle waarden in geïndexeerde kolom uniek moeten zijn. 

-- voorbeeld
create index rijksregNr_Index on student(rijksregNr)
```


**Let op:**
1. Bij het definiëren van een index mag de tabel leeg of al gevuld zijn. 
2. Kolommen met een **UNIQUE** index moeten de not null constraint bevatten.

### Verwijderen van indexen
```SQL
DROP INDEX table_name.index [,...n]

--voorbeeld
drop index student.rijksregNr_Index
```

### Wanneer gebruiken?
* Welke kolommen indexeren we **wel**?
	* primaire en unique kolommen worden automatisch geïndexeerd
	* vreemde sleutels vaak gebruikt in joins
	* kolommen vaak gebruikt in zoek condities of in joins
	* kolommen vaak gebruikt in ORDER BY clause
* Welke kolommen indexeren we **niet**?
	* kolommen die zelden voorkomen in queries
	* kolommen met een klein aantal mogelijke waarden (vb. geslacht)
	* kolommen in kleine tabellen
	* kolommen van het type bit, text of image











