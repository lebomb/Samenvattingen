# Artificiële Intelligentie
## Hoofdstuk 1: Rationale Agenten
---
**Turing-test:**
> Operationele definitie van intelligentie opgesteld door Alan Turing in 1950. Een computer slaagt in de Turing-test wanneer een **menselijke ondervrager niet kan uitmaken** of de geschreven antwoorden van een **computer of een mens** afkomstig zijn.

Om voor deze test te slagen moet de computer volgende deelvelden van AI goed beheersen:
1. Goede beheersing van natuurlijke taal om vlot te kunnen communiceren.
2. Kennisrepresentatie om te kunnen opslaan wat hij weet of hoort.
3. Geautomatiseerd redeneren om de opgeslagen kennis te gebruiken om vragen te beantwoorden en zelf nieuwe conclusies te kunnen afleiden.
4. Machinaal leren om zich aan nieuwe omstandigheden of patronen te kunnen aanpassen. 

In de uitgebreide Turing test beschikt de ondervrager over een camera om de perceptuele mogelijkheden van de ondervrager te testen, maar ook een luik om objecten door te geven aan de ondervrager. Voor deze test moet de computer naast bovenstaande vaardigheden ook de volgende beheersen: mogelijkheid om te zien en robotica om objecten te manipuleren. 

**Rationale Agenten**
> Een **agent** is elke entiteit die zijn **omgeving** kan waarnemen aan de hand van zijn **sensoren** en die invloed kan uitoefenen op zijn omgeving aan de hand van zijn **actuatoren**

Een rationale agent moet ook beschikken over een performantiemaat, deze moet niet het gedrag, maar het gewenste resultaat belonen. 

> Een **rationale agent** selecteert, voor elke mogelijke waarnemingssequentie, die actie waarvan verwacht wordt dat deze zijn **performantiemaat maximaliseert**, rekening houdend met het bewijs aangebracht door de huidige waarnemingssequentie en de eventuele ingeboudwde kennis van de agent. 

**Eigenschappen van omgevingen** 

1. **Compleet observeerbaar**: alle relevante aspacten zijn zichtbaar om een volgende actie te ondernemen. 
2. **Partieel observeerbaar**
3. **Eenpersoons**: de agent handelt alleen in de omgeving
5. **Multipersoons**:  wanneer er meerdere agenten zijn. 
    * **competitief** 
    * **coöperatief** 
        In sommige gevallen kunnen agenten beide gedragenvertonen.
5. **Deterministisch**: de volgende toestand van de omgeving wordt volledig bepaald door de huidige toestand en de actie die werd ondernomen door de agent.
6. **Stochastisch**: de volgende toestand van de omgeving wordt niet volledig bepaald door de huidige omgeving en de actie ondernomen door de agent. (bv. kansen)
7. **Episodisch**: de ervaring van de agent wordt opgedeeld in verschillende onafhankelijke episodes. De actie die wordt ondernomen in de huidige episode heeft *geen* invloed op de volgende episode. 
8. **Sequentieel**: de huidige actie heeft wel een (potentiële) invloed op alle volgende acties.
9. **Statisch**: de omgeving wijzigt niet terwijl de agent nadenkt over zijn volgende actie.
10. **Dynamisch**: de omgeving wijzigt wel tijdens het nadenken. 
11. **Discreet**: er zijn een eindig aantal toestanden. 
12. **Continu**: er zijn een oneindig aantal mogelijke acties.

**Structuur van Agenten**
Op een hoog niveau worden agenten onderverdeeld in 4 types, deze zijn de volgende (in oplopende volgorde van complexiteit en  bruikbaarheid):

1. **Eenvoudige reflex:** 
De agent heeft geen geheugen en neemt volgende actie enkel en alleen op basis van de **huidige waarneming** (**conditie-actie regel**, bv. ALS auto voor mij remt DAN rem).

2. **Modelgebaseerde reflex:** 
De agent houdt een **inschatting** bij van wat de **huidige toestand** is, deze inschatting is in het algemeen *niet gelijk* aan de werkelijke toestand. De agent beschikt over een **model van de manier waarop de toestand wijzigt**, zowel onafhankelijk van de agent (bv. physics van de wereld) als door de acties van de agent zelf. Telkens wanneer een nieuze waarneming binnenkomt wordt de inschatting van de huidige toestand aangepast m.b.v. het model, de waarneming en de laatst ondernomen actie. **Daarna worden de conditie-actie regels** losgelaten op de inschatting van de huidige toestand. 

3. **Doelgebaseerde: (= modelgebaseerde + doel + denken)**
De inschatting die gedaan wordt bij de modelgebaseerde reflex, of zelfs de volledige kennis van de huidige toestand, is niet steeds voldoende om te weten wat je moet doen. De agent heeft in deze situaties een **beschrijving van het doel** nodig. Hierbovenop **denkt** een doelgebaseerde agent ook **hoe de omgeving kan evolueren** op basis van zijn acties. Deze agent is veel **flexibeler** dan de modelgebaseerde, wanneer die een nieuwe bestemming wil geven moeten alle conditie-actie regels herschreven worden, voor de doelgebaseerde enkel de bestemming.  

4. **Utiliteitsgebaseerde: (= doelgebaseerde + performantie)**
Een doelgebaseerde agent is dichotoon: een toestand is een doeltoestand of niet en maakt dus geen onderscheid in performantie tussen deze toestanden. **Utiliteitsgebaseerde agenten** hebben een **utiliteitsfunctie** die **aangeeft hoe performant een toestand is** (~ internalistatie v.d performantiemaat). Wanneer utlititeitsfunctie en performantiemaat overeenkomen dan zal een utiliteitsgebaseerde agent die zijn utiliteit gaat maximaliseren ook meteen zijn performantiemaat gaan maximaliseren. Hierdoor is ze dus veel **flexibeler dan een doelgebaseerde agent**.








































