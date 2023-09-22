## Commentaar

Die ene regel in ons programma was een statement, een opdracht voor de Python-interpreter.
In Python-code kan je ook **tekst** toevoegen die **niet wordt geïnterpreteerd** en uitgevoerd door de interpreter.

### Commentaar op één regel

Voor een commentaar op één regel (*single-line comment*) volstaat het om tekst **vooraf** te laten gaan door een `#` of *hashtag* (hekje).

~~~python
# This is your very first application.
print("Hello world")  # You can insert comments also after your code.
# But please don't exaggerate with comments.
# Avoid writing more comments than code...
~~~

Alles wat achter de eerste hashtag komt op een regel wordt **niet geïnterpreteerd** door de Python-interpreter.

### Commentaar op meerdere regels

Als je commentaar op meerdere regels wilt schrijven, kun je elke regel laten voorafgaan door een hashtag. Maar er is ook een andere manier:

~~~python
"""
This is a comment over more
than one line.
Everything in between three subsequent double quotes
is considered part of the comment.
"""
# This is your very first application.
print("Hello world")  # You can insert comments also after your code.
# But please don't exaggerate with comments.
# Avoid writing more comments than code...
~~~

### Wanneer commentaar gebruiken?

Gebruik van **commentaar** ter documentatie van je code **kan nuttig zijn** en kan je **collega's** (of jezelf een tijdje nadat je de code hebt geschreven) **vooruithelpen**.

Gebruik zeker commentaar om:

* In het begin uit te leggen wat je Python-script doet.
* De verwachte invoer en uitvoer uit te leggen.
* Niet voor de hand liggende statements of blokken (zie later) code uit te leggen.
* Uit te leggen **waarom** je iets doet.

Maar zoals bij alle goede dingen in het leven geldt ook hier: **gebruik commentaar met mate**.

Gebruik zeker geen commentaar om:

* In je eigen woorden te herhalen wat een statement doet. Dat is overbodig. Leg het waarom uit, niet het wat.
* Nodeloos complexe code uit te leggen.

Te veel commentaar is dikwijls een teken van **slecht geschreven/leesbare code**. Als je zoveel commentaar nodig hebt om je code te begrijpen, kun je beter je code herschrijven tot iets wat gemakkelijker te begrijpen is - en daardoor minder commentaar nodig heeft.
