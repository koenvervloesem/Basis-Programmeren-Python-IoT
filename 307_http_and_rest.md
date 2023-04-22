## Werken met REST en web-API's

In dit deel leggen we uit hoe we met **web-API's** werken. We starten aan de clientkant via de Python-bibliotheek **Requests**. Daarna gaan we verder met de serverkant met behulp van het framework **Flask**.

Maar voordat we aan de slag gaan, bekijken we de werking van HTTP, de technologie achter het web:

* HTTP
* HTTP met HTML: webpagina's
* HTTP met data: JSON, XML, ...

### HTTP

Laten we starten met HTTP.  
**HTTP** staat voor **H**yper**T**ext **T**ransfer **P**rotocol.  

**HTTP** behoort tot **laag 7** van het **OSI**-model voor netwerken.  
Het protocol wordt typisch toegankelijk gemaakt op poort 80 (tijdens de ontwikkeling dikwijls poort, 8000, 8080 of 5000). De versleutelde vorm HTTPS maakt gebruik van poort 443.

Het betreft een **applicatienetwerkprotocol** (client-server) dat wordt gebruikt om **resources** van een server te benaderen.

~~~
    +----------------------+
    | 7   APPLICATION      |   ==> HTTP
    +----------------------+
    | 6   PRESENTATION     |   ==> ENCODERINGEN
    +----------------------+
    | 5   SESSION          |   ==> AUTHENTICATIE, SESSIES, ...
    +----------------------+
    | 4   TRANSPORT        |   ==> TCP, UDP, ...
    +----------------------+
    | 3   NETWORK          |   ==> ROUTER-LEVEL (IP)
    +----------------------+
    | 2   DATA LINK        |   ==> SWITCH-LEVEL (MAC)
    +----------------------+
    | 1   PHYSICAL         |   ==> TRANSMISSIE OVER KABEL
    +----------------------+
~~~

Deze **resources** kunnen van velerlei aard zijn.  
Historisch waren dit vooral **resources** nodig om webpagina's te laden (HTML, afbeeldingen, video's, ...).  

Vandaag wordt HTTP via REST ook veel gebruikt om data van een server af te halen.  

~~~
 +--------+   -- request -->     +--------+   html   gif
 | client |                      | server |   json   png
 +--------+   <- response --     +--------+   xml    ...
~~~

### Browsers, HTTP en HTML

Een eerste vorm van applicaties die HTTP gebruiken, zijn webbrowsers zoals Firefox en Chrome. De gebruiker kan daarmee webpagina's en bijbehorende resources (zoals afbeeldingen en video's) afhalen.  


~~~
\O/                      +---------+  -- request -->   +----------+   
 |  ======= url ======>  | BROWSER |  BROWSER CONTENT  |  SERVER  |   
/ \ bv. www.google.com   +---------+  <- response --   +----------+   
~~~

Dit gebeurt meestal via **HTML-pagina's** (statisch of dynamisch gegenereerd).  
**HTML** is de standaard opmaaktaal voor **webpagina's** en staat voor **H**yper**T**ext **M**arkup **L**anguage.

Een heel **eenvoudig voorbeeld** vind je hieronder:

~~~html
<!DOCTYPE html>
<html>
    <head>
        <title>Een voorbeeldpagina</title>
    </head>
    <body>
        <h1>Een voorbeeldpagina</h1>
        <img src="hello.png">
    </body>
</html>
~~~

Naast het eigenlijke document zal de browser (na het parsen van het document) ook
de bijhorende elementen (nodig voor visualisatie) waarnaar verwezen wordt binnen de html - zoals **afbeeldingen** en **video's** - downloaden en in het browserscherm tonen.

~~~
 +----------+                    +-------------+
 | browser  |                    |    server   |
 +----------+                    +-------------+
 | (1)      |   --request-->     | /hello.html |
 | DOWNLOAD |  GET hello.html    |             |
 | HTML     |  <--response--     |             |   
 |          |                    |             |
 | (2)      |                    |             |
 | ...      |                    |             |
 | PARSING  |                    |             |
 | HTML     |                    |             |
 | ...      |                    |             |
 |          |                    |             |
 | (3)      |   --request-->     | /hello.png  |
 | DOWNLOAD |  GET hello.png     |             |
 | IMAGE    |  <--response--     |             |
 +----------+                    +-------------+
