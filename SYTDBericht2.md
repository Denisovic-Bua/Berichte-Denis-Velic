# Arbeitsbericht – Übung 02 Standortvernetzung

**Klasse:** 4AHITS  
**Thema:** Standortvernetzung von 2 Standorten  
**Verfasser:** Denis Velic  
**Datum:** 28.09.2026


---

## 1. Aufgabenstellung

In dieser Übung war die Aufgabe, zwei verschiedene Standorte über zwei Router miteinander zu verbinden.

Die beiden Standorte sind:

- Braunau
- Schärding

An jedem Standort gibt es jeweils einen PC, einen Switch und einen Router.  
Die beiden Router werden direkt miteinander verbunden.

Das Ziel war, dass sich am Ende alle Geräte untereinander erreichen können und besonders PC-A zu PC-B pingen kann.

Die verwendete Topologie sieht ungefähr so aus:

```text
PC-A --- S-Br1 --- R-Br1 ===== R-Sd1 --- S-Sd1 --- PC-B
        Braunau                    Schärding
```

--- 

## 2. IP-Adressierung

Für beide Standorte wurden unterschiedliche Netzwerke verwendet.

Braunau verwendet das Netz:

```text
192.168.1.0/24
```

Schärding verwendet:

```text
192.168.2.0/24
```

Für die Verbindung zwischen den beiden Routern wurde das Netz

```text
10.0.0.0/30
```

verwendet.

### Adresstabelle

| Gerät | Interface | IP-Adresse | Subnetzmaske | Gateway |
|---|---|---|---|---|
| R-Br1 | Fa0/0 | 192.168.1.1 | 255.255.255.0 | - |
| R-Br1 | Fa0/1 | 10.0.0.1 | 255.255.255.252 | - |
| R-Sd1 | Fa0/0 | 192.168.2.1 | 255.255.255.0 | - |
| R-Sd1 | Fa0/1 | 10.0.0.2 | 255.255.255.252 | - |
| S-Br1 | VLAN 1 | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 |
| S-Sd1 | VLAN 1 | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 |
| PC-A | NIC | 192.168.1.111 | 255.255.255.0 | 192.168.1.1 |
| PC-B | NIC | 192.168.2.111 | 255.255.255.0 | 192.168.2.1 |

---

## 3. Verkabelung

Zuerst wurden alle Geräte wie in der Topologie miteinander verbunden.

Die Verbindung war:

```text
PC-A -> S-Br1 -> R-Br1 -> R-Sd1 -> S-Sd1 -> PC-B
```

Nach dem Verbinden waren manche Ports zuerst noch rot, weil die Router Interfaces standardmäßig deaktiviert sind.

Diese wurden später mit

```cisco
no shutdown
```

aktiviert.

---

## 4. Konfiguration von PC-A

PC-A wurde mit folgender IP-Adresse konfiguriert:

```text
IP-Adresse:       192.168.1.111
Subnetzmaske:     255.255.255.0
Default Gateway:  192.168.1.1
```

Der Default Gateway ist die IP-Adresse vom Router R-Br1 auf der LAN Seite.

---

## 5. Konfiguration von PC-B

PC-B wurde wie folgt konfiguriert:

```text
IP-Adresse:       192.168.2.111
Subnetzmaske:     255.255.255.0
Default Gateway:  192.168.2.1
```

Auch hier ist das Gateway die IP-Adresse vom lokalen Router.

---

## 6. Konfiguration R-Br1

Zuerst wurde der Router in den privilegierten Modus versetzt.

```cisco
enable
configure terminal
```

Danach wurde der Hostname gesetzt:

```cisco
hostname R-Br1
```

Anschließend wurde das Enable Passwort konfiguriert:

```cisco
enable secret class
```

Für Console und VTY wurde das Passwort `cisco` verwendet.

```cisco
line console 0
password cisco
login
exit
```

```cisco
line vty 0 4
password cisco
login
exit
```

### Interface Fa0/0

Das Interface Fa0/0 verbindet den Router mit dem LAN in Braunau.

```cisco
interface FastEthernet0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
```

### Interface Fa0/1

Fa0/1 verbindet R-Br1 mit R-Sd1.

```cisco
interface FastEthernet0/1
ip address 10.0.0.1 255.255.255.252
no shutdown
exit
```

---

## 7. Konfiguration R-Sd1

Beim zweiten Router wurde fast das gleiche gemacht.

```cisco
enable
configure terminal
hostname R-Sd1
```

Danach wieder das Enable Passwort:

```cisco
enable secret class
```

Console Passwort:

```cisco
line console 0
password cisco
login
exit
```

VTY Passwort:

```cisco
line vty 0 4
password cisco
login
exit
```

### Interface Fa0/0

```cisco
interface FastEthernet0/0
ip address 192.168.2.1 255.255.255.0
no shutdown
exit
```

### Interface Fa0/1

```cisco
interface FastEthernet0/1
ip address 10.0.0.2 255.255.255.252
no shutdown
exit
```

