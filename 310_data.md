## Werken met bytes 

Tot nu toe hebben we vaak tekst gemanipuleerd in Python, maar in IoT-toepassingen kom je ook heel wat andere types data tegen. Sensormetingen zijn vaak getallen, maar die kunnen op allerlei manieren gecodeerd zijn. In dit deel van de cursus hebben we het over verschillende manieren waarop data gecodeerd kunnen zijn en hoe we ze decoderen of omzetten naar een andere vorm. Want daar gaat het ook vaak om bij IoT-toepassingen: data in één formaat van één systeem doorsturen naar een ander systeem dat een ander formaat verwacht.

### Bytes en bytearrays

Python kent twee datatypes voor een reeks van bytes: `bytes` en `bytearray`. Beide zijn te vergelijken met een lijst van gehele getallen van 0 tot en met 255 (de maximumwaarde van een byte), maar er is een belangrijk verschil: een object van het type `bytes` is niet meer te wijzigen, een `bytearray` wel.

Bytes gebruik je doorgaans als je data van een sensor wilt decoderen en niets hoeft aan te passen. Als je zelf data in een specifiek formaat wilt doorsturen, is een bytearray vaak handiger. Je creëert dan een bytearray van de gewenste lengte en vult die stap voor stap met de juiste bytes, en je kunt zelfs nog bytes aan het einde toevoegen en zo bijvoorbeeld byte per byte de bytearray opbouwen.

#### Bytes

Je kunt een object van het type `bytes` aanmaken op basis van een lijst van getallen:

```python
>>> bytes([65, 66, 67])
b'ABC'
```

Je ziet nu dat Python het resultaat op een andere manier voorstelt: `b'ABC'`. Dit is een _bytes literal_, en je kunt dit object ook op deze manier aanmaken:

```python
>>> b'ABC'
b'ABC'
```

Waarom staan hier de letters A, B en C in? Omdat de ASCII-waarde (zie eerder) van het teken A het getal 65 is, van B 66 en van C 67.

Bytes die niet als een leesbaar teken voorgesteld kunnen worden, toon je met een _escape sequence_ die begint met `\x` en waarachter twee hexadecimale tekens (0 tot en met f) komen die samen het byte voorstellen. Bijvoorbeeld:

```python
>>> bytes([31, 65, 66, 67])
b'\x1fABC'
```

De waarde 31 is in hexadecimale voorstelling 1f (1 x 16 + 15), dus in onze bytes literal wordt dit `\x1f`.

Een object van het type `bytes` kun je ook eenvoudig omzetten naar een lijst van integers:

```python
>>> list(b"\x1fABC")
[31, 65, 66, 67]
```

Maar als je puur in de getalwaardes van de bytes en niet in de ASCII-voorstelling geïnteresseerd bent, is de standaardvoorstelling wat lastig te lezen, met een mix van escape sequences en leesbare tekens. Je kunt die echter eenvoudig omzetten in een string van hexadecimale cijfers voor alle bytes met de methode `hex()`:

```python
>>> b'\x1fABC'.hex()
'1f414243'
```

Je kunt nu eenvoudig de waarde van de opeenvolgende bytes aflezen. Je kunt ook een separator zoals een spatie tussen elke byte toevoegen voor wat meer overzicht:

```python
>>> b'\x1fABC'.hex(" ")
'1f 41 42 43'
```

Merk op dat de waardes hexadecimaal zijn, dus 41, 42 en 43 en niet 65, 66 en 67 zoals het object waarmee we begonnen. 41 hexadecimaal is 4 x 16 + 1 = 65 decimaal.

De omzetting in de andere richting is ook mogelijk: van een string met een hexadecimale voorstelling naar een object van het type `bytes`:

```python
>>> bytes.fromhex("1f 41 42 43")
b'\x1fABC'
```

Witruimte in de string wordt genegeerd.

#### Bytearray

Voor bytearrays bestaat er geen literal syntax. Maar je kunt een object van het type `bytes` eenvoudig omzetten naar een bytearray:

```python
>>> bytearray(b"\x1fABC")
bytearray(b'\x1fABC')
```

En dat object kun je wijzigen. Overigens werken alle omzettingen zoals met `hex()`, `fromhex()` en `list()` ook voor bytearrays.

Een bytearray wijzigen kan zoals met een lijst, bijvoorbeeld met de methode `append()`:

```python
>>> data = bytearray(b'\x1fABC')
>>> data.append(16)
>>> data
bytearray(b'\x1fABC\x10')
```

We hebben het getal 16 (`\x10`) aan het einde van de bytearray toegevoegd. Maar we kunnen ook een byte op een specifieke index veranderen:

```python
>>> data[1] = 67
>>> data
bytearray(b'\x1fCBC\x10')
```

Merk op dat we dit niet kunnen doen:

```python
>>> data[1] = "C"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object cannot be interpreted as an integer
```

"C" is immers een string en geen byte. We kunnen ook niet dit doen:

```python
>>> data[1] = b"C"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'bytes' object cannot be interpreted as an integer
```

Want `b"C"` is een object van het type `bytes` en geen getal, ook al bevat dit object maar één getal.

