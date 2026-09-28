# Übung 02 - Standortvernetzung von zwei Standorten

**Klasse:** 4AHITS  
**Verfasser:** Denis Velic  
**Übungsdatum:**  
**Abgabedatum:**  

---

## 1. Aufgabenstellung

Ziel dieser Laborübung war es, zwei voneinander getrennte LAN-Netzwerke über eine WAN-Verbindung miteinander zu verbinden.

Dabei wurden die Standorte **Braunau** und **Schärding** jeweils mit einem Router und einem Switch ausgestattet.

Die beiden Router wurden über das Netzwerk `10.0.0.0/30` miteinander verbunden.

Nach der Konfiguration sollten alle Netzwerkgeräte untereinander erreichbar sein. Zusätzlich mussten statische Routen eingerichtet werden, damit die Router auch die jeweils entfernten LAN-Netzwerke erreichen können.

---

## 2. Topologie

Die verwendete Netzwerktopologie besteht aus folgenden Geräten:

- 2 × Cisco 2811 Router
- 2 × Cisco Catalyst 2960 Switch
- 2 × PCs

Die logische Struktur des Netzwerkes ist:

```text
                           WAN 10.0.0.0/30
                  10.0.0.1              10.0.0.2
                      │                      │
PC-A ── S-Br1 ── R-Br1 ───────────────── R-Sd1 ── S-Sd1 ── PC-B
        Braunau                                  Schärding

LAN Braunau:     192.168.1.0/24
LAN Schärding:   192.168.2.0/24
```

---

## 3. IP-Adressierung

Für die beiden lokalen Netzwerke wurde jeweils ein `/24`-Netz verwendet.

Ein `/24` besitzt die Subnetzmaske:

```text
255.255.255.0
```

Für die direkte Verbindung zwischen den beiden Routern wurde ein `/30`-Netz verwendet.

```text
/30 = 255.255.255.252
```

Ein `/30`-Netz eignet sich für eine Punkt-zu-Punkt-Verbindung zwischen zwei Routern, da zwei verwendbare Hostadressen zur Verfügung stehen.

### Adresstabelle

| Gerät | Interface | IP-Adresse | Subnetzmaske | Default Gateway |
|---|---|---|---|---|
| R-Br1 | Fa0/0 | 192.168.1.1 | 255.255.255.0 | - |
| R-Br1 | Fa0/1 | 10.0.0.1 | 255.255.255.252 | - |
| R-Sd1 | Fa0/0 | 192.168.2.1 | 255.255.255.0 | - |
| R-Sd1 | Fa0/1 | 10.0.0.2 | 255.255.255.252 | - |
| S-Br1 | VLAN 1 | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 |
| S-Sd1 | VLAN 1 | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 |
| PC-A | NIC | 192.168.1.111 | 255.255.255.0 | 192.168.1.1 |
| PC-B | NIC | 192.168.2.111 | 255.255.255.0 | 192.168.2.1 |

### WAN-Netzwerk

Für die Verbindung zwischen den Routern wurde das Netzwerk `10.0.0.0/30` verwendet.

| Adresse | Funktion |
|---|---|
| 10.0.0.0 | Netzwerkadresse |
| 10.0.0.1 | R-Br1 |
| 10.0.0.2 | R-Sd1 |
| 10.0.0.3 | Broadcastadresse |

---

## 4. Grundkonfiguration

Auf allen Cisco-Geräten wurden zunächst die geforderten Passwörter konfiguriert.

Das Console- und VTY-Passwort lautet:

```text
cisco
```

Das Enable-Secret lautet:

```text
class
```

Beispiel:

```cisco
enable
configure terminal

enable secret class

line console 0
 password cisco
 login
 exit

line vty 0 4
 password cisco
 login
 exit
```

Mit `enable secret` wird das Passwort für den privilegierten EXEC-Modus festgelegt.

Die Konfiguration unter `line console 0` schützt den lokalen Konsolenzugang.

Die VTY-Lines werden für Remote-Zugriffe auf das Gerät verwendet.

---

## 5. Konfiguration Router R-Br1

Zunächst wurde der Hostname des Routers gesetzt.

```cisco
enable
configure terminal
hostname R-Br1
```

Danach wurden die benötigten Passwörter eingerichtet.

```cisco
enable secret class

line console 0
 password cisco
 login
 exit

line vty 0 4
 password cisco
 login
 exit
```

### LAN-Interface

Das Interface FastEthernet0/0 verbindet den Router mit dem LAN am Standort Braunau.

```cisco
interface FastEthernet0/0
 description LAN-Braunau
 ip address 192.168.1.1 255.255.255.0
 no shutdown
 exit
```

Mit dem Befehl