Danach wurde mit

```cisco
show ip interface brief
```

überprüft ob die Interfaces aktiv sind.

Es sollte ungefähr so aussehen:

```text
FastEthernet0/0   192.168.2.1   up   up
FastEthernet0/1   10.0.0.2      up   up
```

Wenn `up up` angezeigt wird, ist das Interface aktiv und die Verbindung sollte grundsätzlich funktionieren.

---

## 8. Statische Routen

Da die beiden Router jeweils nur ihre direkt verbundenen Netzwerke kennen, mussten statische Routen gesetzt werden.

R-Br1 kennt sonst nur:

```text
192.168.1.0/24
10.0.0.0/30
```

R-Sd1 kennt:

```text
192.168.2.0/24
10.0.0.0/30
```

Damit R-Br1 weiß, wie er nach Schärding kommt, wurde folgende Route gesetzt:

```cisco
ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

Das bedeutet, Pakete für das Netz `192.168.2.0/24` werden an `10.0.0.2` weitergeleitet.

Auf R-Sd1 wurde die Rückroute gesetzt:

```cisco
ip route 192.168.1.0 255.255.255.0 10.0.0.1
```

Diese Route ist wichtig, da Pakete auch wieder zurück zu PC-A kommen müssen.

---

## 9. Kontrolle der Routing Tabelle

Mit folgendem Befehl wurde die Routing Tabelle angezeigt:

```cisco
show ip route
```

Auf R-Br1 sollte unter anderem folgende Route vorhanden sein:

```text
S    192.168.2.0/24 via 10.0.0.2
```

Auf R-Sd1 sollte stehen:

```text
S    192.168.1.0/24 via 10.0.0.1
```

Das `S` steht für eine statische Route.

---

## 10. Konfiguration S-Br1

Der Switch in Braunau bekam eine Management IP auf VLAN 1.

```cisco
enable
configure terminal

hostname S-Br1

interface vlan 1
ip address 192.168.1.2 255.255.255.0
no shutdown
exit
```

Danach wurde noch der Default Gateway eingestellt:

```cisco
ip default-gateway 192.168.1.1
```

Der Switch braucht das Gateway, wenn er mit Geräten aus einem anderen Netzwerk kommunizieren soll.

---

## 11. Konfiguration S-Sd1

Beim Switch in Schärding wurde folgendes konfiguriert:

```cisco
enable
configure terminal

hostname S-Sd1

interface vlan 1
ip address 192.168.2.2 255.255.255.0
no shutdown
exit
```

Danach:

```cisco
ip default-gateway 192.168.2.1
```

---

## 12. Test der Verbindung

Nach der Konfiguration wurde die Verbindung zwischen den Geräten mit `ping` getestet.

Von PC-A wurden nacheinander mehrere Geräte angepingt.

### Test zu R-Br1

```cmd
ping 192.168.1.1
```

Der Ping war erfolgreich.

### Test zu S-Br1

```cmd
ping 192.168.1.2
```

Der Ping war ebenfalls erfolgreich.

### Test zum zweiten Router

```cmd
ping 10.0.0.2
```

Damit wurde überprüft, ob die Verbindung zwischen den Routern funktioniert.

### Test zu R-Sd1

```cmd
ping 192.168.2.1
```

### Test zu S-Sd1

```cmd
ping 192.168.2.2
```

### Test zu PC-B

```cmd
ping 192.168.2.111
```

Dieser Test ist am wichtigsten, weil dadurch überprüft wird, ob die komplette Verbindung von Braunau bis Schärding funktioniert.

Wenn die Antwort ungefähr so aussieht:

```text
Reply from 192.168.2.111
Reply from 192.168.2.111
Reply from 192.168.2.111
Reply from 192.168.2.111
```

ist die Verbindung erfolgreich.

---

## 13. Fehlerbehebung

Während der Konfiguration gab es teilweise das Problem, dass ein Ping nicht funktioniert hat.

Ein möglicher Grund dafür war, dass ein Router Interface noch nicht aktiviert war.

Mit

```cisco
show ip interface brief
```

kann man kontrollieren ob die Interfaces aktiv sind.

Wenn dort

```text
administratively down
```

steht, muss das Interface mit

```cisco
no shutdown
```

aktiviert werden.

Außerdem muss bei den PCs der richtige Default Gateway eingetragen sein.

Bei PC-A:

```text
192.168.1.1
```

Bei PC-B:

```text
192.168.2.1
```

Ein weiterer möglicher Fehler ist, dass die statische Route auf einem der Router fehlt.

---

## 14. Speichern der Konfiguration

Nachdem alles funktioniert hat, wurden die Router Konfigurationen gespeichert.

Dafür wurde folgender Befehl verwendet:

```cisco
copy running-config startup-config
```

Bei der Frage

```text
Destination filename [startup-config]?
```

wurde einfach Enter gedrückt.

Dadurch bleibt die Konfiguration auch nach einem Neustart erhalten.

---
