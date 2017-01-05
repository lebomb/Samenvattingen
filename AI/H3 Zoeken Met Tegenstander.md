#Artificiële Intelligentie
## Hoofdstuk 3: Zoeken met een Tegenstander
---
### Inleiding
> In dit Hoofdstuk bekijken we hoe een agent moet handelen om zijn performantiemaat te maximaliseren wanneer er in de omgeving nog een andere (competitieve) agent aanwezig is. Verder gaan we er van uit dat de **omgeving voor beide agenten compleet observeerbaar** is en dat er voor elke toestand slechts een **eindig aantal mogelijke deterministische acties** zijn. De agenten spelen om de beurt, zo'n omgeving wordt vaak *een spel* genoemd.

Enkele definities die we in dit hoofdstuk gaan gebruiken:
**Tweepersoons Nulsomspel**: wordt gespeeld door twee spelers en bestaat uit volgende componenten:
*  Een **verzameling toestanden _S_**
*  Een **verzameling mogelijke acties _A_**
* Een **transitiemodel** dat zegt wat het effect is van het uitvoeren van een actie op een bepaalde toestand:  **_T : (S,A) -> S : (s, a) -> s'_**.  Wanneer *s'* bereikt wordt door het uitvoeren van een actie *a* op een toestand *s* dan wordt *s'* een **opvolger** van *s* genoemd.
* De **initiële toestand _s0_**: deze bepaalt de toestand voor er een zet werd gedaan
* Een **eindtest** die voor een toestand aangeeft of het spel in deze toestand gedaan is of niet. De toestanden waarvoor de eindtest *true* teruggeeft worden eindtoestanden genoemd.
* Een **opbrengstfunctie _U_**: De functie *U* zegt voor elke eindtoestand en voor elke speler wat de opbrengst is in deze toestand voor de gegeven speler. Omdat het spel een nulsomspel is, geldt voor alle eindtoestanden *s* dat: **_U(s, Max) + U(s, Min) = K_**, waarbij *K* een constante is die hoort bij het spel.
