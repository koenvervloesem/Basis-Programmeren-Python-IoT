## Van HTTP naar REST met Flask

De technologie om webpagina's te laden wordt dus ook gebruikt om programma's met elkaar te laten praten.  
Hoe gaan we dit nu realiseren met Python?   

Hiervoor hebben we twee modules nodig:

* **Requests** => API's aanroepen (client)
* **Flask** => Webapplicaties en API's aanmaken (server)


~~~
+--------------+               +--------------+
|              |               |              |
|              | <------------ +              |
|  CLIENT      |               |    SERVER    |
|  REQUESTS    |               |    FLASK     |
|              + ------------> |              |
|              |               |              |
+--------------+               +--------------+
~~~


### Installatie

In het deel over modules hadden we beide bibliotheken reeds geïnstalleerd via pip.  Mocht dit nog niet gebeurd zijn, dan kun je dit alsnog doen:

~~~
$ pip install flask requests
~~~

### Eerste programma

We starten eenvoudig, namelijk met een applicatie die de string "Hello, World!" teruggeeft:

~~~python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "Hello, World!"

if __name__ == "__main__":
    app.run(port=5000, debug=True)
~~~

De `@app.route` is een **decorator** die op de functie erna inwerkt. Ze zorgt ervoor dat als je de url `/` bezoekt die je als argument aan de decorator doorgeeft, de functie erna wordt uitgevoerd en het resultaat aan de gebruiker wordt teruggegeven.

### Programma starten

Start dit programma nu:

~~~
$ python demo.py 
 * Serving Flask app "demo" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
~~~

Als je dan in je webbrowser navigeert naar http://localhost:5000/ zou je het volgende resultaat moeten krijgen:

![](hello_flask.png)

En het terminalvenster waarin je je applicatie hebt gestart, toont de HTTP GET-requests die je webbrowser uitvoert:

~~~
$ python demo.py 
 * Serving Flask app "demo" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
127.0.0.1 - - [19/May/2022 18:56:44] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [19/May/2022 18:56:44] "GET /favicon.ico HTTP/1.1" 404
~~~

### Ophalen van data met Insomnia

Probeer eerst de REST API uit met het programma Insomnia. Vul in de adresbalk in het midden http://localhost:5000 in en klik rechts ernaast op **Send**. Dan krijg je aan de rechterkant in de preview weer de webpagina met de tekst **Hello, World!** te zien. Klik je op **Headers**, dan zie je de headers die de webserver teruggeeft.

Merk op: in het terminalvenster waarin je je applicatie hebt gestart, zie je deze keer geen HTTP GET-request voor favicon.ico. Insomnia vraagt dit immers niet op, maar webbrowsers wel.

### Ophalen van data in Python met Requests

We kunnen nu ook in Python dezelfde data ophalen, namelijk met de module `requests`:

~~~python
import requests

reply = requests.get("http://localhost:5000/")
print("Headers:", reply.headers)
print("OK:", reply.ok)
print("Status:", reply.status_code)
print("Content:", reply.text)
~~~

~~~
$ python client.py
Headers: {'Content-Type': 'text/html; charset=utf-8', 'Content-Length': '13', 'Server': 'Werkzeug/2.0.2 Python/3.9.7', 'Date': 'Wed, 25 May 2022 12:28:21 GMT'}
OK: True
Status: 200
Content: Hello, World!
~~~

### Niet bestaande resource (404)

Wat als we een niet bestaande resource opvragen?

~~~python
import requests

reply = requests.get("http://localhost:5000/notexisting")
print("Headers:", reply.headers)
print("OK:", reply.ok)
print("Status:", reply.status_code)
print("Content:", reply.text)
~~~

Dan geeft onze client dit terug:

~~~
$ python client.py
Headers: {'Content-Type': 'text/html; charset=utf-8', 'Content-Length': '232', 'Server': 'Werkzeug/2.0.2 Python/3.9.7', 'Date': 'Wed, 25 May 2022 12:29:23 GMT'}
OK: False
Status: 404
Content: <!doctype html>
<html lang=en>
<title>404 Not Found</title>
<h1>Not Found</h1>
<p>The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.</p>
~~~

