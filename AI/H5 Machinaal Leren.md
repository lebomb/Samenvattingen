# Artificiële Intelligentie
## Hoofdstuk 5: Machinaal Leren
---

### Machinaal Leren
> Het deelveld van de artificiële intelligentie dat computers de **mogelijkheid geeft om te leren zonder dat ze hiervoor _expliciet_ geprogrammeerd zijn**.  

### 3 Types van Machinaal Leren

1. **Gesuperviseerd Leren:** op basis van een **gelabelde dataset** een **hypothese** op bouwen waarmee, voor een nieuwe invoer het label kan voorspeld worden.
    * Wanneer dit label een (reëel) getal is, dan spreekt men van een **Regressieprobleem**.
    * Wanneer het label 1 van een (klein) aantal voorgedefinieerde klassen is dan spreekt men van een **Classificatieprobleem**.
    In hetgeval dat er slechts 2 klassen zijn, spreekt men van een **binair** classificatieprobleem.

    ![alt text](http://users.hogent.be/~427143la/images/GesuperviseerdSchema.JPG "Schema")

2. **Ongesuperviseerd Leren**: heeft de taak een **structuur te ontdekken** in een **ongelabelde** dataset. Voorbeelden:
    * **Clustering**: Het ontdekken van coherente groepen. (= Meest voorkomende)
    * **Anomaliedetectie**: Het ontdekken van zaken die niet conform zijn met het verwacht patroon of andere zaken in de dataset.
    * **Primaire Componenten Analyse**

3. **Reinforcement Learning**: Werkt in plaats van datasets met **beloningssignalen**, die negatief zijn wanneer de agent een “slechte” handeling stelt en positief wanneer de agent een goede handeling stelt. Eenvoudig gezegd bestaat de taak van reinforcement learning erin om te **leren welke acties**, i.e. welk beleid, **leiden tot de hoogste totale beloning.**


### Evaluatie van Hypothesen voor Gesuperviseerd Leren.
#### Foutmaten
Om gemakkelijk verschillende hypothesen te vergelijken gebruikt men vaak een *numerieke maat* voor de fout over een verzameling van voorbeelden. Kleinere numerieke waarden duiden (meestal) op betere hypothesen.

Voor een **Regressieprobleem** neemt men als maat voor de fout vaak de **Gemiddelde kwadratische afwijking** tussen de voorspelde labels en de werkelijke labels over de verzameling voorbeelden.

In formulevorm:
![alt text](http://users.hogent.be/~427143la/images/Form1.JPG "Formule")

Voor een **Classificatieprobleem** gebruikt men vaak de **Foutratio**, dit is het percentage voorbeelden dat het verkeerde label toegewezen krijgt.

In formulevorm:
![alt text](http://users.hogent.be/~427143la/images/Form2.JPG "Formule")

Voor een **Binair Classificatieprobleem**, waar er een zeer scheve verdeling is tussen de klassen, is de **foutratio niet altijd de meest geschikte** manier om een hypothese te evalueren.
  * bv: Veronderstel dat men een hypothese heeft opgezet om te voorspellen of iemand een kwaadaardige kanker (y=1) heeft ofniet (y=0). Stel dat de foutratio over een bepaalde verzameling patiënten gelijk is aan 1%, of anders gezegd, we krijgen de juiste diagnose in 99% vande gevallen. Op het eerste zicht lijkt deze hypothese het echt goed te doen.Echter, als je weet dat er slechts een half procent van de patiënten in de dataset effectief kanker heeft dan heeft een triviaal algoritme dat steeds y=0 voorspelt, zonder naar de attributen te kijken, een foutratio van slechts een half procent! Het is duidelijk dat in zo’n geval de foutratio niet geschiktis.

We bekijken nu eventjes de vier verschillende uitkomsten die kunnen voor-komen bij een binair classificatieprobleem:

/ | y = 1 | y = 0
--- | --- | ---
h(*x*)=1 | correct (*a*) | vals positief (*b*)
h(*x*)=0 | vals negatief (*c*) | correct (*d*)

De **Precisie** zegt welk percentage van de voorbeelden die voorspeld waren als positief, ook effectief positief waren:

 ![alt text](http://users.hogent.be/~427143la/images/Form3.JPG "Formule")

De **Rappel** (Eng: recall) zegt welk percentage van de positieve voorbeelden ook effectief als positief werd gelabeld door de hypothese:

![alt text](http://users.hogent.be/~427143la/images/Form4.JPG "Formule")

**Een algoritme dat alle voorbeelden het correcte label geeft, heeft steeds een  precisie en rappel die allebei gelijk zijn aan 1.** Een algoritme dat steeds *h(x)=1* voorspelt heeft een rappel die gelijk is aan 1 (want alle positieve voorbeelden werden als positief voorspeld), maar de precisie is in dit geval slechts gelijk aan het percentage positieve voorbeelden in de dataset. Om de precisie te verhogen is het dus nodig om ook het label *0* te gaan voorspellen, maar dan loop je uiteraard het risico dat de rappel daalt.

Het is vaak interessant om de performantie van een hypothese te kunnen uitdrukken als één enkel getal. De precisie en de rappel kunnen gecombineerd worden in één enkele score, de **_F_ -score**:

 ![alt text](http://users.hogent.be/~427143la/images/Form5.JPG "Formule")

 > In dit geval zijn **grotere waarden** voor precisie, rappel en de F-score beter dan kleinere waarden.

 > Het gemiddelde nemen van de precisie en de rappel leidt **niet** tot een goede maat om hypothesen te vergelijken.

#### Trainings-, Validatie- en Testdata
 Eens we een maat gekozen hebben rijst de vraag voor welke verzameling we deze maat wensen te optimaliseren. We wensen een hypothese die het goed doet op nieuwe ongeziene voorbeelden. Dus een hypothese die de voorbeelden uit de traingsdataset **generaliseert** en er de patronen in herkent.

 > Het is foutief om te veronderstellen dat een hypothese die zeer goed scoort op de trainingsvoorbeelden ook onmiddelijk goed zal scoren op neuwe ongeziene voorbeelden.

 Om in te schatten hoe goed een hypothese scoort op nieuwe data gebruiken we een verzameling gelabelde voorbeelden die de **testdata** wordt genoemd.

 `Het is van uiterst belang dat deze testdate op geen enkele manier wordt gebruikt bij het opstellen van de hypothese`

 Een hypothese die zeer goed scoort op de trainingsdata maar die slecht scoort op de testadata leidt aan **overfitting**.
 Om overfitting te vermijden gebruiken sommige modellen meta-parameters of moet er beslist worden wanneer een iteratief trainingsproces wordt gestopt. Bij deze beslissing mag geen gebruik gemaakt worden van de testdata, daarom gebruikt men vaak nog een (derde) dataset die dan de **validatiedataset** wordt genoemd.

  ![alt text](http://users.hogent.be/~427143la/images/OverfittingGrafiek.PNG "OverfittingGrafiek")

  Overfitting begint wanneer de fout op de validatiedataset begint te stijgen. De blauwe lijn stelt de fout voor op de trainingsdata, normaalgezien daalt deze naarmate de training vordert of naarmate het model complexer wordt. Het beste model wordt dus wellicht verkregen op het moment dat de foutmaat op de **validatiedataset** minimaal wordt. *De uiteindelijke schatting voor de foutmaat moet echter gedaan worden adhv een derde dataset: de testdate. Maar deze mag op geen enkel moment gebruikt worden tijdens de trainingsfase*

 Wanneer de klassen van hypothesen waaruit gekozen kan worden niet groot genoeg is om een hypothese te vinden die goed generaliseert spreken we van **onderfitting**.

#### Ockhams Scheermes:
 Wanneer er moet gekozen worden tussen twee of meer modellen die de data goed verklaren dan moet men het meest eenvoudig model kiezen.
