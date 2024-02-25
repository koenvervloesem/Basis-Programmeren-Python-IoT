## GUI's met TkInter

We zijn van start gegaan met het schrijven van programma's in de console: CLI (Command Line Interface). Met behulp van de functies `input()` en `print()` waren we in staat om onze toepassingen wat **interactief** te maken.

Met behulp van TkInter gaan we nu grafische toepassingen of GUI's maken (Graphical User Interface).

### Leeg scherm

We starten met een eenvoudige toepassing: 

~~~python
from tkinter import Tk

window = Tk()
window.mainloop()
~~~

Je dient hiervoor de module `tkinter` te importeren. Vervolgens maak je een `Tk`-object, dat het venster van ons programma voorstelt.

Als laatste regel van deze code roep je de methode `mainloop()`van het `Tk`-object aan.  
Deze zal de grafische interface starten en in de achtergrond een oneindige lus draaien, die er voor zorgt dat het programma actief blijft.

Als resultaat krijg je dan een lege toepassing:

![](tkinter_empty.png)

Deze kun je minimaliseren, maximaliseren en sluiten (via het kruisje) zoals een standaard grafische toepassing.

> Let wel, het uitzicht van de toepassing kan licht variëren afhankelijk van het besturingssysteem.

### Leeg scherm met titel

Je voegt eenvoudig een zelfgekozen titel toe aan de titelbalk:

~~~python
from tkinter import Tk

window = Tk()
window.title("Let's make some gui")
window.mainloop()
~~~

![](tkinter_title.png)

### Label

De GUI die TkInter toont, bestaat uit twee onderdelen:

* De **titelbalk** (titel en acties)
* Het **canvas** waarop je **grafische elementen** of **widgets** kunt plaatsen

Een eerste element dat je op het canvas kunt plaatsen is een **label**. Dat is een stuk tekst dat je programmatorisch kunt manipuleren:

~~~python
from tkinter import Tk
from tkinter.ttk import Label

# Creëer venster
window = Tk()
window.title("Let's make some gui")

# Creëer label
lbl = Label(window, text="Hello gui")
lbl.pack()

# Start main loop
window.mainloop()
~~~

Merk op dat we nu naast `Tk` uit de module `tkinter` nu ook de klasse `Label` uit de module `tkinter.ttk` importeren.

We maken dus een label aan, dat we aan het venster toekennen en dat we een specifieke tekst geven die het label moet tonen. Daarna voeren we de methode `pack()` op dit label uit. Hiermee wordt het label in het venster geplaatst, standaard bovenaan.

Het resultaat ziet er als volgt uit:

![](tkinter_label.png)

### button

Een meer **interactieve widget** die je op een canvas kunt zetten, is een **button**:

~~~python
import sys
from tkinter import Tk
from tkinter.ttk import Button, Label

# Creëer venster
window = Tk()
window.title("Let's make some gui")

# Creëer label
lbl = Label(window, text="Hello gui")
lbl.pack()

# Creëer knop
button = Button(window, text="Goodbye", command=sys.exit)
button.pack()

# Start main loop
window.mainloop()
~~~

Het resultaat ziet er als volgt uit:

![](tkinter_button.png)

Als je nu op de knop klikt, wordt de functie aangeroepen die je als argument `command` bij het aanmaken van het `Button`-object hebt opgegeven. Dat is in dit geval `sys.exit`, waarmee je het Python-programma afsluit.

Overigens kun je met het argument `side="left"` of `side="right"` bij het oproepen van de methode `pack` aangeven of je widget op een andere plaats moet komen te staan dan de standaardpositie bovenaan. Probeer hiermee eens het label links en de knop rechts te zetten.

### Lettertype, lettergrootte en kleuren aanpassen

Voor elk van de widgets kun je het lettertype, de lettergrootte (in pixels) en de kleur van de letters of de achtergrond instellen:

~~~python
import sys
from tkinter import Tk
from tkinter.ttk import Button, Label, Style

# Creëer venster en stijl
window = Tk()
window.title("Let's make some gui")
style = Style()

# Creëer label
style.configure("TLabel", font=("Arial Bold", 40))
lbl = Label(window, text="Hello gui")
lbl.pack()

# Creëer knop
style.configure("TButton", foreground="white", background="black")
button = Button(window, text="Goodbye", command=sys.exit)
button.pack()

# Start main loop
window.mainloop()
~~~

Het uiterlijk van widgets aanpassen doen we door een `Style`-object aan te maken en daarmee stijlen te configureren. Zo geven we met `style.configure("TLabel", font=("Arial Bold", 40))` aan dat alle TkInter-widgets van het type `Label` als lettertype "Arial Bold" met een grootte van 40 pixels gebruiken. Het label dat we daarna aanmaken, krijgt dus automatisch deze stijl.

Hetzelfde doen we daarna met de stijl voor het `Button`-object. We definiëren die stijl met `style.configure("TButton", foreground="white", background="black")`.

Dat ziet er dan als volgt uit:

![](tkinter_font.png)

Let op: op Windows dien je een alternatieve stijl in te stellen, omdat de standaardstijl niet de achtergrondkleur van knoppen instelt maar alleen het kader errond. Voeg dan de volgende regel toe na `style = Style()`:

```
style.theme_use("alt")
```

### Grootte van het venster aanpassen

Je kunt ook de grootte van het volledige venster instellen, bijvoorbeeld 500 pixels breed en 300 pixels hoog:

~~~python
import sys
from tkinter import Tk
from tkinter.ttk import Button, Label, Style

# Creëer venster en stijl
window = Tk()
window.title("Let's make some gui")
window.geometry("500x300")
style = Style()

# Creëer label
style.configure("TLabel", font=("Arial Bold", 40))
lbl = Label(window, text="Hello gui")
lbl.pack()

# Creëer knop
style.configure("TButton", foreground="white", background="black")
button = Button(window, text="Goodbye", command=sys.exit)
button.pack()

# Start main loop
window.mainloop()
~~~

Dan krijg je dit te zien:

![](tkinter_sized.png)

### Een actie aan een knop koppelen (1)

We hebben hiervoor al gezien hoe je een actie aan een knop koppelt. We koppelden namelijk de functie `sys.exit` aan de knop. Hetzelfde kun je doen met functies die je zelf definieert:

~~~python
import sys
from tkinter import Tk
from tkinter.ttk import Button, Label, Style

def clicked():
    lbl.configure(text="Button was clicked !!")

# Creëer venster en stijl
window = Tk()
window.title("Let's make some gui")
window.geometry("500x300")
style = Style()

# Creëer label
style.configure("TLabel", font=("Arial Bold", 40))
lbl = Label(window, text="Hello gui")
lbl.pack()

# Creëer knoppen
style.configure("Inverted.TButton", foreground="white", background="black")
button = Button(window, text="Goodbye", style="Inverted.TButton", command=sys.exit)
button.pack()
message_button = Button(window, text="Message", command=clicked)
message_button.pack()

# Start main loop
window.mainloop()
~~~

Merk op dat we hier onze functie `clicked` doorgeven als waarde voor het argument `command` bij het aanmaken van het tweede `Button`-object.

Een ander verschil is dat we nu twee knoppen hebben, die we elk een andere stijl willen geven. De `message_button` willen we de standaardstijl geven, terwijl we `button` de geïnverteerde stijl willen geven met witte letters op zwarte achtergrond.

Dat doen we hier door aan de methode `style.configure` een zelfgekozen naam door te geven, "Inverted.TButton". Aan de constructor van de knop die deze stijl moet krijgen, geven we dan als argument `style="Inverted.TButton"` door. Aan de andere knop geven we geen specifieke stijl door, waardoor die de (ongewijzigde) standaardstijl voor knoppen krijgt.