Merk op dat `reply.ok` nu de waarde `False` heeft, de statuscode 404 is en de content een webpagina met een uitleg die je ook zou zien als je de niet bestaande url in je webbrowser zou opvragen.

### Verboden operatie (405)

Stel dat we nu een operatie willen uitvoeren waarvoor we geen toestemming hebben, zoals het verwijderen van een resource:

~~~python
import requests

reply = requests.delete("http://localhost:5000/")
print("Headers:", reply.headers)
print("OK:", reply.ok)
print("Status:", reply.status_code)
print("Content:", reply.text)
~~~

Dan geeft de webserver statuscode 405 terug en een overeenkomende webpagina:

~~~
$ python client.py
Headers: {'Content-Type': 'text/html; charset=utf-8', 'Allow': 'GET, HEAD, OPTIONS', 'Content-Length': '178', 'Server': 'Werkzeug/2.0.2 Python/3.9.7', 'Date': 'Wed, 25 May 2022 12:31:57 GMT'}
OK: False
Status: 405
Content: <!doctype html>
<html lang=en>
<title>405 Method Not Allowed</title>
<h1>Method Not Allowed</h1>
<p>The method is not allowed for the requested URL.</p>
~~~

### Netwerkverbinding

Als je in je Python-programma verbinding maakt met een server, werkt die verbinding niet altijd. Je netwerk ligt er even uit, of de server draait niet, of je maakt een typfout in de naam van de server of de poort. Hoe gaat je client daarmee om?

Sluit bijvoorbeeld je server af met Ctrl+C en laat de client proberne om ermee te verbinden:

~~~python
import requests

reply = requests.get("http://localhost:5000/")
print("Headers:", reply.headers)
print("OK:", reply.ok)
print("Status:", reply.status_code)
print("Content:", reply.text)
~~~

Dan krijg je een hele lange foutmelding:

