## Expressies

### Een expressie geeft een waarde terug

We hebben al **statements** in Python gezien, die je kan beschouwen als een soort van **opdracht** of **actie** die Python uitvoert. We zagen als voorbeelden het tonen van een tekst (function call) of het initialiseren van een variabele (assigment statement).

Een ander belangrijk element in Python (en andere programmeertalen) zijn **expressies**. Een expressie is alles wat een **waarde** teruggeeft.  
Een eerste voorbeeld van expressie dat we al veel hebben gebruikt, zijn **literals** (of letterlijke waardes):

~~~python
a = "test"
b = 2
~~~

**"test"** en **2** zijn hier **literals** of  **letterlijke expressies** die een waarde geven die in een variabele wordt opgeslagen via de assignment operator = (en assignment statement).

Een andere expressie is bijvoorbeeld wanneer je de naam van een variabele als argument aan een functie doorgeeft:

~~~python
a = "test"
print(a)
~~~

Hier geldt a - op de tweede regel - als een expressie die de waarde van de variabele a ("test") oplevert.

### Rekenkundige expressies

Een andere soort expressie waar je normaal gezien vanuit de wiskunde al met vertrouwd bent, zijn **rekenkundige expressies**.

~~~python
print(4 / 2)  # 2
print(2 - 6)  # -4
print(5 * 2)  # 10 
print(5 % 2)  # 1
print(2 ** 8)  # 256
~~~

Het overzicht van deze **rekenkundige operatoren** vind je hieronder:

| Operator   |   Betekenis          |
|------------|----------------------|
| +          | optellen             |
| -          | aftrekken            |
| *          | vermenigvuldigen     |
| /          | deling               |
| //         | gehele deling        |
| %          | rest van deling      |
| **         | macht                |


De operatoren in Python komen grotendeels overeen met wat je gewoon bent uit de klassieke wiskunde en hebben geen verdere verklaring nodig, met uitzondering misschien van de deling.

### Delen van integers

Voor het **delen** van getallen in Python heb je **twee operatoren**:

* **//** ofwel **floor division operator** rondt af naar beneden tot een geheel getal.
* **/** ofwel **true division operator** rondt niet af maar geeft de werkelijke decimale waarde.

Bij het delen van twee gehele getallen zie je het verschil duidelijk:

~~~
print(5/2)  # 2.5
print(type(5/2))  # <class 'float'>
print(5//2)  # 2
print(type(5//2))  # <class 'int'>
~~~

De eerste bewerking met **true division** levert een **niet afgeronde uitkomst** op.  
Gezien het resultaat een kommagetal is, is het resultaat een waarde van het **type float**
(gezien je geen kommaresultaten in een integer kan opslaan).

### Delen van floats

Bij de **floor division** daarentegen wordt er afgerond naar beneden en is het **resultaat een integer**. 
Bewerkingen waarbij een float is betrokken (ook voor andere rekenkundige operatoren) zullen altijd in een float resultaat resulteren (ongeacht het andere getal een integer is of niet).

~~~
print(3.50 / 2)  # 1.75
print(type(3.50/2))  # <class 'float'>
print(3.50 // 2)  # 1.0
print(type(3.50//2))  # <class 'float'>
~~~

Wel zullen de operatoren het resultaat afronden of niet naargelang het om floor division of true division gaat.

### Operatoren en operanden (terminologie)

Er zijn verschillende soorten expressies of bewerkingen, naast de **rekenkundige** hebben we ook **logische**, **relationele** en **binaire** expressies die we gedurende deze cursus nader zullen bekijken.

Wat deze wel gemeenschappelijk hebben, is dat deze alle uit **operatoren** en **operanden** bestaan:

* Een **operator** is het symbool dat de bewerking voorstelt.
* De **operanden** zijn de waardes (of andere expressies) die aan de **linker- en rechterkant** staan van de **operator**.

Bijvoorbeeld als ik de **variabele a** vermenigvuldig met de **literal 2**:

~~~
a = 5
b = a * 2
print(b)  # 10
~~~

Dan is **a** de **linker-operand** en **2** de **rechter-operand**  
Een **operand** dient altijd zelf een **expressie** te zijn. In dit geval zijn de operanden een **variabele** en een **literal**, maar een operand kan ook een andere rekenkundige expressie zijn:

~~~
a = 5
b = a * (2 + a)
print(b)  # 35
~~~

### Volgorde van berekeningen (operator precedence) en haakjes

Net zoals in klassieke wiskunde kan je ook **haakjes** gebruiken om de **volgorde van berekening** af te dwingen.  
Als je dit niet doet, zal Python de voorrang van bewerkingen bepalen aan de hand van basisregels.

~~~python
a = 5
b = a * 2 + a
print(b)  # 15
~~~

In bovenstaand geval zal eerst worden vermenigvuldigd (a * 2) en dan pas de som worden gemaakt met a.

De volgende volgorde wordt gerespecteerd:

* alles binnen de haakjes
* machten uitwerken
* vermenigvuldigen, delen en rest
* optellen en aftrekken

Binnen de zelfde "voorrang" (*precedence*), bijvoorbeeld *, /, // en %, wordt er van links naar rechts gewerkt.

Bijvoorbeeld:

~~~python
print(260 / 10 * 2)  # 52 en niet 13
print(35 - 10 + 4)  # 29 en niet 21
~~~

Als je van **links naar rechts** uitrekent tussen operatoren met **dezelfde voorrang** krijg je een **wiskundig correct** resultaat:

~~~
260 / 10 * 2 = 26 * 2 = 52
35  - 10 + 4 = 25 + 4 = 29
~~~

Daarentegen als je van rechts naar links uitrekent, krijg je een ander (foutief) resultaat:

~~~
260 / 10 * 2 = 260 / 20 = 13
35  - 10 + 4 = 35 - 14 = 21
~~~
