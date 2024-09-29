## Voorbereiding en installatie

Alvorens te kunnen starten, heb je op je computer twee zaken nodig:

* **Python-interpreter** (Python 3)
* **Teksteditor**

### Leren op de opdrachtregel te werken

Je hebt een minimale kennis nodig van het werken met een **opdrachtregel** (command line) op je besturingssysteem.  

* Op **Linux/macOS/Unix** is de meest gebruikte opdrachtregelomgeving **Bash**.
* Op **Windows** heb je keuze tussen de klassieke **CMD** (Opdrachtprompt) of het iets uitgebreidere **PowerShell**.

Als je jezelf **niet comfortabel** voelt op de opdrachtregel of er nog **nooit van gehoord** hebt, lees dan de **annex** voor een introductie voor zowel Linux als Windows.

### Installatie van Python

Eerst installeren we de **Python-interpreter**. Dit is de software waarmee je Python-programma's uitvoert.

#### Python 3.12

Voor deze cursus gebruiken we als referentie versie **Python 3.12**.
Als je een eerdere (of latere) versie van Python 3 hebt, is dat ook OK.

> **Nota:**
> Gebruik geen Python 2. Die versie is niet meer ondersteund.

#### Installatie testen

Alvorens je Python gaat installeren, kijk je het best na of je systeem al een **Python-installatie** bevat en **welke versie** dat is.  
Dat kun je doen via de opdrachtregel:

Op Linux/macOS:

~~~bash
koan@x1:~$ python --version
Python 3.12.3
koan@x1:~$
~~~

Op Windows:

~~~
C:\Users\Koen> python --version
Python 3.12.3
C:\Users\Koen>
~~~

> Nota:  
> Op deze manier kun je na de installatie ook testen of de installatie correct gebeurd is.

Windows opent mogelijk de Microsoft Store om Python daaruit te installeren wanneer je bovenstaande opdracht intypt en je nog geen Python geïnstalleerd hebt. Sluit de Microsoft Store en gebruik onderstaande methode.

#### Installeren op Windows

Installeer Python met Winget:

~~~
winget install --id Python.Python.3.12
~~~

#### Installeren op macOS

Installeer Homebrew (<https://brew.sh>) en installeer daarmee dan Python met:

~~~
brew install python3
~~~

#### Installeren op Linux

Onder Debian/Linux Mint/Ubuntu:

~~~bash
$ sudo apt install python3
~~~

Op sommige oudere versies van deze distributies heb je nog twee versies van Python, namelijk Python 2 en Python 3:

~~~bash
bart@bvomini:~/Projects/ucll_python$ python --version
Python 2.7.15+
bart@bvomini:~/Projects/ucll_python$ python3 --version
Python 3.6.8
...
~~~

Op deze systemen moet je Python 3 aanroepen als `python3`.

Onder Fedora/Red Hat:

~~~bash
$ sudo dnf install python3
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
