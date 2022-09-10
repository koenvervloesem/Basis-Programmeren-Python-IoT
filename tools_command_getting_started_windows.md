## Terminal op Windows (CMD of Opdrachtprompt)

Elk besturingssysteem bevat een CLI (command line interface). Windows heeft hiervoor het programma CMD (Opdrachtprompt), dat je kan vinden in het Windows-menu.

> De CMD-tool heeft zijn beperkingen. Voor een geavanceerdere omgeving op Windows is het interessant om Powershell te bekijken.
> Daarnaast bestaan er ook alternatieven zoals **Cygwin**, **MingW/MSYS** en **WSL (Windows Subsystem for Linux)**. Deze geven je een Bash-compatibele omgeving, zodat je dezelfde opdrachten als in Linux kan uitvoeren.

### Voorbeelden van Windows-opdrachten

We gaan nu een overzicht maken van de meest gebruikte opdrachten en principes in Windows. We doen dit aan de hand van een concreet voorbeeld.

### Windows-shell openen

Afhankelijk van de Windows-versie open je via het startmenu het programma CMD (Opdrachtprompt).  

Eenmaal gestart krijg je een scherm zoals hieronder:  

~~~
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Users\bart>
~~~

Deze **prompt** *C:\Users\bart>*:

* geeft aan in welk **pad** je je momenteel bevindt
* geeft je de mogelijkheid om een **opdracht** in te typen

### Bestanden en directory's

#### Een directory aanmaken

We starten met het aanmaken van een **directory** waarin we onze Python-code gaan plaatsen.

~~~
C:\Users\bart>mkdir een_eerste_programma

C:\Users\bart>dir
 Volume in drive C is System
 Volume Serial Number is E687-8D34

 Directory of C:\Users\bart

...
02/02/2017  14:08    <DIR>          een_eerste_programma
...
~~~

Hier zien we twee opdrachten:

* **mkdir** gevolg door het pad **een_eerste_programma**  
  Dit maakt een nieuwe map of directory aan met de opgegeven naam.  
* De opdracht **dir**  
  Dit toont de **inhoud** van de huidige **directory**.


#### Navigeren door directory's

Als je deze directory hebt aangemaakt, kan je hiernaartoe navigeren met de opdracht **cd** (wat staat voor *change directory*):

~~~
C:\Users\bart>cd een_eerste_programma

C:\Users\bart\een_eerste_programma>
~~~

Je navigeert dus naar de directory door **cd** in te typen gevolgd door het pad naar deze directory.

Merk op: de opdrachtprompt toont nu dat je je in de directory bevindt waarnaar je zojuist genavigeerd bent.

#### Relatieve versus absolute paden

Achter **mkdir** en **cd** geef je een **pad** op. Zo'n pad is de verwijzing naar een (doel)directory waarop je de opdracht wil uitvoeren.  

Er zijn een aantal manieren waarop je een pad kan construeren. Het belangrijkste onderscheid is dat tussen een absoluut of relatief pad:

* **absoluut** is een pad dat start vanaf de root-directory. In Windows start dit altijd vanaf een schijfletter (zoals C:).

~~~
C:\Users\>cd C:\Users\bart\een_eerste_programma>

C:\Users\bart\een_eerste_programma>49>
~~~

* **relatief** verwijst naar een locatie ten opzichte van je huidige directory.

~~~
C:\Users\bart>cd een_eerste_programma

C:\Users\bart\een_eerste_programma>cd ..

C:\Users\bart>cd ..\een_andere_directory

C:\Users\een_andere_directory>
~~~

Met het symbool **..** (twee puntjes na elkaar) verwijs je naar de bovenliggende directory.

#### Home-directory

Elke gebruiker in Windows heeft een home-directory (persoonlijke map). Windows voorziet een omgevingsvariabele (environment variabele) waarmee je naar deze directory kan navigeren:

~~~
C:\Users\bart>cd C:\

C:\Users> cd %HOMEPATH%

C:\Users\bart>
~~~

#### Directory's verwijderen

Een directory verwijderen doe je met de opdracht **rmdir**.
Als de directory nog bestanden bevat, zal de poging om ze te verwijderen een foutmelding opleveren.

