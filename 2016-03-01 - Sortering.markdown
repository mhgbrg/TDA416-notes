# Sortering

Sortering är en vanligt förekommande uppgift. Själva sorteringsproblemet i sig är inte så intressant, men många problem blir mycket lättare att lösa om man har sorterad indata. T.ex. sökning (binärsökning vs linjär sökning).

## Sortering i den här kursen

Vi kommer i den här kursen titta på sortering av heltal, men generellt används `Comparable` och `Comparator`. Vi kommer även använda oss av arrayer hela tiden, och inte länkade listor (i vissa fall kan man utnyttja länkade listors egenskaper för att få en snabbare algoritm).


## Stabil sortering

Om ett par av element x, y är lika och x ligger före y i indata-listan så ligger x före y även i den sorterade listan. Här spelar det alltså roll vilken ordning vi lägger element som är lika. Vi vill behålla den ursprungliga ordningen i så stor mån som möjligt.

## Olika sorteringsalgoritmer

* Selection sort
* Bubble sort
* Insertion sort
* Merge sort
* Quick sort
* Heap sort

### Selection sort

*Selection sort* fungerar så att man letar igenom hela listan efter det minsta (eller största) elementet, och lägger in det på "nästa" plats i den sorterade listan. Det här upprepas tills alla element i ursprungslistan har lagts till i den sorterade listan.

Tidskomplexiteten för den här algoritmen är `O(n^2)` både i bästa och värsta fall, eftersom man måste jämföra alla element med alla andra element. Fördelen med selection sort är att den kan utföras "in place" - alltså utan att allokera minne till en ny array.

### Bubble sort

*Bubble sort* är en sorteringsalgoritm som egentligen bara är användbar på listor som nästan är sorterade från början. Den fungerar så att man går igenom hela listan och byter plats på två närliggande element om de ligger i fel ordning. Om man lyckas göra en hel genomlöpning utan att göra en enda swap så är listan sorterad.

Tidskomplexiteten för den här algoritmen är även den `O(n^2)`, och den kan också utföras "in place".

### Insertion sort

*Insertion sort* kan ses som motsatsen till selection sort. I den här algoritmen går man igenom varje element i ursprunglistan och lägger den på rätt plats i den sorterade listan. Man börjar alltså med en tom sorterad lista som man bygger på allt eftersom.

Tidskomplexiteten för den här algoritmen är `O(n^2)` (samma som selection sort), och även den här algoritmen kan utföras "in place". Fördelen med den här algoritmen över selection sort är dock att den kan köras på `O(n)` tid i bästa fall.

Insertion sort kan vara effektiv när man har en lista där alla element kan läsas in i minnet samtidigt.

### Merge sort & quick sort

Både *merge sort* och *quick sort* använder sig av divide-and-conquer. Det man gör är att dela upp problemet i mindre problem, lösa varje problem för sig och sätta ihop alla lösningar till en final lösning till ursprungsproblemet.

#### Merge sort

Det merge sort gör är att upprepat dela den ursprunliga listan på mitten tills varje lista bara har ett element. Sedan *mergas* de här listorna ihop två och två tills listan är hel igen. Det är i merge-processen som själva sorteringen görs.

Tidskomplexiteten för merge sort är `O(n*log(n))`. Vi har `log(n)` nivåer i rekursionen, och det utförst `O(n)` operationer på varje nivå (split och merge).

Nackdelen med merge sort är att man måste allokera nya arrayer när man mergar, så den kräver en del extra utrymme utöver ursprungsarrayen.

#### Quick sort

