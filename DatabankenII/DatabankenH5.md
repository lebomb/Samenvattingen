# Samenvatting Databanken

# Hoofdstuk 5

## Stored Procedures
---


```SQL
CREATE procedure usp_Customers_Delete
@custno nchar(5) = NULL
AS
IF @custno IS NULL BEGIN
RAISERROR('customerID is NULL', 10, 1)
RETURN
END
IF NOT EXISTS (SELECT * FROM customers WHERE customerid = @custno)
BEGIN
RAISERROR ('Klant bestaat niet', 10, 1)
RETURN
END
IF EXISTS (SELECT * FROM orders WHERE customerid = @custno) BEGIN
RAISERROR ('Klant heeft orders', 10, 1)
RETURN
END
DELETE FROM customers WHERE customerid = @custno
```

### Gebruik in MS SQL Server

**-exec *procedure* _value_** 

## Functies
---

1. Standaard SQL Functies
2. Niet standaard built-in functions: 
..* SQL Server: datediff, substring, len, round,... ( [library](http://technet.microsoft.com/en-us/library/ms174318.aspx) ) 
3. User defined functies
### User defined functies
Manier om naast views en CTE's SELECTS te **hergebruiken**, nu zelfs met **parameters**
Voorbeeld:
```SQL
CREATE FUNCTION GetAge
(
@birthdate AS DATE,
@eventdate AS DATE
)
RETURNS INT
AS
BEGIN
RETURN
DATEDIFF(year, @birthdate, @eventdate)
- CASE WHEN 100 * MONTH(@eventdate) + DAY(@eventdate)
< 100 * MONTH(@birthdate) + DAY(@birthdate)
THEN 1 ELSE 0
END;
END;
```
Gebruik: 
```SQL
select lastname,firstname,birthdate,hiredate,
dbo.GetAge(birthdate,getdate()) as leeftijd,
dbo.GetAge(hiredate,getdate()) as dienstjaren,
dbo.GetAge(birthdate,hiredate) as
LeeftijdIndienttreding
from Employees
```

### Inline Table Valued Function 
Geef per product klasse het goedkoopste product dat meer kost dan X€ en een product die die prijs heeft.

```SQL
create function minimum (@grens int) returns table
as
return
select productclassid klasse,min(price) prijs
from product p where price >= @grens
group by productclassid;
-- gebruik:
select klasse,prijs,
(select top 1 productid from product where
productclassid=klasse and price=prijs)
from minimum(0);
```

### Voordelen PSM
1. Code modularisatie. 
    * Reduceren redundante code
    * Minder onderhoud bij schema wijzigingen
    * Vaak voor CRUD operaties
2. Customisatie van "gesloten" systemen zoals ERP: via stored procedures en triggers kan men "ingrijpen".
3. Security: 
    * rechtstreekse query's op tabellen uitsluiten 
    * via SP's vastleggen wat kan en wat niet
    * vermijd SQL-injection door gebruik input-parameters.
4. Centrale administratie van (delen van) DB-code.

### Nadelen PSM
1. Beperkte schaalbaarheid: business logica en db verwerking op zelfde server kan tot bottle-necks leiden.
2. Vendor lock-in
    * Syntax = Geen standaard
    * Portabiliteit heeft zijn prijs: vb built-in functions niet gebruiken
3. Twee programmeertalen:
    1. Java/.NET/ ...
    2. SD / UDF
4. Twee Debuggingomgevingen
5. SD/UDF: Beperkte OO-ondersteuning.

### Vuistregels: 
1. Vermijd PSM voor grotere business logica
2. Gebruik PSM vooral voor technische zaken:
    * Validatie / Logging / Auditing
3. Maak keuze portabiliteit / Vendor Lock-in in overleg met business en corporate IT policies

## Stored Procedure
---
> **Een stored procedure** is een benoemde verzameling SQL en control-of-flow opdrachten (programma) die opgeslagen wordt als een database object.

Analoog aan procedures uit andere talen: 
* Kan worden aangeroepen vanuit: programme, trigger of stored procedure
* Wordt opgeslagen in de catalagus.
* Accepteer in- en uitvoer parameters
* Retourneert statusinformatie over de al dan niet correct uitvoering van de stored procedure.
* Bevat taken die vaak worden uitgevoerd.

## Variabelen
---

### Lokale variabelen
**Declareren:**
de naam wordt steeds voorafgegaan door **@**
```SQL
DECLARE @variable_name1 data_type [, @variable_name2data_type ...]
``` 

**Toekennen van waarde aan variabele:**
```SQL
SET @variable_name = expression
SELECT @variable_name = column_specification
```
Set en select zijn gelijkaardig, maar **SET** is **ANSI** standaard.

Met **SELECT** kan je wel **meerdere variabelen in 1 keer** een waarde geven. (2e SELECT hieronder)
```SQL
SET @max = (select max(invoiceTotal) from invoices)
SELECT @max = max(invoiceTotal) from invoices

SELECT @max = max(invoiceTotal), @nrOfInvoices = count(*) from invoices

PRINT string_expression
```
Als alternatief voor print kan je ook **select** gebruiken: 
*SELECT string + variabele*

```SQL
DECLARE @lname varchar(40), @rijtelling int
SET @lname = ‘Ringer’
SELECT @rijtelling = count(*)
FROM Authors
WHERE au_lname=@lname
PRINT ‘Aantal werknemers met naam ‘ + @lname + ‘ = ‘ +
str(@rijtelling)
```

## Transact SQL
---
### Operatoren in Transact SQL
* Rekenkundige operatoren
    * -- , +, *, /, % (Modulo)
* Vergelijkings operatoren
    * --, <, >, =, ..., IS NULL, LIKE, BETWEEN, IN
* Alfanumerieke operatoren
    * +(String Concatenatie)    
* Logische operatoren
    * AND, OR, NOT   
* Functies
    * Numerieke: ROUND, POWER, COS, ...
    * Datum/Tijd: DATEADD, DATEDIFF, GETDATE, DAY, MONTH...
    * Alfanumerieke: LEFT, RIGHT, LTRIM, RTRIM, TRIM, REPLACE, UPPER, LOWER, ...
    *  Systeemfuncties: CAST, CONVERT, ISNUMERIC, ISDATE, PRINT, ...

## Control flow in Transact SQL
Programma verloop kan je bepalen via: 

**Instructie niveau:**
```SQL
    BEGIN ... END
    IF ... ELSE
    WHILE... (BREAK / CONTINUE)
    RETURN
```
**Rij-niveau:** 
```SQL
    CASE ... END
```
### Commentaar
**Inline commentaar:**

        -- commentaar
    
**Block commentaar:**

        /* commentaar */
        
        /*
        ** commentaar
        */ 
        
### Foutafhandeling met Transact SQL
* **RETURN** : Onmiddelijke beëindiging van de batch of procedure.
* **@@error** : Bevat de fout van de laatste utigevoerde instructie, indien ok value = 0.
* **RAISERROR** : Retourneren van een user defined fout of systeemfout

## CURSORS
---
> SQL statements werken standaard met een complete resultaatset en niet
met individuele rijen. Cursors laten toe om met individuele rijen te werken.

**Een cursor is** een database object die wijst naar een resultaatset. Via de
cursor geef je aan met welke rij uit de resultaatset je wenst te werken

**5 belangrijke statements:**
1.   **DECLARE CURSOR** : Creëert en definieert cursor
2.   **OPEN** : Opent de gedeclareerde cursor
3.   **FETCH** : Haalt 1 rij op
4.  **CLOSE** : Sluit de cursor
5.  **DEALLOCATE** : Verwijdert de cursor definitie

```SQL
1.  DECLARE <cursor_name> [INSENSITIVE][SCROLL] CURSOR FOR
    <SELECT_statement>
    [FOR {READ ONLY | UPDATE[OF <column list>]}]

2.  OPEN <cursor name>

3.  FETCH [NEXT | PRIOR | FIRST | LAST | {ABSOLUTE | RELATIVE
    <row number>}]
    FROM <cursor name>  
    [INTO <variable name>[,...<last variable name>]] 
    
4.  CLOSE <cursor name>

5.  DEALLOCATE <cursor name>
```

### Overzicht voorbeeld CURSORS:
```SQL
    DECLARE @au_lname varchar(40), @au_fname varchar(20)
    DECLARE authors_cursor CURSOR FOR
    SELECT au_lname, au_fname FROM authors
    WHERE au_lname LIKE ‘B%’
    ORDER BY au_lname, au_fname
    
    OPEN authors_cursor
    
    FETCH NEXT FROM authors_cursor
    INTO @au_lname, @au_fname
    WHILE @@FETCH_STATUS = 0
        BEGIN
            PRINT ‘Author: ‘ + @au_fname + ‘ ‘ + @au_lname
            FETCH NEXT FROM authors_cursor
                INTO @au_lname, @au_fname
        END
    CLOSE authors_cursor
    DEALLOCATE authors_cursor
```
### Geavanceerd voorbeeld CURSORS:
```SQL
    DECLARE @au_lname varchar(40)
    DECLARE @au_fname varchar(20), @au_id id
    DECLARE @message varchar(50)
    DECLARE @title varchar(80)
    
    DECLARE authors_cursor CURSOR FOR
    SELECT au_id, au_fname, au_lname
    FROM authors
    WHERE state = 'UT'
    ORDER BY au_id
    
    OPEN authors_cursor
    
    FETCH NEXT FROM authors_cursor INTO @au_id, @au_fname, @au_lname
        WHILE @@FETCH_STATUS = 0
            BEGIN
                PRINT ' '
                SELECT @message = '----- Books by Author:
                    ' + @au_fname + ' ' + @au_lname
                PRINT @message
                FETCH NEXT FROM authors_cursor INTO @au_id, @au_fname, @au_lname
            END
    CLOSE authors_cursor
    DEALLOCATE authors_cursor
```

```SQL
DECLARE @au_lname varchar(40)
DECLARE @au_fname varchar(20), @au_id id
DECLARE @message varchar(50)
DECLARE @title varchar(80)
DECLARE authors_cursor CURSOR FOR
SELECT au_id, au_fname, au_lname
FROM authors
WHERE state = 'UT'
ORDER BY au_id
OPEN authors_cursor
FETCH NEXT FROM authors_cursor
INTO @au_id, @au_fname, @au_lname
    WHILE @@FETCH_STATUS = 0 BEGIN
        PRINT ' '
        SELECT @message = '----- Books by Author:' 
                + @au_fname + ' ' + @au_lname
        PRINT @message
    -- Declare an inner cursor based
    -- on au_id from the outer cursor.
    DECLARE titles_cursor CURSOR FOR
    SELECT t.title
    FROM titleauthor ta join titles t
    ON ta.title_id = t.title_id AND
    ta.au_id = @au_id -- variable value
    from the outer cursor
    
    OPEN titles_cursor
    
    FETCH NEXT FROM titles_cursor INTO @title
    IF @@FETCH_STATUS <> 0
    PRINT ' <<No Books>>'
    WHILE @@FETCH_STATUS = 0 BEGIN
        SELECT @message = ' ' + @title
        PRINT @message
        FETCH NEXT FROM titles_cursor INTO
        @title
    END
    
    CLOSE titles_cursor
    DEALLOCATE titles_cursor
    -- Get the next author.
    
    FETCH NEXT FROM authors_cursor
    INTO @au_id, @au_fname, @au_lname
END -- outer while loop
CLOSE authors_cursor
DEALLOCATE authors_cursor
```
### Update en delete via CURSORS
```SQL
DELETE FROM <table name>
WHERE CURRENT OF <cursor name>

UPDATE <table name>
SET ...
WHERE CURRENT OF <cursor name> 
```
### Creatie van SP (Stored Procedure)
```SQL
CREATE PROCEDURE <proc_name> [parameter declaratie]
AS
<sql_statements> 
```

### Wijzigen, verwijderen en uitvoeren van SP
```SQL
-- WIJZIGEN
ALTER PROCEDURE <proc_name> [parameter declaratie]
AS
<sql_statements> 

-- VERWIJDEREN
DROP PROCEDURE <proc_name>

-- UITVOEREN
EXECUTE <proc_name> [parameters] 

/*
** bij eerste uitvoering: compilatie en optimalisatie
** hercompilatie forceren: wenselijk bij wijzigingen aan structuur databank
*/
    execute uspOrdersSelectAll with recompile
    execute sp_recompile uspOrdersSelectAll
```

### Return waarde van een SP
Bij uitvoering keer een SP een **return waarde** terug, deze waarde is een int met default waarde 0

**Return Statement:** uitvoering van SP wordt gestopt, laat toe om de return waarde te bepalen.

### Voorbeeld gebruik van return waarde:
```SQL
CREATE PROCEDURE usp_OrdersSelectAll AS
select * from orders
return @@ROWCOUNT
-- creatie van een SP met expliciete return-waarde
DECLARE @returnCode int
EXEC @returnCode = usp_OrdersSelectAll
PRINT ‘Er zijn ‘ + str(@returnCode) + ‘ records.’
-- gebruik SP met return-waarde
```

### SP met parameters
Aanroepen van de SP:
*   Voorzie steeds keyword **OUTPUT** voor outputs parameters.
*   2 manieten om de actuele parameters door te geven:
    1. Gebruik formele parmater naam (volgorde onbelangrijk)
    2. Positioneel

```SQL
    DECLARE @aantal int
    EXECUTE usp_OrdersSelectAllForCustomer
    @customerID = ‘ALFKI’,
    @count = @aantal OUTPUT
    PRINT @aantal
    -- param doorgeven via expliciet gebruik formele param-namen
    DECLARE @aantal int
    EXEC usp_OrdersSelectAllForCustomer 'ALFKI', @aantal OUTPUT
    PRINT @aantal
    -- parameters positioneel doorgeven
```

## Error Handling
---
**@@error** is en systeemfunctie die het foutnummer bevat van de laatst uitgevoerde opdracht. De waarde 0 wijst op succesvolle uitvoering.
```SQL
CREATE PROCEDURE usp_ProductsInsert
    @productName nvarchar(40),
    @categoryID int,
    @unitprice money
AS
INSERT INTO products(productname, categoryID, unitprice)
VALUES (@productname, @categoryID, @unitprice)
IF @@error = 515
    PRINT 'ERROR! productname is NULL.'
ELSE IF @@error = 547
        PRINT 'ERROR! CategoryID is not in CATEGORY table.'
     ELSE PRINT 'ERROR! Unable to add new product.'
RETURN @@error
-- gebruik van @@error om specifieke foutboodschappen te genereren en als returnwaarde
```

Alle foutboodschappen zitten in de systeemtabel **sysmessages**:
```SQL
    SELECT * FROM master.dbo.sysmessages
    WHERE error = @@ERROR 
```
Eigen foutboodschappen genereren kan via **raiserror**.
```SQL
    raiserror(msg, severity, state)
    -- Message = Foutboodschap
    -- Severity = Waarde tussen 0 en 18
    -- State = waarde tussen 1 en 127
```

Voorbeeld andere systeemfunctie **@@rowcount**
*   Aantal aangepaste/geselecteerde rijen door de laatst uitgevoerde instructie.

### Exception handling:
```SQL
CREATE PROCEDURE dbo.TestError3 (@e int OUTPUT)
AS
BEGIN
SET @e = 0;
BEGIN TRY
    INSERT INTO Person.Address (AddressID) VALUES (1);
END TRY
BEGIN CATCH
    SET @e = ERROR_NUMBER();
    PRINT N'Error Procedure = ' + ERROR_PROCEDURE();
    PRINT N'Error Message = ' + ERROR_MESSAGE();
END CATCH
END

GO
DECLARE @e int;
EXEC dbo.TestError3 @e OUTPUT;
PRINT N'Error code = ' + CAST(@e AS nvarchar(10));
```

**Catch block functies**
| Functie | |
|--| --- |
| ERROR _ LINE() | Lijn nummer waar exception optrad |
| ERROR_MESSAGE() | Foutboodschap |
| ERROR_PROCEDURE()| SP waar fout optrad|
| ERROR_NUMBER() | Foutnummer |
| ERROR_SEVERITY() | Severity level v/d fout |

## Triggers
---
> **Een trigger**: een hoeveelheid code, bestaande uit
procedurele en declaratieve instructies, die opgeslagen
is in de catalogus en die geactiveerd wordt door het
DBMS indien een bepaalde operatie op de database wordt
uitgevoerd en indien een bepaalde conditie voldaan is

Gelijkaardig aan SP maar **kan niet expliciet worden opgeroepen**
* Een trigger wordt automatisch door het DBMS aangeroepen bij bepaalde
DML en DDL opdrachten

    *   **DML trigger**: bij een **insert, update of delete** voor een tabel of view waar de
trigger aan is gekoppeld (wij beperken ons verder in deze cursus tot dit soort triggers). 
    Kunnen geactiverd worden: **before\*, instead of, after **( = na IUD verwerkt en voor COMMIT) de IUD opdracht. *Niet ondersteund in SQL Server
    *    **DDL trigger**: bijvoorbeeld bij een **create, alter of drop** van een tabel waar een
trigger aan gekoppeld is

## Procedurele database objecten: 
---
### Procedurele programma's
| soort |  opgeslaan als | uitvoering | ondersteunt parameters |
| --- | --- | --- | --- |
|script | apart bestand |client tool (bv. Management Studio) |nee|
|stored procedure|database object |via applicatie of SQLscript| ja |
|user defined function |database object |via applicatie of SQL script | ja|
|trigger |database object |via DML statement |nee|

### Waarvoor Triggers gebruiken?
1. validatie van data en complexe constraints
    * een werknemer mag aan niet meer dan 10 projecten verbonden zijn
    * een werknemer kan enkel verbonden zijn aan een project dat toegewezen is aan zijn
departement
2. automatische generatie van waarden
    * wanneer een werknemer wordt toegewezen aan een project wordt de default waarde voor de
maandelijkse bonus die hieraan verbonden is automatisch ingevuld adhv de project prioriteit
en zijn jobcategorie
3. ondersteuning voor alerts
    * automatisch een e-mail sturen wanneer een werknemer van een project gehaald wordt
4. bijhouden van audits
    * automatisch bijhouden wie wat doet en met welke tabel
5.  replicatie en gecontroleerd bijhouden van redundante data
    * bijhouden van aantal projecten toegekend aan een werknemer in de werknemer tabel
    * automatisch opvullen van datawarehouse-tabellen voor rapportering (zie hoofdstuk
"Datawarehousing")

### Voordelen en nadelen
**Grote voordeel:** mogelijkheid om **functionaliteit in de DB** op te slaan en **consistent uit te voeren** bij elke wijziging aan de DB.

**Dus:** 
*   Geen redundante code, *functionaliteit zit op 1 plaats in de DB*
*   Wijzigingen aanbrengen wordt eenvoudig, *written & tested once door ervaren DBA*
*   Veiligheid, *triggers zitten in de DB dus kunnen alle beveilingsregels volgen*
*   Meer processing power, *voor DBMS en DB*
*   Past in client-server model, *1 aanroep naar db-server waar veel kan gebeuren zonder dat verdere communicatie vereist is*

**Nadelen** 
* Complexiteit, *DB ontwerp, implementatie en onderhoud is complexer door verschuiven van functionaliteit van applicatie naar DB +  Moeilijk te debuggen.*
* Verborgen functionaliteit, *gebruiker kan confronted worden met onverwachte neveneffecten trigger. Triggers kunnen ook cascaderen; bij het ontwerp van de trigger is dit niet altijd duidelijk te voorspellen.*
* Performantie, *bij elke wijziging aan DB moet trigger conditie geëvalueerd worden*
* Portabiliteit, *je pint je vast op het dialect van DBMS*

### Vergelijking trigger functionaliteit
|   | Oracle  | MS SQL Server  | MySQL | 
| --- | :---:| :---: | :---: | 
| BEFORE *bij validatie* | X | simuleren via AFTER-Trigger+ROLLBACK | X |
| AFTER | X | X | X |
| INSTEAD OF *bij views*  | X | X | X |
| FOR EACH STATEMENT | X | DEFAULT | DEFAULT |
| FOR EACH ROW  | **X:** *toegang tot waarden voor/na via :NEW/:OLD* vars| **N/A:** *toegang to waarden voor/na per rij via deleted/inserted pseudo-tabellen* | **X:** *toegang tot waarden voor/na via NEW/OLD vars*|
| TRANSACTIES | COMMIT/ROLLBACK niet toegestaan | COMMIT/ROLLBACK toegestaan | COMMIT/ROLLBACK niet toegestaan |
### "Virtuele" tabellen bij Triggers
2 tijdelijke tabellen
* **Deleted Tabel** : bevat kopies van de gewijzigde (update) verwijderde (delete) rijen.
    * Tijdens de update of delete worden rijen van de **trigger tabel** gekopieerd naar de **deleted tabel**
    * Deze twee tabellen hebben geen gemeenschappelijke rijen
* **Inserted Tabel** : bevat kopies van gewijzigde (update) of ingevoegde (insert) rijen
    * tijdens een update of insert wordt een kopie van elke rij die gewijzigd of toegevoegd wordt in de **trigger tabel** geplaatst in de **inserted tabel**
    * deze twee tabellen hebben dus enkel gemeenschappelijke rijen
    
### Creatie van een after trigger
Enkel mogelijk door SysAdmin of dbo en is **gebonden aan 1 tabel**; niet aan een view.

Wordt uitgevoerd na:
1. na uitvoering van de triggering actie, i.e. *insert, update, delete*
2. Na logging van de update in de tijdelijke trigger tabellen inserted en deleted
3. Voor de commit

```SQL
    CREATE TRIGGER naam
    ON tabel
    FOR [INSERT, UPDATE, DELETE]
    AS ...
    vereenvoudigde syntax voor
    een after trigger
```

## Voorbeelden Triggers
---
### Insert after-trigger
Triggering instructie is een **insert** instructie
* **Inserted:**  logische tabel waarvan kolomnamen gelijk zijn an die van triggering tabel en die een copy bevat van de rij(en) die werden toegevoegd.
* **Merk op:** bij triggering door een INSERT-SELECT statement kunnen bij één INSERT meerdere records toegevoegd worden. De trigger zal dan slechts 1x uitgevoerd worden, maar voor elke record een mutatie record aanmaken.
```SQL
    CREATE TRIGGER insert_speler ON SPELERS FOR insert
    AS
        INSERT INTO mutatie (gebruiker, mut_tijdstip, mut_snr, mut_type, mut_snr_new)
        SELECT user, getdate(), nul, 'i', spelersnr FROM inserted
    -- Automatisch bijwerken van de mutatie tabel bij toevoegen van speler
```
### Delete after-trigger
Triggering instructie is een **delete** instructie: 
* **Deleted** : logische tabel waarvan kolomnamen gelijk zijn aan die van de triggering tabel en die een copy bevat van de rij(en) die werden verwijderd.
* We maken gebruik van onderstaande stored procedure, die we zullen hergebruiken bij de update-trigger.
```SQL
    CREATE PROCEDURE usp_mutatie_insert
        (@MSNR SMALLINT,
        @MTYPE CHAR(1),
        @MSNR_NEW SMALLINT)
    AS
        INSERT INTO mutatie (gebruiker, mut_tijdstip, mut_snr,mut_type, mut_snr_new)
        VALUES (user, getdate() ,@MSNR, @MTYPE, @MSNR_NEW);
        
-- automatisch bijwerken van de mutatie tabel bij verwijderen van één of meerdere spelers
    CREATE TRIGGER delete_speler
        ON spelers FOR delete
    AS
        DECLARE @old_snr smallint
        DECLARE del_cursor CURSOR FOR SELECT spelersnr FROM deleted
        OPEN del_cursor
        FETCH NEXT FROM del_cursor INTO @old_snr
        WHILE @@FETCH_STATUS = 0
        BEGIN
            EXEC usp_mutatie_insert @old_snr,'D',null
            FETCH NEXT FROM del_cursor INTO @old_snr
        END
        CLOSE del_cursor
        DEALLOCATE del_cursor
    -- Activatie van de trigger:
        delete from spelers where spelersnr > 115;
```
### Update after-trigger
Triggering instructie is een **update** instructie
```SQL
CREATE TRIGGER update_speler ON spelers FOR update
AS
DECLARE @old_snr smallint, @new_snr smallint

DECLARE before_cursor CURSOR FOR SELECT spelersnr FROM deleted
ORDER BY spelersnr

DECLARE after_cursor CURSOR FOR SELECT spelersnr FROM inserted
ORDER BY spelersnr

OPEN before_cursor
OPEN after_cursor

FETCH NEXT FROM before_cursor INTO @old_snr
FETCH NEXT FROM after_cursor INTO @new_snr

WHILE @@FETCH_STATUS = 0
BEGIN
    EXEC usp_mutatie_insert @old_snr,'U',@new_snr
    FETCH NEXT FROM before_cursor INTO @old_snr
    FETCH NEXT FROM after_cursor INTO @new_snr
END

DEALLOCATE before_cursor
DEALLOCATE after_cursor

-- Activatie van de trigger
update spelers set jaartoe=jaartoe + 20;
-- OF:
update spelers set spelersnr = spelersnr + 100;
```
### De IF UPDATE clausule
Voorwaardelijke uitvoering van triggers: uitvoering enkel als een specifieke kolom vernoemd wordt in een update of insert
```SQL
CREATE TRIGGER update_speler ON spelers FOR update
AS
DECLARE @old_snr smallint, @new_snr smallint

DECLARE before_cursor CURSOR FOR SELECT spelersnr FROM deleted ORDER BY spelersnr
OPEN before_cursor
IF update(spelersnr)
BEGIN
    DECLARE after_cursor CURSOR FOR SELECT spelersnr FROM inserted ORDER BY spelersnr
    OPEN after_cursor
END

FETCH NEXT FROM before_cursor INTO @old_snr
IF update(spelersnr)
    FETCH NEXT FROM after_cursor INTO @new_snr
ELSE
    SET @new_snr = @old_snr
    
WHILE @@FETCH_STATUS = 0
BEGIN
    EXEC usp_mutatie_insert @old_snr,'U',@new_snr
    FETCH NEXT FROM before_cursor INTO @old_snr
    IF update(spelersnr)
        FETCH NEXT FROM after_cursor INTO @new_snr
    ELSE
        SET @new_snr = @old_snr
END

DEALLOCATE before_cursor
IF update(spelersnr)
DEALLOCATE after_cursor
```

### Andere trigger condities
Ook normale condities kunnen in triggers
```SQL
IF datepart(hour, getdate()) >= 9
AND datepart(hour, getdate()) < 19
    BEGIN ... END
-- enkel tussen 9:00 en 19:00 uur is de trigger actief… 

IF USER IN ('JAN', 'PETER', 'MARK')
    BEGIN...END
-- enkel voor specifieke gebruikers de triggercode uitvoeren … 
```

### Voorbeeld: gecontroleerd bijhouden van redundante data
* Veronderstel dat de SPELERS-tabel een kolom SOM_BOETES bevat **(redundantie)**. Deze kolom bevat voor elke speler de som van zijn of haar boetes. Nu willen we triggers creëren die automatisch de waarden in deze kolom bijhouden **(integriteit)** . Hiervoor moeten we volgende triggers creëren
* Creëer zelf de update en delete triggers
```SQL
CREATE TRIGGER boete_insert ON boetes
FOR INSERT
AS
    DECLARE @boete smallint, @snr smallint
    SELECT @boete = bedrag, @snr = spelersnr from inserted
    update speler set som_boetes = som_boetes + @boete
    WHERE spelersnr = @snr
```
Opmerking: deze trigger werkt enkel indien de inserts gegarandeerd één per
één gebeuren  wegens:
```SQL
    SELECT @boete = bedrag, @snr = spelersnr FROM inserted)
```

Mogelijke aanpak voor de update en delete triggers:
```SQL
CREATE TRIGGER boete_del_upd ON boetes
FOR UPDATE, DELETE
AS
    DECLARE @boete as smallint, @snr as smallint
    SELECT @snr = spelersnr from deleted
    SELECT @boete = SUM(bedrag) FROM boetes WHERE spelersnr = @snr
    UPDATE spelers SET som_boetes = @boete
    WHERE spelersnr = @snr
```
**Opmerking:** deze trigger werkt enkel indien de update of deletes gegarandeerd één per één gebeuren wegens: 
```SQL
SELECT @snr = spelersnr from deleted
```
Kan deze trigger ook gebruikt worden voor insert?

### Opmerkingen
Naast verschillen in syntax; verschillen de SQL-producten tevens voor wat de **functionaliteit van triggers.** Enkele interessante vragen hierbij:
* Mogen voor één tabel en een bepaalde mutatie meerdere triggers
gedefinieerd worden?
    *   **volgordeproblemen die invloed kunnen hebben op het resultaat**
* Kan de verwerking van een instructie die behoort tot een trigger-actie
leiden tot het activeren van een andere trigger?
    * **één mutatie in een applicatie kan leiden tot een waterval van mutaties, recursie**
* Wanneer wordt nu precies een trigger-actie verwerkt?
    * **direct na de mutatie of pas voor de COMMIT-instructie**
* Mogen triggers gedefinieerd worden op catalogustabellen?
    * **je kan geen trigger instellen op een catalogustabel**  

## OOBDMS en ORDBMS
---
### Inleiding tot ORDBMS

* relationele DBMS is dominant
    * markt van ong. US$10 miljard/jaar (US$25 miljard met tools e.d.)
* OO DBMS markt is klein
    * kwam snel op tussen 1995 en 2000, maar is daarna gestagneerd.
    * er wordt niet verwacht dat dit de markt van RDBMS zal overnemen
* RDBMS verkopers zijn zich bewust van
    * de mogelijkheden van OODBMS
    * de mogelijke negatieve impact op hun product
    * de nood aan extra functionaliteit in hun producten om geavanceerde DB applicaties te ontwikkelen
* RDBMS verkopers
    * geloven dat het relationeel model kan uitgebreid worden zodat complexere applicaties kunnen ontwikkeld worden
    * geloven dat deze applicaties nog performant genoeg zullen zijn
    * voegen OO kenmerken toe aan het relationeel model

 **Enkele OO uitbreidingen**
 
1. gebruiker gedefinieerde types    
2.  encapsulatie
3. overerving
4. polymorfisme
5. dynamische method binding
6. complexe objecten
7. object identiteit

### Realiteit
* er is geen standaard 'extended' relationeel model
* alle modellen
    * hebben als basis relationele tabellen en query language
    * hebben 1 of ander concept van object
    * kunnen methodes opslaan
* ORDBMS behouden de opgebouwde kennis rond RDBMS
* aantrekkelijk alternatief
    * sommigen voorspellen dat het aandeel van ORDBMS op de DB markt meer dan 50%
groter zal zijn dan die van RDBMS

### Stonebraker's View
![alt text](http://puu.sh/poi0k/a7890f8b7b.jpg "Stonebrakers View")
### Voordelen van ORDBMS
* Oplossing voor de **grote zwaktes van het relationele model**
    * zwakke representatie van 'real-world' entiteiten
    * semantische overloading
    	* 'relatie' is enige constructie; hoe maak je het verschil tussen de relaties 'heeft', 'bevat', 'beheert',
    …
    * zwakke ondersteuning voor integriteit en andere constraints
    * homogene data structuur
    	* atomaire waarden, geen samengestelde waarden
    * beperkte operaties
    	* geen zelf gedefinieerde operaties
    * geen recursie
    * impedance mismatch (zie verder)
* **hergebruik en delen**
    * uitbreiden van de server functionaliteit om functionaliteit centraal uit te voeren
    * de centraal gedefinieerde functionaliteit kan nu gebruikt worden door alle applicaties
	    * voorbeeld: voor applicaties die met punten en lijnen werken bevat de server de functionaliteit om
afstanden te berekenen, dit hoeft nu niet in elke applicatie te gebeuren
* met als gevolg een **hogere productiviteit**
* behoud van **expertise en kennis** van RDBMS
**backwards compatibility**
    * mogelijkheid om als business geleidelijke overstap te maken van RDBMS naar
ORDBMS
	    * bij OODBMS is deze geleidelijke overschakeling onmogelijk

### Nadelen van ORDBMS
* **complexiteit** van het OR model
	* **eenvoud en puurheid** van het relationeel model gaat verloren
* verhoogde **kosten**
* enkel een klein deel van de applicaties kan gebruik maken van de object
extensies
* het is **niet puur OO**
	* OO applicaties zijn niet zo data gecentreerd
	* in pure OO zijn objects first class citizens, geen extensies van het relationele model
* **SQL is te complex** geworden

### Impedance mismatch
![alt text](http://puu.sh/poiC8/d53aca713e.jpg "Impendance mismatch")

**Alternatief voor OO BDMS of OR DBMS:**
* OR-mapping, vb.
    * .NET: Entity Framework, NHibernate
    * JPA: Java Persistence API
    * Java: Hibernate

## User Defined Types 
---
### UDT
* ~abstract data types
* kunnen gebruikt worden als built-in types
* 2 soorten
	* distinct types
	* structured types
* Ondanks de SQL:2008-standaard, toch heel
veel verschillen tussen DMBS'en
* In deze cursus: enkel bestaande syntax in
grote DBMS'en.

### Distinct type
Is gebasseerd op een **basis type** en laat toe **onderscheid** aan te brengen tussen anders gelijke basistypes.

Voorbeeld (MS-SQL Server):
```SQL
CREATE TYPE IDType FROM INT NOT NULL;
CREATE TYPE NameType FROM NVARCHAR(50) NOT NULL;
-- NameType = Distinct Type
-- NVARCHAR(50) = Het basistype waarvan het DT is afgeleid
```
> Het nieuwe type wordt opgeslagen in de datbank
 
>Het distinct type kan nu gebruikt worden net zoals een built-in datatype

Gebruik: 
![alt text](http://puu.sh/pojiE/d50fbb60b1.png "Gebruik Distinct Types")

### Structured Types
1. Table types 
2. Abstract data types (cf. OO)

### Table Types
**MS SQL Server**
```SQL
CREATE TYPE TotaalOrdersPerJaar AS TABLE
(
    jaar INT NOT NULL PRIMARY KEY,
    hoeveelheid INT NOT NULL
);

DECLARE @mijnordersperjaar AS TotaalOrdersPerJaar;

INSERT INTO @mijnordersperjaar
select year(orderdate) as jaar,round(sum(unitprice*quantity*(1-
discount)),2)
from orders o join orderdetails od
on o.OrderID=od.orderid
group by year(orderdate);

SELECT * FROM @mijnordersperjaar;

-- Ook: Rechtstreeks variabele declareren:
DECLARE @mijnordersperjaar AS
TABLE
(
    jaar INT NOT NULL PRIMARY KEY,
    hoeveelheid INT NOT NULL
);

```
###### Tables Types en Variables
* table types worden opgeslagen in de DB
* table variables bestaan slechts voor de duur van de batch (sequentie van statements)

Voordelen gebruik table variables:

* kortere en overzichtelijker code
* table type variabelen kunnen ook als parameter doorgegeven worden aan stored procedure en functions

### Abstract Data Types
* in het algemeen bevat een Abstract Data Type
    * definitie van **attributen**
    * definitie van **routines (methodes)**
* benamingen:
    * abstract data types (ADT)
    * user defined types (UDT)
    * object types
* = object-relationele aspecten

Voorbeeld: (Oracle, niet beschikbaar in MS SQL Server)
```SQL
CREATE TYPE customer_typ_demo AS OBJECT
    ( customer_id NUMBER(6)
    , cust_first_name VARCHAR2(20)
    , cust_last_name VARCHAR2(20)
    , cust_address CUST_ADDRESS_TYP
    , phone_numbers PHONE_LIST_TYP
    , nls_language VARCHAR2(3)
    , nls_territory VARCHAR2(30)
    , credit_limit NUMBER(9,2)
    , cust_email VARCHAR2(30)
    , cust_orders ORDER_LIST_TYP
) ;
```
### Subtypes, supertypes en methodes
Via **UNDER** kunnen **subtypes/supertype** verbanden gedefinieerd worden.
* geen multiple inheritance
* subtype erft
	* attributen
	* methodes
* het subtype kan
	* uitgebreid worden via definitie nieuwe attributen/methodes
	* gespecialiseerd worden via overloading
* een supertype kan altijd door zijn subtype
gesubstitueerd worden
* Men kan aan een ADT methodes toevoegen en deze implementeren in het CREATE TYPE BODY statement
```SQL
CREATE TYPE data_typ1 AS OBJECT
(
    year NUMBER,
    MEMBER FUNCTION prod(invent NUMBER) RETURN NUMBER
);

CREATE TYPE BODY data_typ1 IS
    MEMBER FUNCTION prod (invent NUMBER) RETURN NUMBER IS
        BEGIN
            RETURN (year + invent);
        END;
    END; 
```
* Subtypes (cf. inheritance): onderstaand voorbeeld creëert het sybtype
corporate_customer_typ, afgeleid van het supertype customer_typ (zie vorige
slide) en voegt het account_mgr_id attribuut toe:
```SQL
CREATE TYPE corporate_customer_typ_demo 
    UNDER customer_typ ( account_mgr_id NUMBER(6) );
```

### Structured Types
* NOT FINAL: Er kunnen nog subtypes gecreëerd worden
* FINAL (default): Er kunnen geen subtypes gecreëerd worden
```SQL
CREATE TYPE person_t AS OBJECT (name VARCHAR2(100), ssn NUMBER,
MEMBER FUNCTION get_name RETURN VARCHAR2(100)) NOT FINAL;

CREATE TYPE BODY person_t IS
MEMBER FUNCTION get_name() RETURN VARCHAR2(100) IS
        BEGIN
            RETURN name;
        END;
    END;
CREATE TYPE employee_t UNDER person_t (department_id NUMBER, salary NUMBER) NOT FINAL;

CREATE TYPE part_time_emp_t UNDER employee_t (num_hrs NUMBER)
FINAL;
```

Gebruik: 
```SQL
CREATE TABLE contacts (
    contact person_t,
    contact_date DATE );
INSERT INTO contacts VALUES (
    person_t ('John Smith',12586982),
    to_date('24 Jun 2003', 'dd Mon YYYY'));
SELECT c.contact.get_name() FROM contacts c;
```
## Large Objects
---
> Een large object is een **datatype die een grote
hoeveelheid aan data kan vasthouden**
– tekstbestand, afbeelding, muziek, video, web-pagina

Soorten:
* **BLOB**
    * Binary Large OBject
* **CLOB**
    * Character Large OBject
* **NCLOB**
    * National Character Large OBject

**Probleem** met LOB's die nu in verschillende DBMS
gebruikt worden
* LOB's worden aanzien als byte-stream
* **DBMS** kan zelf **niets doen** met het LOB
    *  DBMS kent de inhoud, de interne structuur niet
    * nochtans is het LOB dikwijls heel gestructureerd
        * gestructureerde tekst, web-pagina, …
* nadelige transfer van LOB's op server naar client over het netwerk

Voorbeeld (MS SQL Server)

* BLOB data types:
    * varbinary(max) en image (image = deprecated)
* CLOB data types (ASCII data):
    * varchar(max)
* NCLOB data types (unicode data):
    * nvarchar(max)
```SQL
ALTER TABLE Employees
ADD cv varchar(max);
ALTER TABLE Employees
ADD foto varbinary(max);
```



