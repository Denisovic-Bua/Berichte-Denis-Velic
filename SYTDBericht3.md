# 📑 Arbeitsbericht (SYTD)

![Student](https://img.shields.io/badge/Student-Denis%20VELIC-lightblue?style=for-the-badge&logo=github)
![Klasse](https://img.shields.io/badge/Klasse-4AHITS-blue?style=for-the-badge&logo=googleclassroom&logoColor=white)
![Datum](https://img.shields.io/badge/Datum-05.10.2026-darkblue?style=for-the-badge&logo=googlecalendar&logoColor=white)

## 👤 Basisinformationen
| **Thema** | [Standortvernetung IPv6] |
| :--- | :--- |
| **Fach** | Systemtechnik (SYTD) |

---

## Aufgabenstellung

[Übung_03-Standortvernetzung_IPv6.pdf](https://github.com/user-attachments/files/33099486/Ubung_03-Standortvernetzung_IPv6.pdf)

Als Basis wird das fertige Paket Tracer file von der letzen Übung mit IPv4 benutzt.

In dieser Übung war die Aufgabe, zwei verschiedene Standorte über zwei Router miteinander zu verbinden jedoch jetzt mit IPv6.

Die beiden Standorte sind:

- Braunau
- Schärding

An jedem Standort gibt es jeweils einen PC, einen Switch und einen Router.  
Die beiden Router werden direkt miteinander verbunden.

Das Ziel war, dass sich am Ende alle Geräte untereinander erreichen können und besonders PC-A zu PC-B pingen kann.

Die verwendete Topologie sieht ungefähr so aus:

### Topologie:

<img width="1406" height="384" alt="image" src="https://github.com/user-attachments/assets/9fdec988-7123-499c-8db2-260514aa94fe" />

## R-Br1 Konfigurieren
<img width="1736" height="1954" alt="image" src="https://github.com/user-attachments/assets/45fe0cfd-8a90-4765-b354-50dbe962b103" />

## R-Sd1 Konfigurieren
<img width="1736" height="1954" alt="image" src="https://github.com/user-attachments/assets/6cc40e4c-c1bd-4a18-853d-5b2752206285" />

## S-Br1 Konfigurieren
<img width="1736" height="1954" alt="image" src="https://github.com/user-attachments/assets/63179e1b-cde2-4923-8988-1140f1a04ee0" />

## S-Sd1 Konfigurieren
<img width="1736" height="1954" alt="image" src="https://github.com/user-attachments/assets/17c92609-7354-4cc8-b855-fc05bfe199ec" />
Die IPv6 Commands werden bei den Switches nicht direkt funktionieren, dann muss man das eingeben in die shell:

```sh
enable
configure terminal
sdm prefer dual-ipv4-and-ipv6 default
end
copy running-config startup-config
reload
```

## R-Br1 Check
<img width="1736" height="1954" alt="image" src="https://github.com/user-attachments/assets/933b7f62-292e-4d06-950a-07a75881efde" />
















