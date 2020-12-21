
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
| source | destination |flag|ttl|EKn|Ursp|hops

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
| source | destination |flag|ttl|EKn|Ursp|hops

Kn2 Kann dann Kn6 in Tabelle eintragen, da Kn2 jetzt weiss irgendwo ist Kn6 und ihn kann ich über Kn5 erreichen.\

#### Route Error (Flag=5)

Wenn Knoten2 nach 3 Route Requests an Knoten5 kein Route Reply bekommt wird Knoten5 aus der Tabelle gelöscht.\

#### Route Tabelle
Die Tabelle die jeder Knoten verwaltet müsste somit folgende Einträge haben:
* Adresse des Zielknotens  
* Adresse des nächsten Hops (Nachbarknoten)   
* Hops (Qualität der Route) 
 
 