```cisco
no shutdown
```

wird das Interface aktiviert.

### WAN-Interface

Das Interface FastEthernet0/1 verbindet R-Br1 mit R-Sd1.

```cisco
interface FastEthernet0/1
 description WAN-zu-R-Sd1
 ip address 10.0.0.1 255.255.255.252
 no shutdown
 exit
```

---

## 6. Konfiguration Router R-Sd1

Der zweite Router wurde entsprechend konfiguriert.

```cisco
enable
configure terminal

hostname R-Sd1

enable secret class

line console 0
 password cisco
 login
 exit

line vty 0 4
 password cisco
 login
 exit
```

### LAN-Interface

```cisco
interface FastEthernet0/0
 description LAN-Schaerding
 ip address 192.168.2.1 255.255.255.0
 no shutdown
 exit
```

### WAN-Interface

```cisco
interface FastEthernet0/1
 description WAN-zu-R-Br1
 ip address 10.0.0.2 255.255.255.252
 no shutdown
 exit
```

---

## 7. Konfiguration der Switches

Die Switches benötigen eine Management-IP-Adresse.

Diese wurde jeweils auf dem virtuellen Interface VLAN 1 eingerichtet.

### S-Br1

```cisco
enable
configure terminal

hostname S-Br1

enable secret class

line console 0
 password cisco
 login
 exit

line vty 0 4
 password cisco
 login
 exit

interface vlan 1
 ip address 192.168.1.2 255.255.255.0
 no shutdown
 exit

ip default-gateway 192.168.1.1

end
```

Das Default Gateway ist `192.168.1.1`, da dies die Adresse des Routers im LAN Braunau ist.

### S-Sd1

```cisco
enable
configure terminal

hostname S-Sd1

enable secret class

line console 0
 password cisco
 login
 exit

line vty 0 4
 password cisco
 login
 exit

interface vlan 1
 ip address 192.168.2.2 255.255.255.0
 no shutdown
 exit

ip default-gateway 192.168.2.1

end
```

Das Default Gateway des Switches ist der Router R-Sd1 mit der Adresse `192.168.2.1`.

---

## 8. Konfiguration der PCs

### PC-A

Auf PC-A wurde folgende IPv4-Konfiguration vorgenommen:

```text
IP-Adresse:       192.168.1.111
Subnetzmaske:     255.255.255.0
Default Gateway:  192.168.1.1
```

### PC-B

PC-B wurde folgendermaßen konfiguriert:

```text
IP-Adresse:       192.168.2.111
Subnetzmaske:     255.255.255.0
Default Gateway:  192.168.2.1
```

Die Default Gateways sind notwendig, damit die PCs Pakete an Geräte außerhalb ihres eigenen lokalen Netzwerkes senden können.

---

## 9. Statisches Routing

Nach der Konfiguration der Interfaces kennen die Router zunächst nur ihre direkt angeschlossenen Netzwerke.

R-Br1 kennt:

```text
192.168.1.0/24
10.0.0.0/30
```

R-Sd1 kennt:

```text
192.168.2.0/24
10.0.0.0/30
```

Damit die beiden LANs miteinander kommunizieren können, müssen statische Routen eingerichtet werden.

### Route auf R-Br1

R-Br1 benötigt eine Route zum Netzwerk Schärding:

```cisco
ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

Der Befehl bedeutet, dass Pakete für das Netzwerk `192.168.2.0/24` an den Next-Hop `10.0.0.2` weitergeleitet werden.

`10.0.0.2` ist die WAN-Adresse von R-Sd1.

### Route auf R-Sd1

R-Sd1 benötigt eine Route zum Netzwerk Braunau:

```cisco
ip route 192.168.1.0 255.255.255.0 10.0.0.1
```

Der Next-Hop `10.0.0.1` ist die WAN-Adresse von R-Br1.

### Übersicht

| Router | Zielnetz | Subnetzmaske | Next-Hop |
|---|---|---|---|
| R-Br1 | 192.168.2.0 | 255.255.255.0 | 10.0.0.2 |
| R-Sd1 | 192.168.1.0 | 255.255.255.0 | 10.0.0.1 |

Durch diese beiden Routen besitzen die Router nun einen Pfad zu allen Netzwerken der Topologie.

---

## 10. Kontrolle der Interfaces

Zur Überprüfung der Routerinterfaces wurde folgender Befehl verwendet:

```cisco
show ip interface brief
```

Auf R-Br1 sollten unter anderem folgende Interfaces vorhanden sein:

```text
FastEthernet0/0   192.168.1.1   up   up
FastEthernet0/1   10.0.0.1      up   up
```

Auf R-Sd1:

```text
FastEthernet0/0   192.168.2.1   up   up
FastEthernet0/1   10.0.0.2      up   up
```

Der Zustand `up/up` zeigt, dass das Interface sowohl physisch als auch auf Protokollebene aktiv ist.

Falls ein Interface als `administratively down` angezeigt wird, kann es mit

```cisco
no shutdown
```

aktiviert werden.

---

## 11. Kontrolle der Routingtabellen

Die Routingtabelle wurde mit folgendem Befehl kontrolliert:

```cisco
show ip route
```

### R-Br1

Es sollten unter anderem folgende Netzwerke vorhanden sein:

```text
C    192.168.1.0/24 is directly connected
C    10.0.0.0/30 is directly connected
S    192.168.2.0/24 [1/0] via 10.0.0.2
```

### R-Sd1

```text
C    192.168.2.0/24 is directly connected
C    10.0.0.0/30 is directly connected
S    192.168.1.0/24 [1/0] via 10.0.0.1
```

Die Buchstaben am Anfang eines Routingeintrages geben an, wie die Route gelernt wurde.

```text
C = Connected
S = Static
```

---

## 12. Test der End-to-End-Konnektivität

Nach Abschluss der Konfiguration wurde die Verbindung zwischen den Geräten mit `ping` überprüft.

Die Tests wurden von PC-A durchgeführt.

### Router Braunau

```cmd
ping 192.168.1.1
```

**Ergebnis:** erfolgreich

### Switch Braunau

```cmd
ping 192.168.1.2
```

**Ergebnis:** erfolgreich

### Router Schärding

```cmd
ping 10.0.0.2
```

**Ergebnis:** erfolgreich

### Switch Schärding

```cmd
ping 192.168.2.2
```

**Ergebnis:** erfolgreich

### PC-B

```cmd
ping 192.168.2.111
```

**Ergebnis:** erfolgreich

Damit konnte bestätigt werden, dass eine vollständige End-to-End-Verbindung zwischen den beiden Standorten vorhanden ist.

> **Screenshot:** Hier Screenshot des erfolgreichen Pings von PC-A zu PC-B einfügen.

---

## 13. Speichern der Konfiguration

Damit die Routerkonfigurationen auch nach einem Neustart erhalten bleiben, wurden sie vom Running-Config in den Startup-Config gespeichert.

Auf R-Br1 und R-Sd1 wurde folgender Befehl verwendet:

```cisco
copy running-config startup-config
```

Bei der Frage

```text
Destination filename [startup-config]?
```

wurde mit Enter bestätigt.

Die gespeicherte Konfiguration kann anschließend mit

```cisco
show startup-config
```

kontrolliert werden.

---

## 14. Routerkonfigurationen

### R-Br1

```cisco
hostname R-Br1

enable secret class

interface FastEthernet0/0
 description LAN-Braunau
 ip address 192.168.1.1 255.255.255.0
 no shutdown

interface FastEthernet0/1
 description WAN-zu-R-Sd1
 ip address 10.0.0.1 255.255.255.252
 no shutdown

ip route 192.168.2.0 255.255.255.0 10.0.0.2

line console 0
 password cisco
 login

line vty 0 4
 password cisco
 login
```

### R-Sd1

```cisco
hostname R-Sd1

enable secret class

interface FastEthernet0/0
 description LAN-Schaerding
 ip address 192.168.2.1 255.255.255.0
 no shutdown

interface FastEthernet0/1
 description WAN-zu-R-Br1
 ip address 10.0.0.2 255.255.255.252
 no shutdown

ip route 192.168.1.0 255.255.255.0 10.0.0.1

line console 0
 password cisco
 login

line vty 0 4
 password cisco
 login
```

---

## 15. Fazit

In dieser Übung wurden zwei getrennte IPv4-Netzwerke über eine WAN-Verbindung miteinander verbunden.

Zunächst wurden die Router-, Switch- und PC-Interfaces mit den entsprechenden IPv4-Adressen konfiguriert. Anschließend wurden auf beiden Routern statische Routen eingerichtet.

Die statischen Routen waren notwendig, da jeder Router ohne Routinginformationen nur seine direkt angeschlossenen Netzwerke kennt.

Durch die Route

```cisco
ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

kann R-Br1 das Netzwerk in Schärding erreichen.

Durch

```cisco
ip route 192.168.1.0 255.255.255.0 10.0.0.1
```

kann R-Sd1 das Netzwerk in Braunau erreichen.

Abschließend wurde die Konnektivität mit mehreren Ping-Tests überprüft. Die Tests zwischen PC-A und PC-B sowie den dazwischenliegenden Netzwerkgeräten waren erfolgreich.

Somit konnte die Standortvernetzung erfolgreich hergestellt werden.