~~~
C:\Users\bart>rmdir een_eerste_programma

C:\Users\bart>cd een_eerste_programma
The system cannot find the path specified.
~~~

Als je na het verwijderen naar deze directory probeert te navigeren, krijg je de foutmelding dat de directory niet bestaat.

#### Bestanden in een directory

We maken opnieuw een directory aan en navigeren naar deze directory.

~~~
C:\Users\bart>mkdir mijn_eerste_programma

C:\Users\bart>cd mijn_eerste_programma

C:\Users\bart\mijn_eerste_programma>dir
 Volume in drive C is System
 Volume Serial Number is E687-8D34

 Directory of C:\Users\bart\mijn_eerste_programma

02/02/2017  14:15    <DIR>          .
02/02/2017  14:15    <DIR>          ..
               0 File(s)              0 bytes
               2 Dir(s)  123.086.462.976 bytes free
~~~

Vervolgens starten we een **teksteditor** (bijvoorbeeld Notepad++) en typen we daar de volgende Python-opdracht in:

~~~python
print("Hello")
~~~

Bewaar dit bestand onder de eerder aangemaakte directory met als bestandsnaam **hello.py**.
Bekijk nadien met de opdracht **DIR** of het bestand correct aangemaakt is.

~~~
C:\Users\bart\mijn_eerste_programma>dir
 Volume in drive C is System
 Volume Serial Number is E687-8D34

 Directory of C:\Users\bart\mijn_eerste_programma

02/02/2017  14:25    <DIR>          .
02/02/2017  14:25    <DIR>          ..
02/02/2017  14:24                77 hello.py
               2 File(s)            471 bytes
               2 Dir(s)  123.097.833.472 bytes free
~~~

#### Inhoud van een bestand tonen op de opdrachtregel

Als je de inhoud van dit bestand alleen wil bekijken, kan dat eenvoudig op de opdrachtregel via de opdracht **type**:

~~~
C:\Users\bart\mijn_eerste_programma>type hello.py
print("Hello")
C:\Users\bart\mijn_eerste_programma>t
~~~

#### Een bestand kopiëren

Je kan ook een bestand kopiëren met de opdacht **copy**:

~~~
C:\Users\bart\mijn_eerste_programma>copy hello.py hello.txt
        1 file(s) copied.

C:\Users\bart\mijn_eerste_programma>dir
 Volume in drive C is System
 Volume Serial Number is E687-8D34

 Directory of C:\Users\bart\mijn_eerste_programma

02/02/2017  14:34    <DIR>          .
02/02/2017  14:34    <DIR>          ..
02/02/2017  14:24                77 hello.py
02/02/2017  14:24                77 hello.txt
               2 File(s)            154 bytes
               2 Dir(s)  123.095.646.208 bytes free
~~~

#### Een bestand verwijderen

Omdat we dit bestand voor het vervolg van deze cursus niet meer nodig hebben, verwijderen we het.
We gebruiken hiervoor de opdracht **del**, opnieuw gevolgd door het paad naar dit bestand.

~~~bat
C:\Users\bart\mijn_eerste_programma>del hello.txt

C:\Users\bart\mijn_eerste_programma>dir
 Volume in drive C is System
 Volume Serial Number is E687-8D34

 Directory of C:\Users\bart\mijn_eerste_programma

02/02/2017  14:35    <DIR>          .
02/02/2017  14:35    <DIR>          ..
02/02/2017  14:24                77 hello.py
               1 File(s)             77 bytes
               2 Dir(s)  123.094.589.440 bytes free

C:\Users\bart\mijn_eerste_programma>
~~~

> **Nota:**  
> Net zoals bij andere opdrachten kan je naar een bestand verwijzen met een relatief of een absoluut pad.

### Programma's uitvoeren

Je kunt ook programma's uitvoeren op de opdrachtregel.
In onderstaand voorbeeld voeren we via de Python-interpreter het bestand hello.py uit:

~~~bat
C:\Users\bart>cd mijn_eerste_programma

