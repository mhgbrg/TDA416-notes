# ADT, generics

## ADT

ADT = Abstract Data Type.

En **abstrakt datatyp** är den matematiska definitionen av en datatyp. Vad som menas med det är att den är definierad utifrån *operationer* på datatypen. Det här är i konstrast mot **datastrukturer**, där den faktiskta implementationen av operationerna är definierad.

T.ex. i Java så är `List` en abstrakt datatyp, medan `LinkedList` och `ArrayList` (som är två olika implementationer av listor) är datastrukturer.

I Javas collectionspaket så finns det oftast både interfaces och abstrakta klasser som motsvarar abstrakta datatyper. `List` är ett **interface** och `AbstractList` är en **abstrakt klass**. Både `LinkedList` och `ArrayList` ärver från `AbstractList`.

Abstrakta datatyper är endast definierade av de operationer som kan utföras på datatypen. Man bryr sig alltså inte hur operationerna är imlementerade eller hur lång tid de tar att utföra, utan bara vad de åstadkommer.

Specifikationen hos en ADT består av *syntax* och *semantik*. Semantik är helt enkelt en beskrivning av exakt vad som ska hända när en viss operation körs på datatypen. Kan använda sig av pre och post-conditions i Java.

