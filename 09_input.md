## Invoer (en uitvoer)

### Invoer vragen aan de gebruiker

**Tot nog toe** hebben we enkel **literals** gebruikt om **variabelen** te initialiseren.  
Om **invoer** van de **gebruiker** te verkrijgen, voorziet Python in de functie **input**.  

~~~python
text = input("Enter text: ")
print("text " + text)
~~~

Bovenstaand voorbeeld gebruikt de functie **input** om een **tekst** op te vragen aan de gebruiker.  
Als parameter geef je een (optionele) prompt-tekst mee en als waarde van de functie-aanroep ontvang je de tekst als een string.

~~~
                                                       +-------------------------+
                                                       |      CONSOLE/OUTPUT     |
+----------+                                           | +---------------------+ |
| text     |            +------------------+           | |                     | |
+----------+            | Statement 2:     +--+input+--> | > Enter text: hello | |
| hello    <--+write+---+ function call    |           | |                     | |
+----+-----+            +-------+----------+           | |                     | |
     |                          |                      | |                     | |
     |                          |                      | |                     | |
     |                  +-------v----------+           | |                     | |
     +--------+read+----> Statement 3:     |           | |                     | |
                        | function call    +--+print+--> | > Entered text      | |
                        +------------------+           | +---------------------+ |
                                                       +-------------------------+

~~~

Als je dit uitvoert, zal de (Python-)console wachten tot je een tekst invoert, gevolgd door een druk op de entertoets.

~~~
$ python enter_test.py
Enter text: hello
Entered hello
~~~

### Input geeft een string

Het resultaat van de functie **input** sla je meestal op in een variabele.  

~~~python
number = input("Enter number: ")
number_two = input("Enter another number: ")

print ("type of number ", type(number))
print ("type of number_two ", type(number_two ))
~~~

Het type van deze **return value** is een **str**, zoals je in de uitvoer van dit programma ziet:

~~~
Enter number: 22
Enter another number: Elvis
type of number <class 'str'>
type of number_two <class 'str'>
~~~

### Wat als je met getallen wil werken?

Je kan een **string** omvormen naar een **integer**, om te kunnen bewerken als een getal.  
Je kan dit doen door het resultaat van de functie input te converteren via de functie **int**.

~~~python
number_input = input("Enter number: ")
number = int(number_input)
number_two = input("Enter another number: ")

print ("type of number ", type(number))
print ("type of number_two ", type(number_two ))
~~~

Om te vermijden dat je een extra variabele moet gebruiken (number_input) kan je de aanroep van input nesten in de aanroep van int:

~~~python
number = int(input ("Enter number: "))
number_two = input("Enter another number: ")

print ("type of number ", type(number))
print ("type of number_two ", type(number_two ))
~~~

Als je dan een getal ingeeft, zie je dat variabele number ook echt een int is (waar je rekenkundige bewerkingen mee kan uitvoeren):

~~~
Enter number: 22
Enter another number: Elvis
type of number <class 'int'>
type of number_two <class 'str'>
~~~

### Wat als je geen getal ingeeft?

Als je geen getal ingeeft, zal de functie int een foutmelding tonen:

~~~
$ python enter_test.py
Enter number: arzez
Traceback (most recent call last):
  File "enter_test.py", line 1, in <module>
    number = int(input ("Enter number: "))
ValueError: invalid literal for int() with base 10: 'arzez'
~~~

Je ziet hier een **ValueError**, en je krijgt ook een omschrijving van wat er mis is: de functie int verwacht een literal die een getal is met grondtal 10, en 'arzez' is dat niet.

Later in de cursus zien we hoe je in je programma kan reageren op dit soort foutmeldingen zoals een ValueError.