C:\Users\bart\mijn_eerste_programma>dir
 Volume in drive C is System
 Volume Serial Number is E687-8D34

 Directory of C:\Users\bart\mijn_eerste_programma

02/02/2017  14:35    <DIR>          .
02/02/2017  14:35    <DIR>          ..
02/02/2017  14:24                77 hello.py
               1 File(s)            77 bytes
               2 Dir(s)  123.094.589.440 bytes free

C:\Users\bart\mijn_eerste_programma>python hello.py
Hello
~~~

Je ziet nu op de opdrachtregel de uitvoer "Hello", omdat de Python-interpreter het Python-script hello.py met de opdracht print("Hello") heeft uitgevoerd.

### Omgevingsvariabelen

Een shell laat toe om - zoals in een programmmeertaal - variabelen aan te maken en te gebruiken.

### Een omgevingsvariabele definiëren

Een omgevingsvariabele is een variabele (eigenlijk een stuk tekst) die door de shell wordt bijgehouden gedurende de terminalsessie.  

Het volgende voorbeeld gebruikt dit mechanisme om het pad naar je project bij te houden:

~~~bat
C:\Users\bart\mijn_eerste_programma>set MIJN_PROJECT=C:\Users\bart\mijn_eerste_programma

C:\Users\bart\mijn_eerste_programma>echo %MIJN_PROJECT%
C:\Users\bart\mijn_eerste_programma

C:\Users\bart\mijn_eerste_programma>cd C:\

C:\> cd %MIJN_PROJECT%

C:\Users\bart\mijn_eerste_programma>
~~~

* Zo'n variabele geef je een waarde met de opdracht **set**, gevolgd door de **naam** van deze variabele, een isgelijkaanteken (**=**) en de waarde. Let op: er mag geen spatie tussen de naam van de variabele en het isgelijkaanteken komen!
* Je kan de inhoud van een omgevingsvariabele tonen met de opdracht **echo**, gevolgd door de naam omringd met **%**.
* Je kan de inhoud op dezelfde manier ook gebruiken in andere opdrachten, zoals **cd**. De shell vervangt de omgevingsvariabele door zijn inhoud voor deze aan de opdracht doorgegeven wordt.

> **Let op**: als een variabele al bestaat, overschrijft de set-opdracht de waarde die ze al had.

#### Systeemvariabelen

Naast je eigen omgevingsvariabelen die je zelf definieert, houdt Windows ook al een aantal omgevingsvariabelen bij. Een voorbeeld:

~~~bat
C:\Users\bart\mijn_eerste_programma>cd een_directory_die_niet_bestaat

C:\Users\bart\mijn_eerste_programma>echo %ERROR_LEVEL%
11
~~~

De variabele **ERROR_LEVEL** houdt de foutcode van de laatst uitgevoerde opdracht bij.

### Alle omgevingsvariabelen zien

Als je alle omgevingsvariabelen wil zien, typ dan gewoon **set** in:

~~~
C:\Users\bart\mijn_eerste_programma>set
ALLUSERSPROFILE=C:\Documents and Settings\All Users
APPDATA=C:\Documents and Settings\bart\Application Data
CLIENTNAME=Console
CommonProgramFiles=C:\Program Files\Common Files
COMPUTERNAME=V-HJLVYI8TKYLPA
ComSpec=C:\WINDOWS\system32\cmd.exe
FP_NO_HOST_CHECK=NO
HOMEDRIVE=C:
HOMEPATH=\Documents and Settings\bart
KMP_DUPLICATE_LIB_OK=TRUE
LOGONSERVER=\\V-HJLVYI8TKYLPA
MKL_SERIAL=YES
NIEXTCCOMPILERSUPP=C:\Program Files\National Instruments\Shared\ExternalCompi
Support\C\
NUMBER_OF_PROCESSORS=1
OS=Windows_NT
Path=C:\WINDOWS\system32;C:\WINDOWS;C:\WINDOWS\System32\Wbem
PATHEXT=.COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.py;.pyw
PROCESSOR_ARCHITECTURE=x86
PROCESSOR_IDENTIFIER=x86 Family 6 Model 42 Stepping 7, GenuineIntel
PROCESSOR_LEVEL=6
...
~~~
