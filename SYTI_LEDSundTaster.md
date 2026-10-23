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

void setup() {
DDRF=0xFF;

DDRK=0x00;

PORTK=0xFF;

}

void loop() {

PORTF = ~PINK;

}

```

## Aufgabe 2:

### Aufgabenstellung:
Modifizierung der vorherigen Aufgabe: Der Zustand der Taste 0 wird durch LED 7 angezeigt, der
Zustand von Taste 1 von LED 6 usw. => soll auch bei mehreren Tastendrücken gleichzeitig
funktionieren!

### Lösung:

```sh

  byte tasterZustand;
  byte ledZustand;

void setup() {
  DDRF = 0xFF;  
  DDRK = 0x00;  
  PORTK = 0xFF; 
    
}

void loop() {

tasterZustand =  ~PINK;
ledZustand = 0;

 
  for (int i = 0; i < 8; i++) {
    if (tasterZustand & (1 << i)) {
      ledZustand |= (1 << (7 - i));
    }
  }

  PORTF = ledZustand; 
}

```

### Erklärung:

das Programm schaut beim if ob eine taste gedrückt wird das wird mit `&` verknüft und prüft es dann. in der if wird einfach mit `|` verglichen damit es gesetzt wird und rechnet minus dem index. somit wenn ich taste `0` drücke rechnet er `7-0`und dann wird das 7. Led eingeschaltet

## Aufgabe 3:
Ausgabe von LED-Mustern: Solange Taste 0 gedrückt wird, wird das LED-Muster Nr. 0 ausgegeben,
solange Taste 1 gedrückt wird, LED-Muster 1, usw. (z. B. Muster 0 = 00000000, Muster 1 = 10000000,
… Muster 7 = 11111111)

## Lösung:

```sh

```


