## Voorbereiding en installatie

Alvorens te kunnen starten, heb je op je computer twee zaken nodig:

* **Python-interpreter** (Python 3)
* **Teksteditor**

### Leren op de opdrachtregel te werken

Je hebt een minimale kennis nodig van het werken met een **opdrachtregel** (command line) op je besturingssysteem.  

* Op **Linux/macOS/Unix** is de meest gebruikte opdrachtregelomgeving **Bash**
* Op **Windows** heb je keuze tussen de klassieke **CMD** (Opdrachtprompt) of het iets uitgebreidere **PowerShell**

Als je jezelf **niet comfortabel** voelt op de opdrachtregel of er nog **nooit van gehoord** hebt, lees dan de **annex** voor een introductie voor zowel Linux als Windows.

### Installatie van Python

Eerst installeren we de **Python-interpreter**. Dit is de software waarmee je Python-programma's uitvoert.

#### Python 3.10

Voor deze cursus gebruiken we als referentie versie **Python 3.10**.  
Als je een eerdere versie van Python 3 hebt, is dat ook OK.  

> Nota:  
> Gebruik geen Python 2. Dit is niet meer ondersteund.

#### Installatie testen

Alvorens je Python gaat installeren, kijk je het best na of je systeem al een **Python-installatie** bevat en **welke versie** dat is.  
Dat kan je doen via de opdrachtregel:

Op Linux/macOS/Unix:

~~~bash_terminal
koan@x1:~$ python --version
Python 3.10.4
koan@x1:~$
~~~

Op Windows:

~~~
C:\users\py> python --version
Python 3.10.4
C:\users\py>
~~~

Afhankelijk van de installatie kan het zijn dat je op Windows **py** moet typen in plaats van **python**.

> Nota:  
> Op deze manier kun je na de installatie ook testen of de installatie correct gebeurd is.

Windows opent mogelijk de Microsoft Store om Python daaruit te installeren wanneer je bovenstaande opdracht intypt en je nog geen Python geïnstalleerd hebt.

#### Installeren op Windows

Er zijn twee manieren om Python te installeren onder Windows:

* Ga naar https://www.python.org/downloads/ en installeer de nieuwste Python-versie. Daarna kun je Python uitvoeren op de opdrachtprompt met de opdracht `py`.
* Installeer Python vanuit de Microsoft Store. Daarna kun je Python uitvoeren door in het startmenu te zoeken op Python. 

#### Installeren op macOS

MacOS komt meestal met Python geïnstalleerd. Mocht dit niet het geval zijn of alleen Python 2 is geïnstalleerd, installeer dan de nieuwste Python-versie van https://www.python.org/downloads/.

#### Installeren op Linux

*Debian/Mint/Ubuntu:*

~~~bash
$ sudo apt install python3
~~~

Op sommige oudere versies van deze distributies heb je nog twee versies van Python, Python 2 en Python 3:

~~~bash
bart@bvomini:~/Projects/ucll_python$ python --version
Python 2.7.15+
bart@bvomini:~/Projects/ucll_python$ python3 --version
Python 3.6.8
...
~~~

Op deze systemen moet je Python 3 aanroepen als **python3**.

*Fedora/Red Hat:*

~~~bash
# dnf install python3
~~~

### Teksteditor

Om een programma te schrijven hebben we een goede "**teksteditor"** nodig.  
Dit is de **belangrijkste tool** voor een software-ontwikkelaar om code te kunnen schrijven.

> Nota:  
> Microsoft Word, LibreOffice Writer of Apple Pages zijn geen teksteditors maar tekstverwerkers.
> Deze kun je niet gebruiken om Python-code in te schrijven.

Een aantal teksteditors zijn **vrij beschikbaar**:

* Veel Windows-gebruikers zweren bij **Notepad++**.
* Gebruikers van **macOS** kunnen terecht bij **TextMate**.
* Linux-distributies bieden doorgaans al standaard een krachtige teksteditor aan (**Gedit**, Xedit, Kate, Vi, ...).

Er zijn nog veel andere goede teksteditors op de markt, specifiek voor ontwikkelaars: **Visual Studio Code**, Sublime, Brackets, ...

> Nota: als je ervaring hebt met een IDE (integrated development environment) zoals
> Eclipse, PyCharm, ... mag je deze ook gebruiken.
