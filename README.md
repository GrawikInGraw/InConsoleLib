
# InConsoleLib

📦 **Biblioteka do obsługi zestawu InConsole na ESP32**

Dedykowana dla młodych konstruktorów, graczy i pasjonatów elektroniki, którzy chcą mieć kompletną i czytelną bibliotekę do swojego zestawu edukacyjnego z ESP32 — prosto, bez kombinacji, a jednocześnie elastycznie i solidnie.

---

## 🚀 Co masz w InConsoleLib?

| Moduł / Klasa     | Opis                                                           |
|-------------------|----------------------------------------------------------------|
| 🧠 `InConsole`    | Podstawowa klasa inicjalizująca zestaw, obsługa przycisków, buzzer, TFT i SD |

> To jest Twój główny sterownik — pełna obsługa sprzętu w jednym miejscu.

---

## 📋 Wymagania bibliotek zewnętrznych

Przed użyciem `InConsoleLib` musisz mieć zainstalowane:

- **Adafruit GFX**  
  https://github.com/adafruit/Adafruit-GFX-Library  
  Podstawa do wyświetlania grafiki na TFT.
  
- **Adafruit ST7735**  
  https://github.com/adafruit/Adafruit-ST7735-Library  
  Sterownik do Twojego wyświetlacza TFT 1.8" (ST7735).
  
- Upewnij się również, że masz zainstalowaną platformę **esp32** w preferencjach Arduino IDE:
  https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
  
  tak powinno wyglądać twoje okienko w ustawieniach:

  ![image](https://github.com/user-attachments/assets/3124703d-91ae-4e58-8b47-1f59a32d031e)


---

## 🛠️ Funkcje i API klasy `InConsole`

### 1. `void begin();`  
Inicjalizuje cały zestaw:  
- UART (Serial 115200)  
- Konfiguracja pinów przycisków i buzzera  
- Uruchomienie SPI dla TFT i SD (dwa różne SPI!)  
- Inicjalizacja wyświetlacza TFT (Adafruit_ST7735)  
- Inicjalizacja karty SD, sprawdzenie dostępności i komunikat na TFT oraz Serialu

---

### 2. Obsługa przycisków

Wszystkie przyciski działają na zasadzie wykrywania **pojedynczego naciśnięcia** (krawędź opadająca):

```cpp
bool readButtonA();     // przycisk A
bool readButtonB();     // przycisk B
bool readStart();       // przycisk Start
bool readSelect();      // przycisk Select
```

**Uwaga:** Przycisk zwraca `true` tylko w momencie wykrycia naciśnięcia, nie trzyma stanu.

---

### 3. Buzzer

Prosta funkcja beep:

```cpp
void beep(int duration_ms);  // Dźwięk buzzera na określony czas (ms)
```

---

### 4. Status karty SD

```cpp
bool SD_ok();  // true jeśli karta SD jest gotowa i dostępna
```

---

### 5. Wyświetlacz TFT

Wykorzystywany jest sterownik `Adafruit_ST7735` podłączony do SPI HSPI na pinach:

| Funkcja | Pin ESP32 |
|---------|-----------|
| TFT_CS  | 16        |
| TFT_RST | 4         |
| TFT_DC  | 2         |
| TFT_SCLK| 21        |
| TFT_MOSI| 22        |

> Wyświetlacz inicjalizowany jest z rotacją 3 i czarnym tłem.

---

### 6. Karta SD

SD działa na SPI VSPI z pinami:

| Funkcja | Pin ESP32 |
|---------|-----------|
| SD_CS   | 5         |
| SD_MOSI | 23        |
| SD_MISO | 19        |
| SD_SCLK | 18        |

---

## 💡 Przykładowe użycie

```cpp
#include "ic.h"

void setup() {
  ic.begin();

  if(ic.SD_ok()) {
    Serial.println("Karta SD gotowa!");
  } else {
    Serial.println("Brak karty SD!");
  }
}

void loop() {
  if(ic.readButtonA()) {
    ic.beep(100);  // sygnał buzzera po naciśnięciu A
  }
}
```

---

## 📚 Instalacja

1. Pobierz bibliotekę `InConsoleLib` i umieść folder w `Arduino/libraries`.
2. Zainstaluj wymagane biblioteki **Adafruit GFX** i **Adafruit ST7735** przez Menadżera bibliotek Arduino.
3. Upewnij się, że masz zainstalowaną platformę ESP32 (esp32/arduino).
4. Uruchom ponownie Arduino IDE.
5. Podłącz ESP32 i skompiluj swoje projekty z `#include "ic.h"`.

---

## 🔧 Szczegóły implementacji i tipy

- **Dwa SPI:**  
  - TFT używa HSPI (piny 21/22/16)  
  - SD używa VSPI (piny 23/19/18/5)  
  To jest kluczowe, bo mieszanie może powodować błędy komunikacji.

- **Przyciski:** Są podciągnięte wewnętrznie (INPUT_PULLUP) – stan niskiego logicznego oznacza naciśnięcie.

- **Buzzer:** prosty on/off na pin 33. Możesz rozbudować o PWM jeśli chcesz.

- **Wyświetlacz:** Ustawiony na rotację 3, by obraz był dobrze orientowany względem fizycznego montażu.

- **SD:** Sprawdzaj stan przed odczytem/zapisem. Brak karty wyświetlany jest na TFT czerwonym kolorem.

---

## 👨‍💻 Autor i rozwój

Mateusz Lademann (Mati) – twórca InGraw Co. i InConsole.  
Projektuj z głową, ucz się na błędach i buduj przyszłość z InConsole!  

---

📈 *Z InConsole nauczysz się lutowania, programowania i elektroniki krok po kroku – pełna kontrola sprzętu, zero magii, tylko jasny kod i działanie.*
