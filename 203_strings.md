## Werken met tekst

Eerder hebben we reeds kennisgemaakt met het datatype string (str) in Python.  
Hieronder gaan we dit even herhalen en wat verder uitspitten...

### Literals

#### String als literal

Het eerste gebruik dat we hebben gezien was als **literal**:

~~~python
>>> text = "Hello World"
>>> print(text)
Hello World
~~~

#### Enkele en dubbele aanhalingstekens

Zo'n literal wordt **gedemarkeerd** door aanhalingstekens (*quotes*). Dit kunnen enkele of dubbele aanhalingstekens zijn.  
Belangrijk is wel: als je start met de ene soort (" of '), moet je ook eindigen met dezelfde:

~~~python
>>> text = "Hello world'
  File "<stdin>", line 1
    text = "Hello world'
           ^
SyntaxError: unterminated string literal (detected at line 1)
~~~

Je kunt wel enkele aanhalingstekens binnen een string met dubbele aanhalingstekens gebruiken en andersom:

~~~python
>>> print("A string in double quotes can contain 'single quote' characters.")
A string in double quotes can contain 'single quote' characters.
>>> print('A string in single quotes can contain "double quote" characters.')
A string in single quotes can contain "double quote" characters.
~~~

Dit doe je als je enkele of dubbele aanhalingstekens wilt tonen.

#### Backslash als escape character

Als je toch dubbele aanhalingstekens wilt gebruiken binnen een string die je met dubbele aanhalingstekens demarkeert, kan je altijd een backslash gebruiken als escape character:

~~~python
>>> print("\"Double quotes\" with backslash.")
"Double quotes" with backslash.
>>> print('\'Singe quotes\' with backslash.')
'Singe quotes' with backslash.
~~~

Een **escape character** laat je toe om tekens in een literal te zetten die er normaal niet kunnen instaan:

~~~
\\	      Backslash (\)
\'	      Single quote (')
\"	      Double quote (")
~~~

Naast het escapen van deze quotes, gebruik je de backslash om er voor te zorgen dat je deze backslash zelf in een literal kan plaatsen.

~~~python
>>> print("Een backslash \\ gebruik je in Python als escape character.")
Een backslash \ gebruik je in Python als escape character.
~~~

#### Speciale tekens 

Een backslash kan ook worden gebruikt om specifieke karakters te tonen zoals een tab of een nieuwe regel:


~~~
\n	      ASCII Linefeed (LF)
\r	      ASCII Carriage Return (CR)
\t	      ASCII Horizontal Tab (TAB)
~~~

Bijvoorbeeld:

~~~python
>>> print("Deze tekst wordt gevolgd door een tab\t en \n een newline.")
Deze tekst wordt gevolgd door een tab	 en 
 een newline.
~~~

#### Triple quotes

Een gewone string literal kan je maar op één regel invoeren. Zodra je op enter drukt om een volgende regel in te voeren zonder dat je de string met een aanhalingsteken hebt gesloten, krijg je een foutmelding:

~~~python
>>> print("Dit is een string die
  File "<stdin>", line 1
    print("Dit is een string die
          ^
SyntaxError: unterminated string literal (detected at line 1)
~~~

Maar als je drie aanhalingstekens gebruikt (dubbele of enkele), kan je een string literal over meerdere regels invoeren tot je de string met weer drie aanhalingstekens sluit:

~~~python
>>> print("""Dit is een string die
... over meerdere regels
... loopt.""")
Dit is een string die
over meerdere regels
loopt.
~~~

#### ASCII-codes

Een stuk tekst bestaat eigenlijk uit opeenvolgende bytes: elk teken wordt door een byte met een specifieke numerieke waarde voorgesteld.

De standaardcodering van tekens door bytes is de ASCII-codering (American Standard Code for Information Interchange). Deze wordt gebruikt om tekstbestanden op te slaan, maar ook in Python voor het datatype string.

De codering wordt voorgesteld in een ASCII-tabel:

~~~
Dec Hex    Dec Hex    Dec Hex  Dec Hex  Dec Hex  Dec Hex   Dec Hex   Dec Hex  
  0 00 NUL  16 10 DLE  32 20    48 30 0  64 40 @  80 50 P   96 60 `  112 70 p
  1 01 SOH  17 11 DC1  33 21 !  49 31 1  65 41 A  81 51 Q   97 61 a  113 71 q
  2 02 STX  18 12 DC2  34 22 "  50 32 2  66 42 B  82 52 R   98 62 b  114 72 r
  3 03 ETX  19 13 DC3  35 23 #  51 33 3  67 43 C  83 53 S   99 63 c  115 73 s
  4 04 EOT  20 14 DC4  36 24 $  52 34 4  68 44 D  84 54 T  100 64 d  116 74 t
  5 05 ENQ  21 15 NAK  37 25 %  53 35 5  69 45 E  85 55 U  101 65 e  117 75 u
  6 06 ACK  22 16 SYN  38 26 &  54 36 6  70 46 F  86 56 V  102 66 f  118 76 v
  7 07 BEL  23 17 ETB  39 27 '  55 37 7  71 47 G  87 57 W  103 67 g  119 77 w
  8 08 BS   24 18 CAN  40 28 (  56 38 8  72 48 H  88 58 X  104 68 h  120 78 x
  9 09 HT   25 19 EM   41 29 )  57 39 9  73 49 I  89 59 Y  105 69 i  121 79 y
 10 0A LF   26 1A SUB  42 2A *  58 3A :  74 4A J  90 5A Z  106 6A j  122 7A z
 11 0B VT   27 1B ESC  43 2B +  59 3B ;  75 4B K  91 5B [  107 6B k  123 7B {
 12 0C FF   28 1C FS   44 2C ,  60 3C <  76 4C L  92 5C \  108 6C l  124 7C |
 13 0D CR   29 1D GS   45 2D -  61 3D =  77 4D M  93 5D ]  109 6D m  125 7D }
 14 0E SO   30 1E RS   46 2E .  62 3E >  78 4E N  94 5E ^  110 6E n  126 7E ~
 15 0F SI   31 1F US   47 2F /  63 3F ?  79 4F O  95 5F _  111 6F o  127 7F DEL
~~~

In Python kan je met deze ASCII-codes een string construeren, bijvoorbeeld met de hexadecimale waardes:

~~~python
>>> print("\x68\x65llo")
hello
~~~

Python kent ook en functie om een ASCII-waarde naar het bijbehorende teken om te zetten:

~~~python
>>> chr(104)
'h'
>>> chr(0x68)
'h'
~~~

En ook andersom kun je de ASCII-waarde van een teken opvragen:

~~~python
>>> ord("h")
104
~~~

### Opvragen van tekst op de console

Je kan zoals we al zagen tekst opvragen op de console met de functie input:

~~~python
>>> name = input("Geef uw naam: ")
Geef uw naam: Koen
>>> print(name)
Koen
~~~

### String concatenation

Je kan via de +-operator verschillende strings 'aan elkaar plakken' (concateneren):

~~~python
>>> text = "Hello"
>>> print(text + " world")
Hello world
~~~

### Concatenatie met andere types

Let op als je een string wilt concateneren met andere data-types:

~~~python
>>> text = "Hello"
>>> a_number = 2
>>> print(text + " world " + a_number)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate str (not "int") to str
~~~

Zoals de `TypeError` toont, kan je geen int concateneren aan een string met de +-operator.

Hoe doe je dat dan wel? Door eerst de int naar een string om te zetten:

~~~python
>>> text = "Hello"
>>> a_number = 2
>>> print(text + " world " + str(a_number))
Hello world 2
~~~

### Concatenatie via print

De concatenatie kan echter ook via de functie print:

~~~python
>>> text = "Hello"
>>> a_number = 2
>>> print(text, "world", a_number)
Hello world 2
~~~

Je kan hier alle types door elkaar gebruiken, aangezien de functie print zelf de omzetting zal uitvoeren via str().

Merk op: print voegt standaard een spatie tussen elk argument toe.

### String vermenigvuldigen

Naast de +-operator heb je ook de \*-operator:

~~~python
>>> "allo" * 3
'alloalloallo'
~~~

En dit kun je ook nog combineren met de concatenatie:

~~~python
>>> "allo" * 3 + "vélo"
'alloalloallovélo'
~~~

### Lengte van strings

We kunnen eenvoudig het aantal tekens in een string opvragen:

~~~python
>>> len("testje")
6
>>> len("")
0
~~~

### Stringinterpolatie of f-strings (Python 3.6+)

Sinds **Python 3.6** kun je gebruikmaken van **stringinterpolatie**. Hiermee kun je in een string rechtstreeks **expressies injecteren**.

Hiervoor moet je:

* De string laten **starten** met **f** vóór het eerste aanhalingsteken.
* De **expressies** in de string tussen accolades {} (*curly braces*) plaatsen.

Bijvoorbeeld:

~~~python
>>> name = "Koen"
>>> f"Hallo {name}!"
'Hallo Koen!'
~~~

Alle expressies zijn toegestaan tussen de accolades, dus ook **ook rekenkundige expressies**:

~~~python
>>> a = 2
>>> b = 3
>>> c = 5
>>> print(f"{a} + {b} * {c} = {a + b * c} and not {(a + b) * c}.")
2 + 3 * 5 = 17 and not 25.
~~~

En je kunt ook functies gebruiken tussen de accolades:

~~~python
>>> name = "Koen"
>>> print(f"Hallo {name}! Je naam is {len(name)} letters lang.")
Hallo Koen! Je naam is 4 letters lang.
~~~
