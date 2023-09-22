## Terminal op Linux en macOS (Bash)

Linux en macOS gebruiken beide Bash als shell.

We geven hier een overzicht van de meest gebruikte opdrachten en principes. We doen dit aan de hand van een concreet voorbeeld.

Voor meer informatie verwijzen we naar de cursus **Basis Linux**.

### Bash-shell openen

Linux en macOS hebben verschillende programma's waarmee je toegang tot de shell krijgt. Op de meeste systemen heet dat programma Terminal.

![](bash_terminal.png)

Hierin krijg je een scherm zoals hieronder:  

~~~bash
bart@bvpers4 ~ $
~~~

Deze **opdrachtprompt** :

* toont welke **gebruiker** ingelogd is (hier bart op de computer bvpers4)
* geeft aan in welk **pad** je je momenteel bevindt 
  (in dit geval komt ~ overeen met de home-directory van de gebruiker)
* geeft je de mogelijkheid om een **opdracht** in te typen na het dollarteken ($)

### Bestanden en directory's

#### Een directory aanmaken

We starten met het aanmaken van een **directory** waarin we onze Python-code gaan plaatsen.

~~~bash
bart@bvpers4 ~ $ mkdir een_eerste_programma
bart@bvpers4 ~ $ ls
... een_eerste_programma Documents Games ...
~~~

Hier zien we twee opdrachten:

* `mkdir` gevolg door het pad **een_eerste_programma**  
  Dit maakt een nieuwe map of directory met deze naam aan.  
* De opdracht `ls`  
  Hiermee krijgen we de **inhoud** van de huidige **directory** te zien.

#### Navigeren door directory's

Als je deze directory hebt aangemaakt, kan je hiernaartoe navigeren met de opdracht `cd`  
(wat staat voor *change directory*):

~~~bash
bart@bvpers4 ~ $ cd een_eerste_programma
bart@bvpers4 ~/een_eerste_programma $ pwd
/home/bart/een_eerste_programma
bart@bvpers4 ~/een_eerste_programma $
~~~

Je navigeert door `cd` te typen gevolgd door het pad naar de directory.  
De opdracht `pwd` (*print working directory*) toont je de huidige directory. 

#### Relatieve versus absolute paden

`mkdir` en `cd` roep je aan met een **pad**.    
Zo'n pad is de verwijzing naar een (doel)directory waarop je deze opdracht wil uitvoeren.  

Er zijn een aantal manieren waarop je een pad kan construeren. Het belangrijkste onderscheid is absoluut versus relatief:

* **absoluut** is een pad dat start vanaf de root-directory, wat onder Linux en macOS `/` is.

~~~bash
bart@bvpers4 ~ $ cd /home/bart/een_eerste_programma
bart@bvpers4 ~/een_eerste_programma $ pwd
/home/bart/een_eerste_programma
bart@bvpers4 ~/een_eerste_programma $
~~~

Een absoluut pad begint dus altijd met `/`.

* **relatief** verwijst naar een locatie relatief ten opzichte van je huidige directory

~~~bash
bart@bvpers4 ~ $ cd een_eerste_programma
bart@bvpers4 ~/een_eerste_programma $ pwd
/home/bart/een_eerste_programma
bart@bvpers4 ~/een_eerste_programma $ cd ../een_andere_directory
bart@bvpers4 ~/een_andere_directory $
~~~

Dit verwijst van je huidige directory naar een pad relatief ten opzichte van je huidige directory.  
Met het symbool `..` (twee punten na elkaar) verwijs je naar de bovenliggende directory. 

#### Home-directory

Elke gebruiker op Unix-achtige systemen, zoals GNU/Linux, macOS en de BSD's, heeft een home-directory. In Bash kan je daarnaar verwijzen met het teken `~`.  
Ook als je `cd` typt zonder een directorynaam erachter, kom je in de home-directory terecht.

~~~bash
bart@bvpers4 ~ $ cd ..
bart@bvpers4 /home $ cd
bart@bvpers4 ~ $ pwd
/home/bart
bart@bvpers4 ~ $ cd ..
bart@bvpers4 /home $ cd ~
bart@bvpers4 ~ $ pwd
/home/bart
bart@bvpers4 ~ $
~~~

#### Directory's verwijderen

Een directory verwijder je met de opdracht `rmdir`.
Als je een directory probeert te verwijderen terwijl die nog bestanden bevat, zal dit een foutmelding opleveren.

~~~bash
bart@bvpers4 ~ $ rmdir een_eerste_programma
bart@bvpers4 ~ $ cd een_eerste_programma
bash: cd: een_eerste_programma: No such file or directory
~~~

Als je na het verwijderen van een directory ernaartoe probeert te navigeren, krijg je een foutmelding dat deze directory niet bestaat.

#### Bestanden in een directory

We maken de directory opnieuw aan, navigeren erheen en bekijken de inhoud:

~~~bash
bart@bvpers4 ~ $ mkdir een_eerste_programma
bart@bvpers4 ~ $ cd mijn_eerste_programma/
bart@bvpers4 ~/mijn_eerste_programma $ ls
bart@bvpers4 ~/mijn_eerste_programma $
~~~