~~~

### HTTP en HTML in actie

Je kunt deze gang van zaken eenvoudig testen door in Python een kleine webserver te starten die statische resources serveert.

> Zie voor het volledige verhaal https://docs.python.org/3/library/http.server.html

#### Stap 1: maak een html-bestand aan

Maak een bestand test.html aan en kopieer de onderstaande inhoud (html-code) in dit bestand.

~~~html
<!DOCTYPE html>
<html>
    <head>
        <title>Een voorbeeldpagina</title>
    </head>
    <body>
        <h1>Een voorbeeldpagina</h1>
        <img src="hello.png">
    </body>
</html>
~~~

#### Stap 2: voeg een hello.png toe

Met de img-tag geven we aan dat de webbrowser een afbeelding downloadt en die op het scherm toont.
Plaats in dezelfde map als het html-bestand een png-bestand met de naam hello.png.

Je kan hiervoor een willekeurig png-bestand van het internet plukken, zoals https://www.ucll.be/sites/default/files/documents/algemeen/logo/logo_ucll_rgb.png. Vergeet dan niet het gedownloade bestand te hernoemen naar hello.png, of in de img-tag van de html-code naar de juiste bestandsnaam te verwijzen.

#### Stap 3: start de webserver

Voer de volgende opdracht op de commandline uit:

~~~
$ python -m http.server 9000
Serving HTTP on 0.0.0.0 port 9000 (http://0.0.0.0:9000/) ...
~~~

Dit start een webserver op die naar aanvragen luistert op poort 9000.

> Waarschuwing: Gebruik deze webserver niet voor productie. Dit is puur voor een test.

#### Stap 4: test via een webbrowser

Als laatste stap open je de url http://localhost:9000/test.html in je webbrowser. Dan zou je het volgende resultaat moeten krijgen:

![](html_page.png)

Aan de kant van de server zie je dat de webbrowser twee bestanden heeft aangevraagd aan de webserver:

~~~
$ python -m http.server 9000
Serving HTTP on 0.0.0.0 port 9000 (http://0.0.0.0:9000/) ...
127.0.0.1 - - [26/Apr/2022 23:43:18] "GET /test.html HTTP/1.1" 200 -
127.0.0.1 - - [26/Apr/2022 23:43:18] "GET /hello.png HTTP/1.1" 200 -
~~~

Als je dit in de netwerkprotocolanalyzer Wireshark zou bekijken, dan zie je dat je webbrowser twee interacties met een request en response heeft:

* Een eerste voor het html-bestand
* Een tweede voor het png-bestand

~~~
546	69.372822126	127.0.0.1	127.0.0.1	HTTP	519	GET /test.html HTTP/1.1 
550	69.374803049	127.0.0.1	127.0.0.1	HTTP	257	HTTP/1.0 200 OK  (text/html)
558	69.432336639	127.0.0.1	127.0.0.1	HTTP	454	GET /hello.png HTTP/1.1 
562	69.432966252	127.0.0.1	127.0.0.1	HTTP	26223	HTTP/1.0 200 OK  (PNG)
~~~

### HTTP-protocol

Een webpagina of andere resource wordt van een server afgehaald door middel van HTTP.  
Hoe ziet zo'n HTTP request/response er nu uit?

Zowel de request als de response bestaan elk uit een **header** en een **body**.

~~~
                +-----------------------------------+
                |         HTTP                      |
+-------+-------+----------+------------------------+
|       |       |          |                        |
|  TCP  |  IP   |  HEADER  |         BODY           |
|       |       |          |                        |
+-------+-------+----------+------------------------+
                           |      OPTIONEEL         |                         
                           +------------------------+ 

~~~

Deze **body** is optioneel en bevat de **payload**.  
Dit kan een stuk tekst zijn, zoals html, maar ook een png-bestand, video, ...


De **header** daarentegen is altijd verplicht en bevat metadata die nodig zijn om deze uitwisseling te voltooien.  

#### HTTP-request

In onderstaand dump (verkregen met Wireshark) zie je dat je webbrowser een bestand test.html aanvraagt.  

~~~
GET /test.html HTTP/1.1
Host: localhost:9000
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:99.0) Gecko/20100101 Firefox/99.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
DNT: 1
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
~~~

De **eerste regel** is de belangrijkste. Die geeft aan welke **resource** en welke **opdracht** er uitgevoerd dient te worden.  
Daarnaast wordt er nog wat bijkomende informatie verschaft in het volgende formaat:

~~~
<header-field>: <inhoud>
~~~

Zo geeft de webbrowser onder andere de volgende info:

* De host is localhost
* Met poort 9000
* De browser aanvaardt een aantal mediatypes, zoals text/html
* ...

#### De HTTP-response

Bij de HTTP-response is de eerste regel ook de belangrijkste.  
Deze geeft namelijk de status aan van de operatie. In onderstaand voorbeeld is deze OK (bevestiging dat de operatie geslaagd is):

~~~
HTTP/1.0 200 OK
Server: SimpleHTTP/0.6 Python/3.9.7
Date: Tue, 26 Apr 2022 21:48:58 GMT
Content-type: text/html
Content-Length: 189
Last-Modified: Tue, 26 Apr 2022 21:31:30 GMT
<html>
    <body>
        <title>Een voorbeeldpagina</title>
    </body>
    <head>
        <h1>Een voorbeeldpagina</h1>
        <img src="hello.png">
    </head>
</html>
~~~

Daarnaast geeft deze onder andere:

* Het mediatype (text/html)
* De lengte in bytes van de content
* Het tijdstip van aanmaak, wat gebruikt wordt voor caching
* De eigenlijke content

#### Method (HTTP REQUEST)

Essentieel om HTTP (en later REST) te verstaan, is dat er verschillende operaties mogelijk zijn.  
We hadden onder andere al de GET-methode gezien, maar er zijn heel wat andere methodes of operaties mogelijk.  

Je kan deze operaties een beetje vergelijken met de CRUD-operaties op een database die we eerder hebben gezien. De vier belangrijkste die wij zullen gebruiken, zijn:

* **GET**  
  Met deze operatie vraag je een resource/document op van de server.  
  Binnen CRUD kan je dit als een **read** beschouwen.
* **POST**  
  Dit overschrijft of maakt een **nieuwe resource** aan.   
  Dit wordt beschouwd als een **create**.
* **PUT**  
  Dit **wijzigt** een **resource** (meestal via een webformulier).  
  Dit wordt beschouwd als een **update**.
* **DELETE**  
  Hiermee verwijder je een resource.

Bekijk voor een vollediger overzicht:

* https://www.w3schools.com/tags/ref_httpmethods.asp
* https://developer.mozilla.org/en-US/docs/Web/HTTP
* https://www.w3.org/Protocols/Specs.html

##### Praktisch voorbeeld

Als voorbeeld gaan we dit nu in de praktijk brengen.

De bedoeling is uiteindelijk dat we met Flask een REST-gebaseerde studentenapplicatie met een webinterface schrijven.

Als we de voorgaande principes zouden toepassen op dit voorbeeld, krijg je volgende mapping:

| Beschrijving                              | Method | Endpoint      | Idempotent |
|-------------------------------------------|--------|---------------|------------|
| Maak een student aan                      | POST   | /student      | Nee        |
| Vraag de info over een student            | GET    | /student/{id} | Ja         |
| Pas data van een student aan (bv. punten) | PUT    | /student/{id} | Ja         |
| Verwijder een student uit database        | DELETE | /student/{id} | Ja         |

#### STATUS (HTTP RESPONSE)

Een **status** is naast de eigenlijke **response body** het belangrijkste onderdeel.  
Het geeft namelijk twee stukken informatie door:

* Is de operatie geslaagd?
* Wat is ermee gebeurd?

De statuscodes kun je opdelen in vier groepen, afhankelijk van hun honderdtal.  
Codes die starten met onderstaande cijfers, geven aan dat:

* 2XX => OK, de operatie is geslaagd
* 3XX => INFO, dit kan bijvoorbeeld een redirect zijn (ander endpoint)
* 4XX => FOUT VAN JOU, je hebt bijvoorbeeld een niet bestaande pagina opgevraagd
* 5XX => FOUT VAN DE SERVER, er is bijvoorbeeld een interne fout op de server

De belangrijkste:

* 2XX
  * 200 **OK** – Het gevraagde document is succesvol opgevraagd.
* 3XX
  * 301 **Redirect** - De informatie bevindt zich in een andere resource (je browser zal dan deze resource gaan ophalen)
  * 304 **Not Modified** – De pagina is niet gewijzigd ten opzichte van de versie in de cache.
* 4XX
  * 400 **Bad Request** - De gebruiker heeft een fout gemaakt in het verzoek waardoor deze niet verwerkt kan worden.
  * 403 **Forbidden** – Het opgevraagde document mag niet opgevraagd worden.
  * 404 **Not Found** – Het opgevraagde document bestaat niet.
  * 405 **Method Not Allowed** – De gebruikte requestmethode is niet toegestaan.
  * 410 **Gone** – Het opgevraagde document heeft bestaan maar is niet meer beschikbaar.
  * 451 **Unavailable For Legal Reasons** - Het opgevraagde document kan niet worden weergegeven omwille van juridische redenen.
* 5XX
  * 500 **Internal Server Error** – De webserver heeft de gevraagde actie niet kunnen uitvoeren.
  * 503 **Service Temporarily Unavailable** – De webserver is tijdelijk in onderhoud.

Als je bijvoorbeeld een refresh van je pagina uitvoert, zie je dat de server een 304 teruggeeft:

~~~
(base) bart@bvlegion:~/Tryout/http_py_server$ python -m http.server 9000
Serving HTTP on 0.0.0.0 port 9000 (http://0.0.0.0:9000/) ...
127.0.0.1 - - [26/Apr/2022 23:43:18] "GET /test.html HTTP/1.1" 200 -
127.0.0.1 - - [26/Apr/2022 23:43:18] "GET /hello.png HTTP/1.1" 200 -
127.0.0.1 - - [26/Apr/2022 23:48:01] "GET /test.html HTTP/1.1" 304 -
127.0.0.1 - - [26/Apr/2022 23:48:01] "GET /hello.png HTTP/1.1" 304 -
~~~

### Wat is een API?

We gaan nu echter geen webpagina's maken en gebruiken, maar API's die data teruggeven.

Een API of **A**pplication **P**rogramming **I**nterface is een verzameling van operaties of functies 
die je tussen twee stukken software kan definiëren.  

De API's waarover wij hier spreken zijn netwerk-API's en specifieker REST-API's (REpresentational State Transfer).

### Web-API's

Tot hiertoe gebruikten we via een webbrowser het **http-protocol** om **(html-)resources** 
op te halen van een server.

~~~
\O/                     +---------+   -- request -->     +----------+   
 |  ======= url ======> | BROWSER |  BROWSER CONTENT     |  SERVER  |   
/ \ bv. www.google.com  +---------+  <-- response --     +----------+   
~~~

HTTP kan echter ook gebruikt worden in andere scenario's om data op te halen (in plaats van webpagina's): 

~~~
                 (1)                        (2)
+--------+  -- request -->  +--------+   -- request -->  +------------+   
| SENSOR |      DATA        | SERVER |       DATA        | APPLICATIE |
+--------+ <-- response --  +--------+  <-- response --  +------------+ 
                                                         (bv. dashboard)
~~~

Een typisch voorbeeldscenario is waarbij een of meerdere sensoren (of een embedded device met een sensor) sensordata doorsturen naar een server in de cloud.  

Aan de andere kant zal een andere applicatie (zoals bijvoorbeeld een dashboard) deze data (die wordt bijgehouden op de server) afhalen.  
Dit is een veel voorkomend patroon binnen IoT.

Zulke API's die beschikbaar zijn op het web (of cloud), noemen we web-API's.  
Laten we een aantal elementen bekijken van deze API's.


#### REST (structuur van de API)

Veel API's gebruiken het principe van REST of RESTful design.  
Dit is niet zozeer een protocol, maar eerder een (architecturale) stijl van hoe je data (of "resources") ordent.

De basisprincipes:

* Je gebruikt de HTTP-methodes (POST, GET, PUT, DELETE) om CRUD-operaties uit te voeren.
* Je entiteiten worden voorgesteld als http-resources.
* Deze resources worden binnen de URL hiërarchisch geordend.

Het programma Insomnia (https://insomnia.rest) laat je toe om eenvoudig HTTP- en REST-aanvragen te doen en de resultaten te bekijken. Download het programma van https://insomnia.rest/download om eenvoudige tests uit te voeren.

Voer nu in de adresbalk in het midden de url https://randomuser.me/api/ in en klik rechts ervan op **Send**. Je krijgt dan rechts als preview een hoop data in de vorm van een JSON-string. JSON (https://www.json.org/json-en.html) staat voor JavaScript Object Notation en is een tekstformaat om gestructureerde data uit te wisselen. Het wordt veel in REST-API's gebruikt.

We kunnen ook extra query parameters toevoegen. Voeg in het tabblad **Query** onder de adresbalk een parameter **gender** met waarde **female** in een parameter **nat** met waarde **DE**. Insomnia past de url nu aan tot https://randomuser.me/api/?gender=female&nat=NL. Klik nog eens op **Send**. Je krijgt nu data van een willekeurige vrouwelijke gebruiker met Duitse naam.

Ook in Python kunnen we een REST-API aanroepen, namelijk met de module `requests`. Probeer dit bijvoorbeeld eens uit in de REPL:

~~~python
>>> import requests
>>> response = requests.get("https://randomuser.me/api/")
>>> response.text
'{"results":[{"gender":"male","name":{"title":"Mr","first":"Melvin","last":"Holmes"},"location":{"street":{"number":3902,"name":"Fairview Road"},"city":"Portsmouth","state":"Tyne and Wear","country":"United Kingdom","postcode":"R2B 7LB","coordinates":{"latitude":"-5.5941","longitude":"-137.8553"},"timezone":{"offset":"-4:00","description":"Atlantic Time (Canada), Caracas, La Paz"}},"email":"melvin.holmes@example.com","login":{"uuid":"b6462ae3-84d0-4346-a6a0-8e107a9f8ff9","username":"redbear579","password":"having","salt":"HsZansyi","md5":"b003a842de456a8404aa8f1cca7a326c","sha1":"70ac4801b7f71c6fe623316ba4c81797430adfa9","sha256":"32005634c0ce6679632eaa011e32602684ff37e63448dddd3ab7a3055d287fb6"},"dob":{"date":"1997-10-21T19:35:13.544Z","age":25},"registered":{"date":"2004-01-15T02:24:01.423Z","age":19},"phone":"016973 76793","cell":"07888 154639","id":{"name":"NINO","value":"KW 85 91 00 J"},"picture":{"large":"https://randomuser.me/api/portraits/men/4.jpg","medium":"https://randomuser.me/api/portraits/med/men/4.jpg","thumbnail":"https://randomuser.me/api/portraits/thumb/men/4.jpg"},"nat":"GB"}],"info":{"seed":"91e2e7bba1572ef7","results":1,"page":1,"version":"1.4"}}'
~~~

Het resultaat is een string. Maar in Python kunnen we er ook een JSON-structuur van maken en er onderdelen uit halen:

~~~python
>>> response.json()["results"][0]["name"]
{'title': 'Mr', 'first': 'Melvin', 'last': 'Holmes'}
~~~

We kunnen dus tussen rechte haken keys zetten zoals in een Python-dictionary om de bijbehorende waarde eruit te halen, en de 0 is een index om het eerste element uit de lijst "results" te halen.

En we kunnen ook extra query-parameters toevoegen in de url:

~~~python
>>> response = requests.get("https://randomuser.me/api/?gender=female&nat=DE")
>>> response.json()["results"][0]["name"]
{'title': 'Ms', 'first': 'Ronja', 'last': 'Schädlich'}
~~~

Of ook op deze manier:

~~~python
>>> query_params = {"gender": "female", "nat": "DE"}
>>> response = requests.get("https://randomuser.me/api/", params=query_params)
>>> response.json()["results"][0]["name"]
{'title': 'Ms', 'first': 'Mareen', 'last': 'Wecker'}
~~~

We maken hier dus een dictionary met de query-parameters aan en geven die als extra argument door aan de methode `requests.get`.
