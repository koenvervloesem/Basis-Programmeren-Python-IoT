## Extra oefeningen

### Oefening op exceptions

~~~python
"""
Volgend programma deelt twee getallen door elkaar
"""

"""
Vraag 1:
Zorg dat je het resultaat toont met een f-string
"""

"""
Vraag 2:
Vang de deling door nul op (met een try-except)
"""

"""
Vraag 3:
Volgende functie vraagt een getal op.
Het probleem is echter dat deze een ValueError-exception
zal opwerpen als de gebruiker geen getal invoert.

Wijzig deze code opdat deze het getal blijft
opvragen zolang dat de gebruiker geen geldig
integer invoer.
Je zal hiervoor een lus moeten combineren met
een try-except-statement.
"""

def get_number(message):
    input_user = input(message)
    try:
        return int(input_user)
    except ValueError:
        return 0

a = get_number("Geef een eerste nummer: ")
b = get_number("Geef een 2de nummer: ")

print(str(a) + " / " + str(b) + " = " + str(a / b))
~~~

### Car management

Implementeer een Python-programma dat voertuigen beheert. Je moet er iets als het volgende mee kunnen doen:

~~~
$ python carmanagement.py 
Welkom, gelieve keuze te maken
1. Voertuig toevoegen
2. Voertuigen printen
3. Afsluiten
> 1
Geef merk: Lada
Geef kleur: Geel
Geef brandstof 1 voor Diesel, 2 voor Benzine: 1
Geef kw: 50
Welkom, gelieve keuze te maken
1. Voertuig toevoegen
2. Voertuigen printen
3. Afsluiten
1
Geef merk: Ferrari
Geef kleur: Rood
Geef brandstof 1 voor Diesel, 2 voor Benzine: 1
Geef kw: 500
Welkom, gelieve keuze te maken
1. Voertuig toevoegen
2. Voertuigen printen
3. Afsluiten
2
Lada, kleur = Geel, brandstof = Benzine, KW = 50
Ferrari, kleur = Rood, brandstof = Benzine, KW = 500
~~~