~~~
$ python client.py
Traceback (most recent call last):
  File "/usr/lib/python3/dist-packages/urllib3/connection.py", line 169, in _new_conn
    conn = connection.create_connection(
  File "/usr/lib/python3/dist-packages/urllib3/util/connection.py", line 96, in create_connection
    raise err
  File "/usr/lib/python3/dist-packages/urllib3/util/connection.py", line 86, in create_connection
    sock.connect(sa)
ConnectionRefusedError: [Errno 111] Connection refused

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/usr/lib/python3/dist-packages/urllib3/connectionpool.py", line 699, in urlopen
    httplib_response = self._make_request(
  File "/usr/lib/python3/dist-packages/urllib3/connectionpool.py", line 394, in _make_request
    conn.request(method, url, **httplib_request_kw)
  File "/usr/lib/python3/dist-packages/urllib3/connection.py", line 234, in request
    super(HTTPConnection, self).request(method, url, body=body, headers=headers)
  File "/usr/lib/python3.10/http/client.py", line 1282, in request
    self._send_request(method, url, body, headers, encode_chunked)
  File "/usr/lib/python3.10/http/client.py", line 1328, in _send_request
    self.endheaders(body, encode_chunked=encode_chunked)
  File "/usr/lib/python3.10/http/client.py", line 1277, in endheaders
    self._send_output(message_body, encode_chunked=encode_chunked)
  File "/usr/lib/python3.10/http/client.py", line 1037, in _send_output
    self.send(msg)
  File "/usr/lib/python3.10/http/client.py", line 975, in send
    self.connect()
  File "/usr/lib/python3/dist-packages/urllib3/connection.py", line 200, in connect
    conn = self._new_conn()
  File "/usr/lib/python3/dist-packages/urllib3/connection.py", line 181, in _new_conn
    raise NewConnectionError(
urllib3.exceptions.NewConnectionError: <urllib3.connection.HTTPConnection object at 0x7fb70b24b6a0>: Failed to establish a new connection: [Errno 111] Connection refused

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/usr/lib/python3/dist-packages/requests/adapters.py", line 439, in send
    resp = conn.urlopen(
  File "/usr/lib/python3/dist-packages/urllib3/connectionpool.py", line 755, in urlopen
    retries = retries.increment(
  File "/usr/lib/python3/dist-packages/urllib3/util/retry.py", line 574, in increment
    raise MaxRetryError(_pool, url, error or ResponseError(cause))
urllib3.exceptions.MaxRetryError: HTTPConnectionPool(host='localhost', port=5000): Max retries exceeded with url: / (Caused by NewConnectionError('<urllib3.connection.HTTPConnection object at 0x7fb70b24b6a0>: Failed to establish a new connection: [Errno 111] Connection refused'))

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/koan/client.py", line 3, in <module>
    reply = requests.get("http://localhost:5000/")
  File "/usr/lib/python3/dist-packages/requests/api.py", line 76, in get
    return request('get', url, params=params, **kwargs)
  File "/usr/lib/python3/dist-packages/requests/api.py", line 61, in request
    return session.request(method=method, url=url, **kwargs)
  File "/usr/lib/python3/dist-packages/requests/sessions.py", line 542, in request
    resp = self.send(prep, **send_kwargs)
  File "/usr/lib/python3/dist-packages/requests/sessions.py", line 655, in send
    r = adapter.send(request, **kwargs)
  File "/usr/lib/python3/dist-packages/requests/adapters.py", line 516, in send
    raise ConnectionError(e, request=request)
requests.exceptions.ConnectionError: HTTPConnectionPool(host='localhost', port=5000): Max retries exceeded with url: / (Caused by NewConnectionError('<urllib3.connection.HTTPConnection object at 0x7fb70b24b6a0>: Failed to establish a new connection: [Errno 111] Connection refused'))
~~~

Je ziet hier een aantal exceptions terugkomen. De laatste is `requests.exceptions.ConnectionError`. Als we willen dat ons programma een gebruiksvriendelijker melding geeft bij verbindingsproblemen, kunnen we de code dus in een try-except blok zetten en deze exception opvangen:

~~~python
import requests

try:
    reply = requests.get("http://localhost:5000/")
    print("Headers:", reply.headers)
    print("OK:", reply.ok)
    print("Status:", reply.status_code)
    print("Content:", reply.text)
except requests.exceptions.ConnectionError as exception:
    print("Verbindingsfout:", exception)
~~~

De foutmelding ziet er nu wat overzichtelijker uit:

~~~
$ python client.py
Verbindingsfout: HTTPConnectionPool(host='localhost', port=5000): Max retries exceeded with url: / (Caused by NewConnectionError('<urllib3.connection.HTTPConnection object at 0x7f0598707820>: Failed to establish a new connection: [Errno 111] Connection refused'))
~~~

### Meerdere routes

In onze eerste Flask-applicatie gebruikten we maar één route, voor de root (`/`). Nu voegen we een tweede toe, voor `/test`:

~~~python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "Hello, World!"

@app.route("/test")
def hello_school():
    return "Hello, Mars!"

if __name__ == "__main__":
    app.run(port=5000, debug=True)
~~~

Je kunt nu in de client deze endpoints opvragen:

~~~python
import requests

BASE_URI = "http://localhost:5000"


def get_and_print_reply(uri):
    try:
        reply = requests.get(BASE_URI + uri)
        print("Headers:", reply.headers)
        print("OK:", reply.ok)
        print("Status:", reply.status_code)
        print("Content:", reply.text)
    except requests.exceptions.ConnectionError as exception:
        print("Netwerkfout:", exception)


get_and_print_reply("/")
get_and_print_reply("/test")
~~~

Met als resultaat:

~~~
$ python client.py
Headers: {'Server': 'Werkzeug/2.2.3 Python/3.10.6', 'Date': 'Sun, 16 Apr 2023 08:44:52 GMT', 'Content-Type': 'text/html; charset=utf-8', 'Content-Length': '13', 'Connection': 'close'}
OK: True
Status: 200
Content: Hello, World!
Headers: {'Server': 'Werkzeug/2.2.3 Python/3.10.6', 'Date': 'Sun, 16 Apr 2023 08:44:52 GMT', 'Content-Type': 'text/html; charset=utf-8', 'Content-Length': '12', 'Connection': 'close'}
OK: True
Status: 200
Content: Hello, Mars!
~~~

### Parameters

Tot nu bezochten we alleen nog maar vaste url's, zoals `/`  of `/test`. Maar we kunnen ook willekeurige url's toelaten door parameters aan de route van een Flask-applicatie toe te voegen:

~~~python
from flask import Flask

app = Flask(__name__)


@app.route("/")
def hello_world():
    return "Hello, World!"


@app.route("/test")
def hello_school():
    return "Hello, Mars!"


@app.route("/user/<username>")
def profile(username):
    return f"Hello {username}"


if __name__ == "__main__":
    app.run(port=5000, debug=True)
~~~

Door de route `"/user/<username>"` in de decorator op te nemen, geven we aan dat op de plaats van `<username>` een willekeurig deel van de url kunnen opgeven. Dit deel is dan als argument `username` beschikbaar in de functie erna.

Start de server:

~~~
$ python demo.py
~~~

Nu kunnen we in de client dus een url voor een willekeurige gebruiker opvragen:

~~~python
import requests

BASE_URI = "http://localhost:5000"


def get_and_print_reply(uri):
    try:
        reply = requests.get(BASE_URI + uri)
        print("Headers:", reply.headers)
        print("OK:", reply.ok)
        print("Status:", reply.status_code)
        print("Content:", reply.text)
    except requests.exceptions.ConnectionError as exception:
        print("Netwerkfout:", exception)


get_and_print_reply("/user/koen")
~~~

En dat ziet er dan als volgt uit:

~~~
$ python client.py
Headers: {'Content-Type': 'text/html; charset=utf-8', 'Content-Length': '10', 'Server': 'Werkzeug/2.0.2 Python/3.9.7', 'Date': 'Wed, 25 May 2022 13:14:28 GMT'}
OK: True
Status: 200
Content: Hello koen
~~~

We kunnen ook parameters op meerdere niveaus introduceren:

~~~python
from flask import Flask

app = Flask(__name__)


@app.route("/")
def hello_world():
    return "Hello, World!"


@app.route("/test")
def hello_school():
    return "Hello, Mars!"


@app.route("/user/<firstname>/<lastname>")
def profile(firstname, lastname="test"):
    return f"Hello {firstname} {lastname}"


if __name__ == "__main__":
    app.run(port=5000, debug=True)
~~~

We gebruiken dit als volgt in de client:

~~~python
import requests

BASE_URI = "http://localhost:5000"


def get_and_print_reply(uri):
    try:
        reply = requests.get(BASE_URI + uri)
        print("Headers:", reply.headers)
        print("OK:", reply.ok)
        print("Status:", reply.status_code)
        print("Content:", reply.text)
    except requests.exceptions.ConnectionError as exception:
        print("Netwerkfout:", exception)


get_and_print_reply("/user/koen/vervloesem")
~~~

En dit uitvoeren geeft dan:

~~~
$ python client.py
Headers: {'Server': 'Werkzeug/2.2.3 Python/3.10.6', 'Date': 'Sun, 16 Apr 2023 09:24:26 GMT', 'Content-Type': 'text/html; charset=utf-8', 'Content-Length': '21', 'Connection': 'close'}
OK: True
Status: 200
Content: Hello koen vervloesem
~~~

Merk op: `/user/koen/vervloesem/` is een andere url dan `/user/koen/vervloesem`. Als je die eerste bezoekt in je clientprogramma, zul je dan ook een 404-foutmelding krijgen.

### Voorbeeld voor studentendatabase

Met deze kennis kunnen we nu een web-API maken voor onze studentendatabase. We hergebruiken daarvoor de modules `student_entities` en `student_service` uit het deel over databases. Onze web-API biedt dus een op Flask gebaseerde vervanging voor onze eerdere commandline-interface in `student_command`.

Stel dat je een REST-API gaat maken om een school te beheren. Je wilt:

* klassen beheren
* studenten aan deze klassen toevoegen en bewerken

Hiërarchisch zien je entiteiten er als volgt uit:

~~~
groep: 1
    student: 1
    student: 2
~~~

De code ziet er als volgt uit:

~~~python
from flask import Flask, request, jsonify, redirect
from student_entities import Student
from student_service import *

app = Flask(__name__)


@app.route("/")
def index():
    return redirect("/groups")


def group_to_json(group):
    return group.__dict__


def student_to_json(student):
    return student.__dict__


@app.route("/groups", methods=["GET"])
def groups():
    groups = list(map(group_to_json, get_groups()))
    return jsonify(groups)


@app.route("/groups", methods=["POST"])
def group_post():
    group_name = request.json["name"]
    teacher = request.json["teacher"]
    room = request.json["room"]
    save_new_group(group_name, teacher, room)
    return jsonify(group_to_json(get_group(group_name)))


@app.route("/groups/<groupname>")
def group(groupname):
    return group_to_json(get_group(groupname))


@app.route("/groups/<groupname>/students", methods=["GET"])
def students(groupname):
    students = get_students_for_group(groupname)
    return jsonify(list(map(student_to_json, students)))


@app.route("/groups/<groupname>/students/<studentid>", methods=["GET"])
def student_get(groupname, studentid):
    return jsonify(student_to_json(get_student(studentid)))


@app.route("/groups/<groupname>/students", methods=["POST"])
def student_post(groupname):
    name = request.json["name"]
    lab = int(request.json["lab_points"])
    theory = int(request.json["theory_points"])
    student = Student(name, lab, theory)
    save_new_student(student, groupname)
    return jsonify(student_to_json(student))


if __name__ == "__main__":
    app.run()
~~~

Opmerking: je moet de module `student_service.py` nog uitbreiden met een functie `get_group`. Dat laten we als oefening voor je.

### Client-code

We hebben nu een server die een REST-API voor studenten implementeert.

Laten we kijken hoe we deze zouden gebruiken in een client. Je kunt dit eerst uitproberen in Insomnia, en daarna met `requests` in Python.

Stel dat je een overzicht wilt krijgen van de klasgroepen. Dan ga je naar het endpoint `groups`.

Zo'n endpoint is het pad relatief ten opzichte van je host/domeinnaam.

In de url hieronder:

* http => protocol
* localhost => domein- of servernaam
* 5000 => poort waarop je app luistert
* groups => alles wat er op volgt

We starten bijvoorbeeld met alle groepen op te lijsten door de url http://localhost:5000/groups te bezoeken. Die geeft initieel een lege lijst terug:

~~~
[]
~~~

Hoe kunnen we nu een groep toevoegen? Daarvoor roepen we het endpoint `/groups` aan met een POST-methode en een JSON-string met de volgende inhoud:

~~~json
{"name": "1", "room": "D114", "teacher": "BV"}
~~~

Dat kan in Insomnia door vlak voor de adresbalk in plaats van **GET** voor **POST** te kiezen, bij **Body** voor **JSON** te kiezen en de JSON-string in te voeren. Druk dan op **Send**.

Voeg nu op dezelfde manier een tweede groep toe:

~~~json
{"name": "hello", "room": "D118", "teacher": "Jos"}
~~~

Vraag nu nog eens met een GET-aanroep de groepen op (vergeet niet de methode naar **GET** te veranderen en de body leeg te maken). Dan krijg je deze keer geen lege lijst, maar:

~~~json
[
	{
		"name": "1",
		"room": "D114",
		"teacher": "BV"
	},
	{
		"name": "hello",
		"room": "D118",
		"teacher": "Jos"
	}
]
~~~

We zien hier twee groepen: 1 en hello.  
Als we alleen in groep 1 geïnteresseerd zijn, kunnen we die opvragen op de url http://localhost:5000/groups/1.

Met als resultaat:

~~~json
{
	"name": "1",
	"room": "D114",
	"teacher": "BV"
}
~~~

Studenten vallen in onze school onder een groep.  
Als je alle studenten wilt zien uit groep 1, schrijf je de naam van de entiteit (students) achter het voorgaande pad: http://localhost:5000/groups/1/students.

Dat geeft voorlopig een lege lijst terug:

~~~json
[]
~~~

We moeten de groepen dus vullen met studenten. Dat kan voor groep 1 door het endpoint `/groups/1/students` aan te roepen met een POST-methode. Geef dit de volgende JSON-string als body:

~~~json
{"name": "aa", "lab_points": 10, "theory_points": 20}
~~~

En doe nu hetzelfde voor nog enkele andere studenten.

Als je daarna alle studenten van groep 2 opvraagt, krijg je iets als:

Met als resultaat:

~~~json
[
	{
		"lab_points": 10,
		"name": "aa",
		"student_id": 1,
		"theory_points": 20
	},
	{
		"lab_points": 10,
		"name": "bb",
		"student_id": 2,
		"theory_points": 15
	},
	{
		"lab_points": 10,
		"name": "cc",
		"student_id": 3,
		"theory_points": 15
	},
	{
		"lab_points": 12,
		"name": "dd",
		"student_id": 4,
		"theory_points": 15
	},
	{
		"lab_points": 15,
		"name": "ee",
		"student_id": 5,
		"theory_points": 16
	},
	{
		"lab_points": 12,
		"name": "ff",
		"student_id": 6,
		"theory_points": 20
	}
]
~~~

Als je alleen student met ID 2 wilt zien, kun je dat opnieuw filteren aan de hand van het ID: http://localhost:5000/groups/1/students/2.

Met als resultaat:

~~~json
{
	"lab_points": 10,
	"name": "bb",
	"student_id": 2,
	"theory_points": 15
}
~~~

We kunnen dit alles ook in Python doen met de module `requests` Voeg bijvoorbeeld een student toe aan groep 1 en vraag dan alle studenten van groep 1 op:

~~~python
>>> import requests
>>> import json
>>> response = requests.post("http://localhost:5000/groups/1/students", json={"lab_points":12,"name":"gg","theory_points":15})
>>> response.ok
True
>>> response = requests.get("http://localhost:5000/groups/1/students").json()
>>> print(json.dumps(response, indent=2))
[
  {
    "lab_points": 10,
    "name": "aa",
    "student_id": 1,
    "theory_points": 20
  },
  {
    "lab_points": 10,
    "name": "bb",
    "student_id": 2,
    "theory_points": 15
  },
  {
    "lab_points": 10,
    "name": "cc",
    "student_id": 3,
    "theory_points": 15
  },
  {
    "lab_points": 12,
    "name": "dd",
    "student_id": 4,
    "theory_points": 15
  },
  {
    "lab_points": 15,
    "name": "ee",
    "student_id": 5,
    "theory_points": 16
  },
  {
    "lab_points": 12,
    "name": "ff",
    "student_id": 6,
    "theory_points": 20
  },
  {
    "lab_points": 12,
    "name": "gg",
    "student_id": 7,
    "theory_points": 15
  }
]
~~~

De module `json` bevat enkele nuttige functies om met JSON-strings te werken. Zo kun je met `json.dumps` zoals je ziet een JSON-string met duidelijke witruimte weergeven. Meer informatie over de module vind je op https://docs.python.org/3/library/json.html.
