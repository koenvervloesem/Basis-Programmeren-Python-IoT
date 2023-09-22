## Starten met Python

Na de installatie van Python kunnen we starten met ons eerste programma.
Alle voorbeelden voeren we op de opdrachtregel uit. Lees de annex als je daarbij nog verduidelijking nodig hebt.

### Python is een scriptingtaal

Een Python-script wordt door een speciaal programma - de **interpreter** - gelezen en uitgevoerd.
De code/instructies worden regel per regel **geïnterpreteerd** en **uitgevoerd** op de computer.
Deze instructies noemen we ook wel **statements**.

### Uitvoeren van Python-code

Python-code kan je uitvoeren op twee manieren:

* **Scripts:** Python-statements vanuit een bestand uitvoeren
* **Interactief:** Python-statements statement per statement uitvoeren in een shell

Voor de eerste voorbeelden werken we met scripts. Nadien wisselen we beide manieren af.

### Script "Hello world"

Na al deze voorbereiding volgt nu eindelijk het **langverwachte allereerste Python-programma**...

**Stap 1:** Open je favoriete **teksteditor** en typ de **volgende tekst** in:

~~~python
print("Hello world")
~~~

**Stap 2:** **Bewaar** dit op je computer in een bestand **hello.py**.
Bijvoorbeeld in je home-directory in een map "basis_programmeren"

> Nota: Zorg altijd dat je je Python-scripts gemakkelijk terugvindt. Maak dus een directory aan voor deze cursus en daarin bijvoorbeeld per les een nieuwe directory waarin je alle scripts van die les opslaat.

**Stap 3:** **Navigeer** op de **opdrachtregel** naar deze directory:

~~~
C:\users\python> cd basis_programmeren
C:\users\python\basis_programmeren> 
~~~

**Stap 4:** Voer dit programma uit.
Dit doe je door de **Python-interpreter** aan te roepen met als argument **hello.py** (naam van het Python-programma):

~~~
C:\users\python\basis_programmeren> python hello.py
Hello World
C:\users\python\basis_programmeren> 
~~~

> "Hello world" is de klassieker om de basiswerking van een programmeertaal te demonstreren.

### Wat hebben we zonet gedaan?

We hebben de **Python-interpreter** - het programma met de naam **python** dat verantwoordelijk is voor het uitvoeren van deze Python-scripts - aangeroepen met ons **tekstbestand** als **argument**.

Python-scripts eindigen altijd met de **bestandsextensie .py**.  
Dit is technisch gezien niet verplicht, maar er is geen reden om deze conventie te negeren. In het kader van de cursus is dit dus wel verplicht...

### Programmeertalen zijn gevoelig voor fouten

Een **programmeertaal is gevoelig voor fouten**, nog meer dan mensen.
Bekijk de volgende ongeldige Python-code bijvoorbeeld:

~~~python
print("Hello world"
~~~

Als je deze wil uitvoeren, zal de Python-interpreter het programma afbreken en aanduiden waar het in de mist gaat:

~~~
koan@x1:~$ python hello.py
  File "/home/koan/hello.py", line 1
    print("Hello world"
         ^
SyntaxError: '(' was never closed
~~~

De Python-interpreter gaat ook meestal naar goed vermogen proberen aan te duiden:

* **Waar het probleem** zich in de code bevindt.
    * Regelnummer code (hier **line 1**)
    * Positie binnen de regel (aangeduid met **^**)
* Welke **soort fout** je hebt gemaakt (hier een `SyntaxError`).
* Eventueel een **hint** wat je moet doen om de fout te corrigeren (hier het haakje sluiten).

Er zijn **verschillende soorten fouten** die in je in een Python-programma kan maken.  
Gedurende de cursus gaan we nog veel stilstaan bij die fouten en hoe je ermee moet omgaan.
