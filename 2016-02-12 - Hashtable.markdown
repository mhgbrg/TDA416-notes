# Hashtable

Hashtable är en datastruktur som ger konstant tid för access, insert och delete.

Det fungerar som så att man har en array med `k` *buckets*, där varje bucket innehåller en länkad lista (eller ett binärt sökträd).

När man ligger in ett element så omvandlar man elementet till ett heltal (`hashCode()` i Java) och kör modulo med antalet buckets i tabellen. Då får man indexet till vilken bucket värdet ska lagras i. Det här kallas för att *hasha* ett värde.

    int i = value.hashCode() % k;

Nästa steg är att lägga till värdet i bucketen på index `i`. Det kan hända att den här bucketen inte är tom, men eftersom varje bucket innehåller en länkad lista så kan den innehålla flera värdet. Om listan är tom så lägger man in elementet direkt, om listan inte är tom så går man igenom hela listan och lägger till elementet i slutet. Om elementet redan fanns i listan så ersätter man värdet.

Har man otur så hashas alla element till samma index, och då kommer alla värden sparas i samma länkade lista. Då blir tidskomplexiteten för de vanliga operationerna `O(n)` (eller `O(log(n))` i fallet med ett BST) istället för `O(1)`. Det här kan förhindras med mer avancerade hash-funktioner som ger bättre spridning.

## Hashcode

Ett sätt att räkna ut `hashCode` i ett objekt är multiplicera alla beroende variabler i objektet med en potens av ett primtal.

## Collisions

Det finns två sätt att hantera kollisioner i en hashtable:
* Open addressing
* Chaining (closed addressing)

### Open addressing

När man använder *open addressing* och man försöker lagra ett objekt på en plats som är upptagen så försöker man lagra objektet på en annan plats som är relativ till ursprungsplatsen.

En metod som man kan använda är *linear probing*. Det här innebär att man fortsätter leta på nästa index tills man hittar en tom plats. Den här metoden har en tendens att bilda kluster som gör att operationer tar lång tid. Andra metoder som förhindrar det är *quadratic probing* där intervallet mellan de index man kollar är kvadratiskt, t.ex. i+1, i+4, i+9, i+16 etc.

När man söker efter ett objekt i en hashtable med open addressing så hashar man objektet och letar enligt probe-metoden tills antingen objektet hittas eller en tom plats hittas. Om en tom plats hittas så finns inte objektet i tabellen.

När vi tar bort ett element ur en sådan här hashtable så kan vi inte bara tömma rätt index i arrayen, för då kommer sökningen att gå sönder. Det man istället gör är att markera elementet som borttaget. När vi sedan söker så bortser vi från de här borttagna elementen och fortsätter tills vi antingen hittar det sökta elementet eller en helt tom plats. När vi lägger till nya element så betraktar vi de borttagna platserna som tomma.

### Chaining (closed addressing)

Vid *chaining* så innehåller varje index i tabellen en ny datastruktur, vanligtvis en länkad lista men kan även vara ett binärt sökträd. 

## Rehashing

När antalet upptagna platser i tabellen närmar sig arrayens maxstorlek så blir den genomsnittliga tiden för operationerna längre. Man brukar kalla måttet på hur full tabellen är för *load factor*, och det är antalet element i tabellen delat på antalet index i arrayen.

    Load factor = n / k
    n = number of entries
    k = number of buckets

När load factorn når en viss gräns så vill man utöka storleken på den bakomliggande arrayen så att load factorn minskas. När man gör det här så måste man hasha om varje element i tabellen, eftersom hashen räknas ut med hjälp av antalet index i arrayen. Det här kallas för *rehashing*.

## Open addressing vs chaining

Det genomsnittliga antalet jämförelser `c` för en operation är:

    Open addressing:
    c = 1/2(1 + 1/(1 - L))
    
    Chaining:
    c = 1 + L/2

**Se slides för en tabell över hur lång tid det tar i praktiken vid olika load factors.**

Man ser att chaining faktiskt alltid ger bättre komplexitet än open addressing. Det enda som är bra med open addressing är att alla element ligger nära varandra i minnet.

## Nackdelar med hashtables

* Hashtables behöver rehashas. Det tar mycket tid (även om man gör det sällan)
* Den garanterade komplexiteten för operationerna är O(n) (fast den genomsnittliga är O(1))
* Det är svårt att traversera elementen i ordning (till skillnad från till exempel ett binärt sökträd)

