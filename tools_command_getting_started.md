## Werken met een shell

Eén van de eerste basisvaardigheden van een software-ontwikkelaar is het kunnen omgaan met een shell of command line (opdrachtregel).
Het kunnen omgaan met de command line is meestal noodzakelijk voor een build en deploy van je software:

* Starten en compilen van je software
* Flashen van de software op een microcontroller (of ontwikkelbordje)
* Automatisatie (bijvoorbeeld met één opdracht je software compileren en flashen)

Voor vele microcontrollers en bijbehorende ontwikkelomgevingen bestaan er grafische tools, maar deze voeren in de meeste gevallen op de achtergrond ook de command line tools uit.

### Wat is een shell?

Een shell is een programma waarmee je rechtstreeks toegang hebt tot **systeemoperaties** door opdrachten als tekst in te typen:

* Opstarten van programma's
* Navigeren door een bestandssysteem
* Manipuleren van bestanden en mappen
* Controleren en monitoren van processen
* Automatiseren van taken
* Communicatie over een seriële interface

### Tekstopdrachten ingeven

Deze tekstopdrachten kunnen meestal ook **gebundeld** worden in een **script**, dat je dan kan uitvoeren van een CLI (command line interface) net zoals je programma's kan uitvoeren.

Nadat zo'n opdracht/script/programma uitgevoerd is, krijgt de gebruiker weer de kans om de shell of het programma aan te spreken door op de opdrachtregel een nieuwe opdracht op te geven.  

### Waarom werken met een shell?

Elke software-ontwikkelaar moet de beginselen kennen van het werken met de opdrachtregel. Dat geldt zeker als je met **embedded devices** werkt, die doorgaans alleen via een opdrachtregel aan te sturen zijn. In andere programmeervakken ga je deze vaardigheden ook nodig hebben om met toolchains met een compiler en linker om te gaan.

### Vervolg...

We gaan nu dit bekijken voor 2 soorten shell-omgevingen

* Windows CMD
* Bash (GNU/Linux, macOS en andere POSIX-achtige systemen)