Vervolgens openen we een **teksteditor** (bijvoorbeeld gedit) en typen hierin de volgende Python-code:

~~~python
print("Hello")
~~~

Bewaar het bestand onder de eerder aangemaakte directory met de bestandsnaam **hello.py**.  
Bekijk nadien met de opdracht `ls` of het bestand correct is aangemaakt:

~~~bash
bart@bvpers4 ~/mijn_eerste_programma $ ls
hello.py
~~~

#### Inhoud van een bestand tonen

Stel dat je alleen de inhoud van het bestand wil zien, dan kan dat vanaf de opdrachtregel met de opdracht `cat`.

~~~bash
bart@bvpers4 ~/mijn_eerste_programma $ cat hello.py
print("Hello")
~~~

#### Een bestand kopiëren

Je kan ook een bestand via de terminal kopiëren met de opdracht `cp` (afkorting van *copy*):

~~~bash
bart@bvpers4 ~/mijn_eerste_programma $ cp hello.py hello.txt
bart@bvpers4 ~/mijn_eerste_programma $ ls
hello.py hello.txt
~~~

#### Een bestand verwijderen

Aangezien we het bestand niet nodig hebben voor het vervolg van onze cursus, verwijderen we het. Dat doen we met de opdracht `rm`, gevolgd door het pad naar het bestand:

~~~bash
bart@bvpers4 ~/mijn_eerste_programma $ ls
hello.py hello.txt
bart@bvpers4 ~/mijn_eerste_programma $ rm hello.txt
bart@bvpers4 ~/mijn_eerste_programma $ ls
hello.py
~~~

> **Nota:**  
> Net zoals bij andere opdrachten kan je naar een bestand verwijzen met zowel een relatief als een absoluut pad.

### Programma's uitvoeren

Je kan ook programma's uitvoeren op de opdrachtregel. Zo voert onderstaand voorbeeld de Python-interpreter uit met als argument het Python-bestand **hello.py**:

~~~bash
bart@bvpers4 ~/mijn_eerste_programma $ ls
hello.py
bart@bvpers4 ~/mijn_eerste_programma $ python hello.py
Hello
~~~

### Omgevingsvariabelen

Een shell laat toe om - zoals in een programmmeertaal - variabelen aan te maken en te gebruiken.

### Een omgevingsvariabele definiëren

Een omgevingsvariabele (*environment variable*) is een variabele (eigenlijk een stuk tekst) die door de shell wordt bijgehouden gedurende de terminalsessie.  

Het volgende voorbeeld gebruikt dit mechanisme om het pad naar je project bij te houden:

~~~
bart@bvpers4 ~ $ MIJN_PROJECT=/home/bart/mijn_eerste_programma
bart@bvpers4 ~ $ echo $MIJN_PROJECT
/home/bart/mijn_eerste_programma
bart@bvpers4 ~ $ cd $MIJN_PROJECT
bart@bvpers4 ~/mijn_eerste_programma $

~~~

* Een omgevingsvariabele geef je een waarde door de **naam** van deze variabele te verbinden via een `=`-teken met een tekst. Let op: er mogen geen spaties rond het isgelijkaanteken komen!
* Je kan de inhoud van de variabele tonen met de opdracht `echo`, gevolgd door een `$`-teken en de naam van de variabele.
* Je kan de inhoud op dezelfde manier ook gebruiken in andere opdrachten, zoals `cd`. De shell vervangt de omgevingsvariabele door zijn inhoud voor deze aan de opdracht doorgegeven wordt.

> **Let op**: als een variabele al bestaat wanneer je ze een waarde geeft, overschrijft dit de waarde die ze al had.

#### Systeemvariabelen

Naast je eigen omgevingsvariabelen die je zelf definieert, houdt je besturingssysteem ook een aantal variabelen bij. Een voorbeeld:

~~~bash
bart@bvpers4 ~ $ cd een_directory_die_niet_bestaat
bart@bvpers4 ~ $ echo $?
1
bart@bvpers4 ~ $ cd mijn_eerste_programma/
bart@bvpers4 ~/mijn_eerste_programma $ echo $?
0
~~~

Deze variabele `$?` houdt de exitcode bij die door het laatste programma aan de shell wordt teruggegeven. De exitcode is 0 als er geen problemen waren en is een foutcode in andere gevallen.  

### Alle omgevingsvariabelen zien

Als je alle omgevingsvariabelen wil zien, typ dan de opdracht `printenv` in:

~~~
bart@bvpers4 ~/mijn_eerste_programma $ printenv
LC_PAPER=de_BE.UTF-8
XDG_VTNR=8
SSH_AGENT_PID=2713
XDG_SESSION_ID=c1
LC_ADDRESS=de_BE.UTF-8
LC_MONETARY=de_BE.UTF-8
COMP_WORDBREAKS= 	
"'><;|&(:
QT_STYLE_OVERRIDE=gtk
GPG_AGENT_INFO=/home/bart/.gnupg/S.gpg-agent:0:1
TERM=xterm-256color
...
~~~
