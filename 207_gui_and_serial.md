## Led aansturen via een seriële interface

In dit laatste stuk van deel 2 van de cursus gaan we een stukje **physical computing** doen. We maken een grafische interface in TkInter voor een programma dat via de USB-poort opdrachten geeft aan een microcontrollerbordje. Dit bordje luistert naar de opdrachten die via de USB-poort binnenkomen en reageert daarop door de ingebouwde LED in en uit te schakelen.

We hebben twee soorten microcontrollerbordjes voorbereid: enkele met CircuitPython en enkele Arduino-compatibele bordjes.

### CircuitPython-bordjes

CircuitPython (<https://circuitpython.org>) is een afgeslankte versie van de programmeertaal Python, zodat je er microcontrollers mee kunt programmeren. CircuitPython ondersteunt honderden microcontrollerbordjes (<https://circuitpython.org/downloads>), maar voor onze code hebben we een bordje nodig dat seriële communicatie via USB CDC (<https://docs.circuitpython.org/en/latest/shared-bindings/usb_cdc/index.html>) ondersteunt. Dat zijn bijvoorbeeld:

* Arduino Nano RP2040 Connect (microUSB)
* Raspberry Pi Pico (microUSB)
* Raspberry Pi Pico W (microUSB)
* Seeed Studio XIAO SAMD21 (USB-C)
* Seeed Studio XIAO nRF52840 (USB-C)

Na de installatie van de CircuitPython-firmware kun je je eigen CircuitPython-code eenvoudig op het bordje kopiëren. Onze code bestaat uit twee bestanden: **boot.py** wordt bij het opstarten uitgevoerd voor vroege initialisatie, en **code.py** wordt daarna uitgevoerd.

We zetten het volgende in **boot.py**:

```python
import usb_cdc

# Enable console-over-serial and data-over-serial
usb_cdc.enable(console=True, data=True)
```

Hiermee creëren we een serieel apparaat om data door te sturen. Dit verschijnt dan als COM-poort in Windows of als serieel tty-apparaat in Linux of macOS.

In **code.py** staat het volgende:

```python
from time import sleep
from board import LED
from digitalio import DigitalInOut, Direction
import usb_cdc

led = DigitalInOut(LED)
led.direction = Direction.OUTPUT

if __name__ == "__main__":
    # Get the USB data feed object
    serial = usb_cdc.data

    while True:
        # Check for incoming data
        if serial.in_waiting > 0:
            command = serial.read(1)

            # Process command
            if command == b"0":
                led.value = False
            elif command == b"1":
                led.value = True
            elif command == b"2":
                led.value = not led.value
            elif command == b"3":
                led.value = not led.value
                sleep(0.020)
                led.value = not led.value

            # Return state of led
            if led.value:
                serial.write(b"1")
            else:
                serial.write(b"0")
```

Met de Python-kennis die je in deze cursus hebt opgedaan, zou dit eenvoudig te volgen moeten zijn. We importeren eerst enkele functies en klassen die we nodig hebben. Daarna maken we een object `led` van de klasse `DigitalInOut` aan, met als argument de constante `LED` die naar het pin-nummer van de ingebouwde led verwijst. Daarna vragen we het object `usb_cdc.data` aan, dat we gebruiken voor de seriële communicatie.

De rest van de code bestaat uit een oneindige lus, die continu wacht op binnenkomende seriële data en deze byte per byte leest. Komt er een teken "0" binnen, dan wordt de led uitgeschakeld, komt er een teken "1" binnen, dan wordt de led ingeschakeld, bij "2" verandert de led van status en bij "3" knippert de led eenmalig. Daarna schrijft de code de huidige status van de led naar de seriële interface.

### Arduino-code voor het Digispark-ontwikkelbordje

Een ander microcontrollerbordje is de Digispark, dat goedkoop is (ca. € 2,5), klein (2 x 2 cm) en eenvoudig zonder kabel in een vrije usb-poort van je computer past. Het is gebaseerd op de ATtiny85 AVR-microcontroller. De Digispark is ondersteund in de Arduino IDE. Volg daarvoor de instructies op <http://digistump.com/wiki/digispark/tutorials/connecting>. Een nadeel is dat er geen ondersteunde drivers voor nieuwe Windows-versies van bestaan.

We draaien op de Digispark de volgende code:

~~~c
#include <DigiCDC.h>

#define LED_PIN 1  // LED on Model A or Pro. Use 0 for Model B.
#define COMMAND_OFF '0'
#define COMMAND_ON '1'
#define COMMAND_TOGGLE '2'
#define COMMAND_FLASH '3'

// Hold the current LED state, initially off
int led_state = LOW;

// Initial setup of the device.
void setup() {                
    // Initialize USB as a serial interface.
    SerialUSB.begin(); 
    // Initialize the digital pin as an output.
    pinMode(LED_PIN, OUTPUT);
}

// The loop function runs over and over again forever.
void loop() {
  
    // Check for input on the serial terminal.
    if (SerialUSB.available()) {
        char input = SerialUSB.read();
        if(input == COMMAND_OFF) {
            // Turn off LED when receiving 0.
            led_state = LOW;
            digitalWrite(LED_PIN, led_state);
            SerialUSB.write(COMMAND_OFF);
        } else if(input == COMMAND_ON) {
            // Turn on LED when receiving 1.
            led_state = HIGH;
            digitalWrite(LED_PIN, led_state);
            SerialUSB.write(COMMAND_ON);
        } else if(input == COMMAND_TOGGLE) {
            // Toggle LED when receiving 2.
            led_state = !led_state;
            digitalWrite(LED_PIN, led_state);
            SerialUSB.write(led_state ? COMMAND_ON : COMMAND_OFF);
        } else if(input == COMMAND_FLASH) {
            // Flash LED when receiving 3.
            led_state = !led_state;
            digitalWrite(LED_PIN, led_state);
            SerialUSB.delay(20);
            led_state = !led_state;
            digitalWrite(LED_PIN, led_state);            
            SerialUSB.write(led_state ? COMMAND_ON : COMMAND_OFF);
        }
    }
  
    // Keep USB alive
    SerialUSB.refresh();
}
~~~

Dit is C++-code, en dit valt buiten het bestek van de cursus 'Basis programmeren', omdat C++ een andere taal is dan Python. Later in de cursus 'Embedded programmeren' leren jullie C-code, wat een gelijkaardige taal is. Maar we leggen hier kort even uit wat de code doet.

In de functie `setup()` initialiseren we de USB-poort als een seriële interface. We stellen ook `LED_PIN` (eerder in de code gedefinieerd als pin 1) als uitvoer in. Dat betekent dat we er later een waarde 0 of 1 naar kunnen schrijven om wel of geen spanning over de pin te zetten.

De functie `loop()` wordt keer op keer uitgevoerd zolang de microcontroller draait. Hierin controleren we of er invoer op de seriële terminal klaar staat. Zo ja, dan lezen we één teken in, en kijken we of het gelijk is aan het cijfer 0. Zo ja, dan schrijven we de waarde `LOW` naar de ledpin, waardoor die pin met GND wordt verbonden en er dus geen stroom door loopt. De led gaat uit. Is de invoer gelijk aan het cijfer 1, dan schrijven we de waarde HIGH naar de ledpin, waardoor die pin met 5 V van de USB-poort wordt verbonden en er dus stroom door loopt. De led gaat aan. Verder laat de microcontroller de led omschakelen van aan naar uit of andersom bij het inlezen van het cijfer 2, en hij laat de led kort flitsen als hij het cijfer 3 inleest. Leest de microcontroller een andere waarde in, dan doet hij niets. Na elke opdracht die je geeft, stuurt de code ook de huidige toestand van de led terug over de seriële interface.

### Python-code

Als je je microcontrollerbordje op een USB-poort van je computer aansluit, dan blijft het wachten op opdrachten van je computer.

De Python-code om de microcontroller aan te sturen, maakt niet alleen gebruik van TkInter voor de grafische interface, maar ook van de externe Python-bibliotheek pySerial (<https://pyserial.readthedocs.io>) voor de communicatie over de seriële interface. Installeer pySerial met:

~~~
pip install pyserial
~~~

Onze Python-code ziet er dan als volgt uit:

~~~python
from tkinter import Tk
from tkinter.ttk import Button, Combobox, Label

from serial import EIGHTBITS, PARITY_NONE, STOPBITS_ONE, Serial
from serial.tools.list_ports import comports


class SerialPortManager:
    """Klasse om een seriële poort te beheren.

    Deze klasse toont een uitklaplijst met beschikbare seriële poorten en een
    knop om met de gekozen seriële poort te verbinden. De klasse laat ook toe
    om naar de seriële poort opdrachten te schrijven.
    """

    def __init__(self, master):
        # De combobox toont alle beschikbare seriële interfaces.
        self.combobox = Combobox(master)
        self.combobox["values"] = [
            f"{port.device} ({port.manufacturer})" for port in comports()
        ]
        self.combobox["state"] = "readonly"
        self.combobox.pack()

        # De knop verbindt met de gekozen seriële interface.
        self.button = Button(master, text="Connect", command=self.connect)
        self.button.pack()

    def connect(self):
        """Verbind met een seriële interface."""
        self.connection = Serial(
            port=self.combobox.get().split(" ")[0],  # Haal poort uit combobox
            baudrate=9600,
            bytesize=EIGHTBITS,
            parity=PARITY_NONE,
            stopbits=STOPBITS_ONE,
            timeout=1,
            xonxoff=0,
            rtscts=0,
        )

    def write_command(self, command):
        """Schrijf een opdracht naar de seriële interface.

        Dit geeft de toestand die de microcontroller terugstuurt terug.
        """
        # Zet de string met encode om naar de overeenkomstige bytes.
        self.connection.write(command.encode())

        # Lees een byte terug en zet die met decode om naar een string.
        return self.connection.read().decode()


class Led:
    """Klasse om een led aan te sturen.

    De klasse toont twee knoppen ON en OFF en stuurt bij een druk op een knop
    de juiste opdracht naar de opgegeven seriële verbinding. Een label toont
    de huidige toestand van de led.
    """

    def __init__(self, master, serial):
        self.serial = serial

        # Het label toont de huidige toestand van de led, initieel onbekend.
        self.label = Label(master, text="LED is unknown")
        self.label.pack()

        # Met de knoppen schakel je de led aan of uit.
        Button(master, text="OFF", command=self.turn_off).pack()
        Button(master, text="ON", command=self.turn_on).pack()

    def turn_off(self):
        """Schakel de led uit."""
        self.update_label(self.serial.write_command("0"))

    def turn_on(self):
        """Schakel de led aan."""
        self.update_label(self.serial.write_command("1"))

    def update_label(self, value):
        """Update het label met de toestand van de led."""
        if value == "0":
            self.label.configure(text="LED is OFF")
        elif value == "1":
            self.label.configure(text="LED is ON")


# Creëer venster
window = Tk()
window.title("LED")
window.geometry("350x350")

# Creëer widgets voor poortbeheerder en led
port_manager = SerialPortManager(window)
led = Led(window, port_manager)

# Start main loop
window.mainloop()
~~~

Wat gebeurt er hier allemaal? In de klasse `SerialPortManager` creëren we in de constructor een combobox. De waardes hierin zijn alle poorten die pySerial vindt met de functie `comports()`. We maken hier gebruik van een **list comprehension**. De functie `comports()` geeft namelijk een lijst terug met objecten, waarvan we een lijst met strings maken: `[f"{port.device} ({port.manufacturer})" for port in comports()]`.

We maken in de constructor van `SerialPortManager` ook een knop aan. Als je hierop klikt, roept die de methode `connect` van de klasse aan. Hierin maken we een `Serial`-object van pySerial aan met als poort de poort die je gekozen hebt in de combobox, en enkele andere verbindingsparameters. Daarna kun je met de methode `write_command` van de klasse `SerialPortManager` een opdracht naar de seriële interface schrijven. De code leest daarna ook een byte terug, want de microcontroller stuurt een byte met de nieuwe toestand van de led terug. Die waarde zetten we met `decode` naar een string om en geven we terug.

Daarna definiëren we een klasse `Led`. Die toont een label met de huidige toestand van de led en knoppen om de led in en uit te schakelen. Die knoppen koppelen we aan methodes om de juiste opdracht naar de `SerialPortManager` te sturen, en we updaten hierin ook het label om de toestand van de led te tonen.

Uiteindelijk creëren we dan een venster en widgets voor de poortbeheerder en led, waarna we de main loop starten.

### Aan de slag

Sluit het microcontrollerbordje op een USB-poort van je computer aan en wacht enkele seconden tot het opgestart is. Start dan je Python-code op je computer op. Kies een poort uit de uitklaplijst (voor de CircuitPython-bordjes is het normaal de poort met het hoogste nummer) en klik op **Connect**. Klik daarna op **ON**. De led op het microcontrollerbordje gaat aan. Klik je op **OFF**, dan gaat de led uit.

### Opdracht

Breid deze code nu uit:

* Voeg een knop **TOGGLE** toe die de led op de het microcontrollerbordje laat omschakelen tussen aan en uit door de opdracht "2" naar de seriële interface te schrijven.
* Voeg een knop **FLASH** toe die de led op het microcontrollerbordje laat flitsen door de opdracht "3" naar de seriële interface te schrijven.
* Momenteel zoekt het programma in het begin naar de seriële poorten die het vindt en voegt die toe aan de uitklaplijst. Als je tijdens de werking van het programma een microcontrollerbordje aansluit, wordt die poort niet gevonden. Breid de klasse `SerialPortManager` uit met een knop die een methode `refresh` van de klasse aanroept waarmee je de combobox nieuwe waardes geeft.
* Er gebeurt geen foutenafhandeling in de code. Als je bijvoorbeeld op **Connect** klikt wanneer je nog geen poort gekozen hebt, of op **ON** of **OFF** klikt wanneer je nog niet met een seriële interface verbonden bent, krijg je in de console waarin je het programma opgestart hebt een exception te zien. Vang die op met try-except en toon dan een foutmelding in een label.
* Voeg een tekstveld toe om de baudrate (bitsnelheid) te kiezen waarmee je de seriële verbinding opzet.
* Toon de toestand van de seriële verbinding, bijvoorbeeld met een label dat **Connected** of **Not connected** toont, of met een ander widget naar keuze.
* Zoek eens uit hoe je de interface wat mooier kan maken, bijvoorbeeld door sommige widgets naast in plaats van onder elkaar te zetten.
