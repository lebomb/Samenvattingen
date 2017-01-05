#Artificiële Intelligentie
## Hoofdstuk 3: Zoeken met een Tegenstander
---
### Inleiding
> In dit Hoofdstuk bekijken we hoe een agent moet handelen om zijn performantiemaat te maximaliseren wanneer er in de omgeving nog een andere (competitieve) agent aanwezig is. Verder gaan we er van uit dat de **omgeving voor beide agenten compleet observeerbaar** is en dat er voor elke toestand slechts een **eindig aantal mogelijke deterministische acties** zijn. De agenten spelen om de beurt, zo'n omgeving wordt vaak *een spel* genoemd. De spelers van het spel noemen we *Max* en *Min*.

Enkele definities die we in dit hoofdstuk gaan gebruiken:
**Tweepersoons Nulsomspel**: wordt gespeeld door twee spelers en bestaat uit volgende componenten:
*  Een **verzameling toestanden _S_**
*  Een **verzameling mogelijke acties _A_**
* Een **transitiemodel** dat zegt wat het effect is van het uitvoeren van een actie op een bepaalde toestand:  **_T : (S,A) -> S : (s, a) -> s'_**.  Wanneer *s'* bereikt wordt door het uitvoeren van een actie *a* op een toestand *s* dan wordt *s'* een **opvolger** van *s* genoemd.
* De **initiële toestand _s0_**: deze bepaalt de toestand voor er een zet werd gedaan
* Een **eindtest** die voor een toestand aangeeft of het spel in deze toestand gedaan is of niet. De toestanden waarvoor de eindtest *true* teruggeeft worden eindtoestanden genoemd.
* Een **opbrengstfunctie _U_**: De functie *U* zegt voor elke eindtoestand en voor elke speler wat de opbrengst is in deze toestand voor de gegeven speler. Omdat het spel een nulsomspel is, geldt voor alle eindtoestanden *s* dat: **_U(s, Max) + U(s, Min) = K_**, waarbij *K* een constante is die hoort bij het spel.

###Spelbomen en het Minimax Algoritme
De initiële toestand *s0* bepaalt, samen met het transitiemodel *T* en de eindtest een **spelboom** voor het gegeven spel.
De wortel van de spelboom is de initiële toestand.
De kinderen van een top zijn de opvolgers van de toestand horend bij de top onder het gegeven transitiemodel.
De blaadjes van de boom corresponderen met de eindtoestanden van het spel.
De waarden die worden geschreven bij deze blaadjes corresponderen met de opbrengstfunctie vanuit het standpunt van de speler Max.

De spelboom is vooral een theoretisch concept, want voor realistische spellen is deze veel te groot om volledig op te bouwen en op te slaan in het geheugen.

bv. Tic Tac Toe:

![alt text](http://users.hogent.be/~427143la/images/TicTacToe.PNG "TicTacToeBoom")

Speler Max is als eerste aan de beurt en wil uiteraard die actie selecteren die
zijn opbrengst zo groot mogelijk zal maken. Wanneer het spel gedaan zou
zijn na één zet dan zou Max eenvoudigweg de waarde van de opbrengstfunctie
in de eindtoestanden kunnen gebruiken. Echter, Min is ook nog in
het spel en Max weet dat Min er alles aan gaat doen om de opbrengst voor
Max te minimaliseren, terwijl Max ook weet dat Min weet dat Max de opbrengst
voor zichzelf wil maximaliseren. We zien hier dus een recursief proces.
De MINIMAX WAARDE van een toestand wordt dus als volgt bepaald:

![alt text](http://users.hogent.be/~427143la/images/MiniMax.PNG "Minimax")

> Uiteraard beschouwen we in de toestand *s* enkel die acties *a* die zinvol kunnen uitgevoerd worden op de toestand *s*.

Eens de Minimax waarde van een toestand is bepaald is het eenvoudig om de gepaste actie voor Max te selecteren. *Men kiest eenvoudigweg die __ actie waarvoor het maximum wordt bereikt.__* . Die noemt men dan de **Minimax Beslissing**.



### Snoeien van Spelbomen


### Praktische Uitwerking
