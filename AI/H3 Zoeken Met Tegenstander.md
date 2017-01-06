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

Eens de Minimax waarde van een toestand is bepaald is het eenvoudig om de gepaste actie voor Max te selecteren. *Men kiest eenvoudigweg die* **_actie waarvoor het maximum wordt bereikt._**

Die noemt men dan de **Minimax Beslissing**, dit is dus de beste beslissing wanneer er gespeeld wordt tegen een tegenstander die ook optimaal speelt. Een andere beslissing dan de minimax beslissing, kan en zal door een optimaal spelende tegenstader uitgebuit worden om ervoor te zorgen dat zijn eigen opbrengst zal stijgen, of gelijk blijven.

#### Minimax Algoritme
`Voor toelichting check cursus Algoritme 3.1`

![alt text](http://users.hogent.be/~427143la/images/MinimaxAlgoritme.PNG "Minimax Algoritme")


### Snoeien van Spelbomen
Bij een spelboom is *niet elke waarde relevant* voor het eindresultaat:

We berekenen de minimax waarde van toestand a. We vinden:

        minimax(a) = max(minimax(b), minimax(c), minimax(d))
                   = max(min(12, 3, 8), min(1, 5, 7), min(14, 5, 1))
                   = max(3, 1, 1)
                   = 3.
                   
Veronderstel nu dat de laatste twee opvolgers van toestand *c* onbepaalde
waarden *x* en *y* hebben. In dit geval vinden we voor de minimax waarde van *a*:

        minimax(a) = max(minimax(b), minimax(c), minimax(d))
                   = max(min(12, 3, 8), min(1, x, y), min(14, 5, 1))
                   = max(3, z, 1)                                      (met z ≤ 1)
                   = 3

Dus wat *x* en *y* ook zijn, de minimaxwaarde van *a* blijft 3, merk op dat het niets uitmaakt dat de laatste twee opvolgers van *c* eindtoestanden zijn. De redenering gaat ook op als *x* en *y* het gevolg zijn van een spel met duizenden mogelijke zetten, in dit geval kan er dus veel tijd bespaard worden door deze takken van de spelboom niet te evalueren.

> Het **niet evalueren van een tak** in een spelboom wordt het **snoeien** (Eng: pruning) van die tak genoemd.

De parameter *a* (alfa) houdt de waarde bij van de beste keuze (hoogste waarde)



### Praktische Uitwerking
