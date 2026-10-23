# 📑 Arbeitsbericht (SYTI)

![Student](https://img.shields.io/badge/Student-Denis%20VELIC-lightblue?style=for-the-badge&logo=github)
![Klasse](https://img.shields.io/badge/Klasse-4AHITS-blue?style=for-the-badge&logo=googleclassroom&logoColor=white)
![Datum](https://img.shields.io/badge/Datum-23.10.2026-darkblue?style=for-the-badge&logo=googlecalendar&logoColor=white)

## 👤 Basisinformationen
| **Thema** | [LEDs und Taster](https://github.com/user-attachments/files/33861108/Ubungsaufgaben.LEDs.und.Taster.pdf)
) |

| **Name** | Denis Velic |

| **Fach** | SYTI |

---

## Aufgabe 1:

### Aufgabenstellung:
Der Zustand der Tasten soll durch die LED-Reihe angezeigt werden: Solange Taste 0 gedrückt ist,
leuchtet LED 0 usw.

### Lösung:

```sh

#include <Arduino.h>



void setup() {
DDRF=0xFF;

DDRK=0x00;

PORTK=0xFF;

}

void loop() {

PORTF = ~PINK;

}

```
