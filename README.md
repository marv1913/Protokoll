
# Protokoll
Spezifikation für unser Protokoll im Kurs TmS


### Netzwerkstruktur
![Netzwerk.jpeg](Netzwerk.jpeg)


### Nachrichtenversand
#### Route Request (Flag=3)
Knoten1 will an Knoten6 eine Nachricht schicken\

Dafür sendet Kn1 eine Route-Request als Broadcast-Nachricht an alle benachbarten Knoten in folgendem Format:\

RouteRequest Beispiel fur Kn1:\
Source = Kn1\
Dest = Kn2\
F = Flag = 3,\
T = TTL=10\
Empfangsknoten = Kn6\
H = Hops=0\

|  |  |  |  |  |  |  |
|--|--|--|--|--|--|--|
| source | destination |flag|ttl|Empfangsknoten|hops

RouteRequest Beispiel fur Kn5:\
source = Kn1\
destination = Kn5\
flag = 3,\
ttl = 9\
Empfangsknoten = Kn6\
hops = 2

|  |  |  |  |  |  |  |
|--|--|--|--|--|--|--|
| source | destination |flag|ttl|EKn|Ursp|hops


#### Route Reply wäre unser ACK Paket (Flag=4)
Knoten6 schickt an Knoten1 ein Route Reply zurück.\

RouteReply Beispiel fur Kn6:\
source = Kn6\
destination = Kn5\
flag = 4\
ttl=10\
Empfangsknoten = Kn1\
hops = 2

|  |  |  |  |  |  |  |
|--|--|--|--|--|--|--|
| source | destination |flag|ttl|Empfangsknoten|hops

Kn2 Kann dann Kn6 in Tabelle eintragen, da Kn2 jetzt weiss irgendwo ist Kn6 und ihn kann ich über Kn5 erreichen.\

#### Route Error (Flag=5)

Wenn Knoten2 nach 3 Route Requests an Knoten5 kein Route Reply bekommt wird Knoten5 aus der Tabelle gelöscht.\

#### Route Tabelle
Die Tabelle die jeder Knoten verwaltet müsste somit folgende Einträge haben:
* Adresse des Zielknotens  
* Adresse des nächsten Hops (Nachbarknoten)   
* Hops (Qualität der Route) 

#### Nachrichtenweiterleitung (flag=1, TTL=10)
Davon ausgehend das Kn1 Kn6 in seiner Tabelle als aktiven Knoten hat (vorheriger Route Reply kam erfolgreich bei Kn1 an). Möchte Kn1(0131) an Kn6(0136) eine Textnachricht schicken.

Nachrichtenweiterleitung am Beispiel für Kn2:
source = Kn1(0131)
destination = Kn6(0136)
flag = 1
ttl = 10
textnachricht = payload


|  |  |  |  |  |  | 
|--|--|--|--|--|--|
| source | destination |flag|ttl|textnachricht


Kn1 baut Nachricht auf:
AT+DEST=0136, AT+SEND=16, Hallo wie gehts?

Kn2(0132) empfängt Nachricht von 0131 und parst header:
LR,0131,1B,01310136110Hallo wie gehts?
Kn2 sieht flag=1 und Destination != 0132, also Textnachricht ist nicht für mich, daraufhin schaut Kn2 in seiner Tabelle nach Adresse eines erreichbaren Knoten und findet Kn5(0135). (darüber lief ja auch der vorherige Route Reply deshalb ist 0135 in Tabelle).
Kn2 baut Nachrichtenweiterleitung auf:
AT+DEST=0135, AT+SEND=16, Hallo wie gehts? 

Kn5 empfängt Nachricht von Kn2 und parst header:
LR,0132,1B,0131013819Hallo wie gehts?
Kn5 sieht flag=1 und Destination != 0135, also Textnachricht ist nicht für mich, daraufhin schaut Kn5 in seiner Tabelle nach Adresse eines erreichbaren Knoten und findet Kn6(0136).
Kn5 baut Nachrichtenweiterleitung auf:
AT+DEST=0136, AT+SEND=16, Hallo wie gehts? 

Kn6 empfängt Nachricht von Kn5 und parst header:
LR,0132,1B,0131013819Hallo wie gehts?
Kn6 sieht flag=1 und Destination == 0136 also Textnachricht ist für mich.
Hallo wie gehts? wird am Bildschirm ausgegeben.