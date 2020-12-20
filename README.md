# Protokoll
Spezifikation für unser Protokoll im Kurs TmS


### Netzwerkstruktur
![Netzwerk.jpeg](Netzwerk.jpeg)

### Nachrichtenversand

#### Route Request (Flag=3)
Knoten1 will an Knoten6 eine Nachricht schicken<br\>

Dafür sendet Kn1 an Kn2 und Kn3 ein Route Request in folgendem Format:<br\>

RouteRequest Beispiel fur Kn1:<br\>
Source = Kn1<br\>
Dest = Kn2<br\>
Flag = 3,<br\>
T = TTL=10<br\>
EKn = Kn6<br\>
Ursp = Kn1<br\>
H = Hops=0<br\>

+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-|<br\>
| Source|  Dest |F|T| EKn   |  Ursp |H|<br\>
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+<br\>

RouteRequest Beispiel fur Kn5:<br\>
Source = Kn2<br\>
Dest = Kn5<br\>
Flag = 3,<br\>
T = TTL=9<br\>
EKn = Kn6<br\>
Ursp = Kn1<br\>
H = Hops=2<br\>

+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-|<br\>
| Source|  Dest |F|T| EKn   |  Ursp |H|<br\>
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+<br\>

#### Route Reply wäre unser ACK Paket (Flag=4)
Knoten6 schickt an Knoten1 ein Route Reply zurück.<br\>

RouteReply Beispiel fur Kn6:<br\>
Source = Kn6<br\>
Dest = Kn5<br\>
Flag = 4<br\>
T = TTL=10<br\>
EKn = Kn1<br\>
Ursp = Kn6<br\>
H = Hops=2<br\>

+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-|<br\>
| Source|  Dest |F|T| EKn   |  Ursp |H|<br\>
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+<br\>

Kn2 Kann dann Kn6 in Tabelle eintragen, da Kn2 jetzt weiss irgendwo ist Kn6 und ihn kann ich über Kn5 erreichen.<br\>

#### Route Error

Wenn Knoten2 nach 3 Route Requests an Knoten5 kein Route Reply bekommt wird Knoten5 aus der Tabelle gelöscht.<br\>
