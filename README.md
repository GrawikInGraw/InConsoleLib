
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

- **TFT_eSPI**  
  https://github.com/Bodmer/TFT_eSPI  
  Wydajna biblioteka graficzna dla wyświetlaczy TFT, używana zamiast Adafruit ST7735 i GFX.
  
  **Biblioteka TFT_eSPI jest używana w tym projekcie zgodnie z licencją MIT.
  Pełna treść licencji dostępna jest w repozytorium autora!**

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

## 🎨 Korzystanie z TFT_eSPI w InConsoleLib

Twoja biblioteka już inicjalizuje wyświetlacz TFT, więc skupimy się na tym, jak efektywnie korzystać z podstawowych funkcji rysowania i wyświetlania tekstu, które da Ci TFT_eSPI.

### Kolory

- Używaj 16-bitowych kolorów w formacie RGB565, np.:
  - `ST77XX_BLACK` — czarny (tło domyślne)
  - `ST77XX_WHITE` — biały (tekst, elementy)
  - `ST77XX_RED`, `ST77XX_GREEN`, `ST77XX_BLUE` — podstawowe kolory
  - Możesz tworzyć własne kolory przez kombinację RGB, np. `ic.tft.color565(255, 100, 0)`

### Tekst

- Funkcje do wyświetlania tekstu:
  ```cpp
  ic.tft.setCursor(x, y);         // ustawia pozycję tekstu (piksele)
  ic.tft.setTextColor(color);     // ustawia kolor tekstu (opcjonalnie z tłem)
  ic.tft.setTextSize(size);       // skaluje rozmiar czcionki (1..n)
  ic.tft.print("Twój tekst");     // rysuje tekst na ekranie
  ```
- Pamiętaj, że tekst jest rysowany od aktualnej pozycji kursora.

### Rysowanie kształtów

- Linie i kształty do szybkiego GUI:
  ```cpp
  ic.tft.drawLine(x0, y0, x1, y1, color);          // linia
  ic.tft.drawRect(x, y, width, height, color);     // prostokąt
  ic.tft.fillRect(x, y, width, height, color);     // wypełniony prostokąt
  ic.tft.drawCircle(x, y, r, color);                // okrąg
  ic.tft.fillCircle(x, y, r, color);                // wypełniony okrąg
  ```
  
### Czyszczenie ekranu

- Szybkie czyszczenie:
  ```cpp
  ic.tft.fillScreen(ST77XX_BLACK);
  ```

---

### Dodatkowe tipy

- Pamiętaj aby przed `tft` dodać `ic`.
- Kolory i pozycje miej na uwadze względem rozdzielczości 128x160 px.
- Optymalizuj rysowanie, minimalizując niepotrzebne czyszczenia i rysowanie dużych elementów na nowo.

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
2. Zainstaluj wymagane bibliotekę **TFT_eSPI** przez Menadżera bibliotek Arduino.
3. 
4. Upewnij się, że masz zainstalowaną platformę ESP32 (esp32/arduino).
5. Uruchom ponownie Arduino IDE.
6. Podłącz ESP32 i skompiluj swoje projekty z `#include "ic.h"`.

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

Mateusz Lademann (InGraw Co.) 

---

# Licencja

Ten projekt jest udostępniany na licencji **Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)**.

Oznacza to, że:

- **możesz** kopiować, modyfikować, rozpowszechniać i tworzyć na bazie tego projektu,
- **musisz** podać autora: *Mateusz Lademann (InGraw Co.)*,
- **nie możesz** używać tego projektu do celów komercyjnych ani sprzedawać go.

🔗 Pełny tekst licencji dostępny jest pod adresem:  
[https://creativecommons.org/licenses/by-nc/4.0/deed.pl](https://creativecommons.org/licenses/by-nc/4.0/deed.pl)

---

## Przykład prawidłowego oznaczenia autora

> Na podstawie projektu autorstwa Mateusza Lademanna (InGraw Co.)  
> Źródło: https://github.com/InGraw-Co/InConsoleLib  
> Licencja: CC BY-NC 4.0

---

[![License: CC BY-NC 4.0](https://licensebuttons.net/l/by-nc/4.0/88x31.png)](https://creativecommons.org/licenses/by-nc/4.0/)


📈 *Z InConsole nauczysz się lutowania, programowania i elektroniki krok po kroku – pełna kontrola sprzętu, zero magii, tylko jasny kod i działanie.*