Voer je dit programma uit, dan krijg je initieel het volgende te zien:

![](tkinter_button2.png)

Klik je nu op de knop met de tekst **Message**, dan wordt de functie `clicked` uitgevoerd. Omdat die de tekst van het label bovenaan aanpast, wijzigt de tekst "Hello gui" daar nu in "Button was clicked !!":

![](tkinter_button2_after.png)

### Een actie koppelen aan aan een button (2)

Een ander widget van TkInter is `Entry`. Hiermee kun je tekstinvoer van een gebruiker opvragen. En die tekstinvoer kunnen we dan na een klik op een knop ook elders weergeven:

~~~python
import sys
from tkinter import Tk
from tkinter.ttk import Button, Entry, Label, Style

def clicked():
    lbl.configure(text="Message: " + txt.get())

# Creëer venster en stijl
window = Tk()
window.title("Let's make some gui")
window.geometry("500x300")
style = Style()

# Creëer label
style.configure("TLabel", font=("Arial Bold", 40))
lbl = Label(window, text="Hello gui")
lbl.pack()

# Creëer knoppen
style.configure("Inverted.TButton", foreground="white", background="black")
button = Button(window, text="Goodbye", style="Inverted.TButton", command=sys.exit)
button.pack()
message_button = Button(window, text="Message", command=clicked)
message_button.pack()

# Creëer tekstveld
txt = Entry(window, width=10)
txt.pack()

# Start main loop
window.mainloop()
~~~

Initieel krijg je dit te zien:

![](tkinter_button3.png)

Vul je nu "Test" in het tekstveld in en klik je dan op de onderste knop, dan verschijnt je tekst "Test" in het label bovenaan:

![](tkinter_button3_after.png)

Dit gebeurt dankzij de functie `clicked`. Die wordt uitgevoerd wanneer je op de onderste knop klikt. De functie haalt de waarde uit het tekstveld met `txt.get()` en kent die toe aan de tekst van het label.

### Uitklaplijst

Een laatste widget in TkInter dat we bekijken, is de `Combobox`: een uitklaplijst met waardes waaruit je kunt kiezen. We kunnen onze vorige code dus aanpassen zodat je geen vrije tekst invult, maar uit een lijst van opties kiest wat er in het label komt te staan:

~~~python
import sys
from tkinter import Tk
from tkinter.ttk import Button, Combobox, Label, Style

def clicked():
    lbl.configure(text="Message: " + combo.get())

# Creëer venster en stijl
window = Tk()
window.title("Let's make some gui")
window.geometry("500x300")
style = Style()

# Creëer label
style.configure("TLabel", font=("Arial Bold", 40))
lbl = Label(window, text="Hello gui")
lbl.pack()

# Creëer knoppen
style.configure("Inverted.TButton", foreground="white", background="black")
button = Button(window, text="Goodbye", style="Inverted.TButton", command=sys.exit)
button.pack()
message_button = Button(window, text="Message", command=clicked)
message_button.pack()

# Creëer uitklaplijst
combo = Combobox(window)
combo["values"] = (1, 2, 3, 4, 5, "Text")
combo.current(1)  # Selecteer standaard het element op positie 1
combo.pack()

# Start main loop
window.mainloop()
~~~

Dit toont het volgende venster, met de waarde 2 standaard geselecteerd in de uitklaplijst, omdat deze op positie 1 (te beginnen vanaf 0) staat:

![](tkinter_combo.png)

Selecteer je nu in de uitklaplijst bijvoorbeeld de waarde 5 en klik je dan op de onderste knop, dan zie je dit:

![](tkinter_combo_after.png)

### Meer informatie

Meer informatie over TkInter vind je in de documentatie van het pakket:

* <https://docs.python.org/3/library/tkinter.html>
* <https://docs.python.org/3/library/tkinter.ttk.html>
