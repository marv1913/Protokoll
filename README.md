# Protokoll
Spezifikation für unser Protokoll im Kurs TmS


### Netzwerkstruktur
![Netzwerk.jpeg](Netzwerk.jpeg)

### Nachrichtenversand

#### Route Request (Flag=3)
Knoten1 will an Knoten6 eine Nachricht schicken\

Dafür sendet Kn1 an Kn2 und Kn3 ein Route Request in folgendem Format:\

RouteRequest Beispiel fur Kn1:\
Source = Kn1\
Dest = Kn2\
Flag = 3,\
T = TTL=10\
EKn = Kn6\
Ursp = Kn1\
H = Hops=0\

+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-|\
| Source|  Dest |F|T| EKn   |  Ursp |H|\
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+\

RouteRequest Beispiel fur Kn5:\
Source = Kn2\
Dest = Kn5\
Flag = 3,\
T = TTL=9\
EKn = Kn6\
Ursp = Kn1\
H = Hops=2\

+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-|\
| Source|  Dest |F|T| EKn   |  Ursp |H|\
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+\

#### Route Reply wäre unser ACK Paket (Flag=4)
Knoten6 schickt an Knoten1 ein Route Reply zurück.\

RouteReply Beispiel fur Kn6:\
Source = Kn6\
Dest = Kn5\
Flag = 4\
T = TTL=10\
EKn = Kn1\
Ursp = Kn6\
H = Hops=2\

+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-|\
| Source|  Dest |F|T| EKn   |  Ursp |H|\
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+\

Kn2 Kann dann Kn6 in Tabelle eintragen, da Kn2 jetzt weiss irgendwo ist Kn6 und ihn kann ich über Kn5 erreichen.\

#### Route Error

Wenn Knoten2 nach 3 Route Requests an Knoten5 kein Route Reply bekommt wird Knoten5 aus der Tabelle gelöscht.\