Wat als je dan de ASCII-waarde van het teken "C" wilt toevoegen, maar je niet weet dat dit de waarde 67 heeft? Python heeft daarvoor een functie:

```python
>>> ord("C")
67
```

#### Van en naar strings

We hebben nu met bytes en bytearrays gewerkt en deze omgezet van en naar hexadecimale cijferreeksen of lijsten van getallen. Maar wat als deze effectief een string voorstellen en we deze naar een object van het type string willen omzetten? Dat kan voor beide types met de methode `decode()`:

```python
>>> bytes([65, 66, 67])
b'ABC'
>>> bytes([65, 66, 67]).decode()
'ABC'
```

Het verschil lijkt klein, met de extra b al dan niet vooraan. Het eerste object is van het type `bytes` en het tweede van het type `str`.

Andersom kun je ook een string naar een reeks van bytes omzetten:

```python
>>> "ABC".encode()
b'ABC'
```

Het resultaat is een object van het type `bytes`.

Voor ons lijkt het verschil misschien triviaal, maar sommige functies in Python verwachten een string en andere bytes of een bytearray, dus al deze omzettingen zijn regelmatig nodig om het juiste resultaat te verkrijgen.

### Getallen als bytes voorgesteld

Als sensoren getallen in de vorm van bytes voorstellen, kan dat op meerdere manieren. De kunst is dus om deze bytes op de juiste manier naar een getal om te zetten zodat je hierop verder bewerkingen kunt uitvoeren. We beperken ons hier tot gehele getallen (`int`). Het omzetten van bytes naar int en andersom kan met de functies `from_bytes()` en `to_bytes()`.

#### Eén byte

In één byte kunnen alle getallen van 0 tot en met 255 voorgesteld worden. Dit omzetten naar een int kan met:

```python
>>> int.from_bytes(b"\xff")
255
```

Maar er bestaat ook een manier om negatieve getallen voor te stellen in een byte: _two's complement_. Dan kan één byte de waardes -128 tot en met +127 voorstellen:

```
  0 00000000    0
  1 00000001    1
  2 00000010    2
...
126 01111110  126
127 01111111  127
128 10000000 -128
129 10000001 -127
130 10000010 -126
...
254 11111110   -2
255 11111111   -1
```

Dan stelt de byte \xff dus niet 255 maar -1 voor. Die omzetting met two's complement doen we in Python met een extra argument `signed`:

```python
>>> int.from_bytes(b"\xff", signed=True)
-1
```

Voor de omzetting in de andere richting gebruik je de methode `to_bytes()` op het getal. Maar als je dit zo zou doen, krijg je een foutmelding:

```python
>>> 255.to_bytes()
  File "<stdin>", line 1
    255.to_bytes()
       ^
SyntaxError: invalid decimal literal
```

Dit gebeurt omdat de punt die we hier gebruiken als afscheiding tussen het object en de methode ook geïnterpreteerd kan worden als een kommateken in een getal. We moeten de methode dus als volgt aanroepen:

```python
>>> (255).to_bytes()
b'\xff'
```

Zo is er geen onduidelijkheid over wat het punt betekent.

Wat als we nu een negatief getal willen omzetten naar een byte?

```python
>>> (-1).to_bytes()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
OverflowError: can't convert negative int to unsigned
```

Python zegt ons dat het geen negatief getal naar een "unsigned" type kan omzetten. We moeten dus aangeven dat we met getallen met minteken willen werken en voor de omzetting naar bytes two's complement willen gebruiken:

```python
>>> (-1).to_bytes(signed=True)
b'\xff'
```

#### Meerdere bytes

Vaak wil je echter grotere getallen dan 255 of -128 voorstellen. Dan heb je meerdere bytes nodig. Hoe doe je dit? Ook daarvoor bestaan verschillende manieren. We bekijken eerst positieve getallen. Met twee bytes kun je 256 in het kwadraat getallen voorstellen, of 65.536, namelijk de getallen 0 tot en met 65.535. Bijvoorbeeld:

```
...
  254 00000000 11111110
  255 00000000 11111111
  256 00000001 00000000
  257 00000001 00000001
  258 00000001 00000010
...
65534 11111111 11111110
65535 11111111 11111111
```

Je kunt nu op basis van deze twee bytes afzonderlijk de getalwaarde berekenen: die is immers gelijk aan 256 vermenigvuldigd met het eerste byte, en daarbij het tweede byte opgeteld. Maar je hoeft zelf die berekening niet uit te voeren. De methode `from_bytes()` kan immers ook meer dan één byte tegelijk omzetten naar een `int` en zal dit met de juiste berekening doen:

```python
>>> int.from_bytes(b"\x01\x02")
258
```

Dit werkt ook als een getal door meer bytes voorgesteld wordt, bijvoorbeeld vier bytes (maximum 4.294.967.296 getallen):

```python
>>> int.from_bytes(b"\xff\xff\xff\xff")
4294967295
```

De two's complement voorstelling voor negatieve getallen bestaat ook voor meerdere bytes:

```python
>>> int.from_bytes(b"\xff\xff\xff\xff", signed=True)
-1
```

Met `to_bytes()` kun je een int naar een reeks bytes omzetten. Maar als het om meer dan één byte gaat, moet je de lengte in bytes aangeven. Bijvoorbeeld:

```python
>>> (258).to_bytes(2)
b'\x01\x02'
```

Is het getal te groot voor het aantal opgegeven bytes, dan krijg je een exception:

```python
>>> (258).to_bytes()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
OverflowError: int too big to convert
```

Om negatieve getallen in two's complement voor te stellen, voeg je het argument `signed` toe dat we al zagen:

```python
>>> (-1).to_bytes(2, signed=True)
b'\xff\xff'
>>> (-1).to_bytes(4, signed=True)
b'\xff\xff\xff\xff'
```

Er is nog een belangrijk verschil mogelijk: in welke volgorde zet je de opeenvolgende bytes van een getal? In de voorbeelden tot nu toe kwamen de _meest significante_ bytes het eerst:

```
258 00000001 00000010
```

Maar heel wat apparaten en programma's gebruiken een andere volgorde:

```
258 00000010 00000001
```

Die eerste voorstelling noemen we _big endian_, omdat de software de _big end_ van het getal eerst leest. De tweede voorstelling heet _little endian_, omdat de software de _little end_ van het getal eerst leest.

Standaard leest `from_bytes()`_ een reeks bytes in the big endian volgorde in, maar je kunt dit aanpassen met het argument `byteorder`:

```python
>>> int.from_bytes(b"\x02\x01")
513
>>> int.from_bytes(b"\x02\x01", byteorder="little")
258
```

We zien hier dus twee keer dezelfde reeks bytes, maar op een andere manier geïnterpreteerd. Dit werkt overigens ook samen met `signed=True`. In big endian formaat bepaalt het hoogste (eerste) bit van de eerste byte het teken van het getal, terwijl in little endian formaat het hoogste (eerste) bit van de _laatste_ byte het teken van het getal bepaalt.

Ook bij `to_bytes` kunnen we de volgorde van de bytes opgeven, uiteraard weer nadat je het aantal bytes specificeert:

```python
>>> (258).to_bytes(2)
b'\x01\x02'
>>> (258).to_bytes(2, byteorder="little")
b'\x02\x01'
```

### Structs

Vaak krijg je van een IoT-sensor een hele reeks bytes die achtereenvolgens meerdere getalwaardes voorstellen, zoals een temperatuur, luchtvochtigheid, luchtdruk, batterijspanning enzovoort. Je kunt nu, bijvoorbeeld met _slicing_, die individuele stukjes uit de reeks bytes halen en een voor een met `from_bytes` naar getallen omzetten. Maar er is ook een andere manier, met de module `struct`. Hiermee definieer je een _format string_ van welke types er achtereenvolgens in die bytes zitten, en vraag je dan om de bytes naar die types om te zetten. In de andere richting, van types naar bytes, werkt dit ook.

#### Unpack

Stel dat je data er als volgt uitzien, in little endian formaat:

```
Bytes  Beschrijving
0-1    Temperatuur
2-3    Luchtvochtigheid
4-5    Luchtdruk
6      Batterijpercentage
```

Dan kijk je in de Python-documentatie van de module `struct` hoe je hiervoor een format string (<https://docs.python.org/3/library/struct.html#format-strings>) opstelt. Dit wordt: `"<hHHB"`. Hierbij staat de `<` voor little endian, de h voor een _short integer_ (twee bytes), de H voor een _unsigned short integer_ (twee bytes met alleen positieve getallen) en de B voor een _unsigned char_ (een getal van 0 tot en met 255).

Als je nu een reeks bytes inleest die dit formaat volgt, kun je de waardes als volgt decoderen:

```python
>>> import struct
>>> struct.unpack("<hHHB", b'\x04\x01\x82\x02\xf6\x03c')
(260, 642, 1014, 99)
```

Het resultaat is een _tuple_ (wat je aan de haakjes rond het resultaat ziet). Je kunt de componenten ervan ook rechtstreeks aan variabelen toekennen:

```python
>>> temperature, humidity, pressure, battery = struct.unpack("<hHHB", b'\x04\x01\x82\x02\xf6\x03c')
>>> temperature
260
```

#### Pack

Dit werkt ook in de andere richting. Stel dat je wat waardes hebt en deze in een gegeven formaat binair wilt opslaan. Dan gebruik je diezelfde format string, maar gebruik je de functie `pack` waaraan je achter de format string de verschillende waardes doorgeeft:

```python
>>> struct.pack("<hHHB", 260, 642, 1014, 99)
b'\x04\x01\x82\x02\xf6\x03c'
```

Bekijk zeker de documentatie van struct voor nog meer mogelijkheden. Merk op dat struct ervan uitgaat dat alle onderdelen van de reeks bytes dezelfde bytevolgorde gebruiken, dus allemaal big endian of allemaal little endian. Doorgaans is dit inderdaad het geval, maar in andere gevallen zul je de reeks niet in één keer kunnen decoderen.
