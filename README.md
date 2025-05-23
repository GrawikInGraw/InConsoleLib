
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
  W celu poprawnego działania należy samodzielnie pobrać i zainstalować TFT_eSPI, a następnie zastąpić plik `User_setup.h` dostarczony w tym projekcie.
  
  **Uwaga:** Biblioteka TFT_eSPI pozostaje własnością oryginalnego autora! Proszę zapoznać się z warunkami licencji tej biblioteki w jej repozytorium.

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

Twoja biblioteka już inicjalizuje wyświetlacz TFT, więc poniżej znajdziesz skróconą dokumentację najczęściej używanych funkcji z biblioteki TFT_eSPI, które pozwolą Ci tworzyć GUI, teksty i kształty graficzne na ekranie.

### Kolory

- Gotowe kolory znajdziesz np. w `TFT_BLACK`, `TFT_WHITE`, `TFT_RED`, `TFT_GREEN`, `TFT_BLUE`, `TFT_YELLOW`, itd.
- Własne kolory:
  ```cpp
  uint16_t kolor = ic.tft.color565(255, 128, 0); // RGB → pomarańczowy

### Tekst

- Funkcje do wyświetlania tekstu:
  ```cpp
  ic.tft.setTextColor(TFT_WHITE, TFT_BLACK); // kolor tekstu i tła (tło opcjonalne)
  ic.tft.setTextSize(2);                     // skalowanie czcionki (1 = normalny rozmiar)
  ic.tft.setCursor(10, 20);                  // ustaw pozycję x, y (piksele)
  ic.tft.print("Witaj!");                    // wypisz tekst

  ```
- Pamiętaj, że tekst jest rysowany od aktualnej pozycji kursora.
- Domyślna czcionka to GLCD (5x7). Możesz ładować większe fonty, np. `ic.tft.setFreeFont(FSS12);`.

### Rysowanie kształtów

- Linie i kształty do szybkiego GUI:
  ```cpp
  ic.tft.drawPixel(x, y, TFT_WHITE); // Jeden pixel
  ic.tft.drawLine(x0, y0, x1, y1, TFT_BLUE); // Linia
  ic.tft.drawRect(x, y, w, h, TFT_GREEN); // Prostokąt
  ic.tft.fillRect(x, y, w, h, TFT_RED); // Wypełniony Prostokąt
  ic.tft.drawCircle(x, y, r, TFT_YELLOW); // Okrąg
  ic.tft.fillCircle(x, y, r, TFT_CYAN); // Wypełniony Okrąg
  ic.tft.drawRoundRect(x, y, w, h, radius, TFT_ORANGE); // Zaokrąglony Prostokąc
  ic.tft.fillRoundRect(x, y, w, h, radius, TFT_MAGENTA); // Wypełniony Zaokrąglony Prostokąt

  ```
  
### Czyszczenie ekranu

- Szybkie czyszczenie:
  ```cpp
  ic.tft.fillScreen(TFT_BLACK); // czyszczenie
  ```

### Włączenie/Wyłącznie

- Wyłączenie lub włączenie wyświetlania (np. do oszczędzania energii):
  ```cpp
  ic.tft.writecommand(TFT_DISPOFF);  // ekran wyłączony
  ic.tft.writecommand(TFT_DISPON);   // ekran włączony

  ```

---

### Dodatkowe tipy

- Pamiętaj aby przed `tft` dodać `ic`.
- Kolory i pozycje miej na uwadze względem rozdzielczości 128x160 px.
- Optymalizuj rysowanie, minimalizując niepotrzebne czyszczenia i rysowanie dużych elementów na nowo.

---

## 💾 Korzystanie z karty SD (SPI)

InConsoleLib obsługuje kartę SD przez interfejs SPI, umożliwiając zapisywanie i odczytywanie danych, np. wyników, zapisów gier czy ustawień.

### 📁 Tworzenie, odczyt, zapis i usuwanie plików

| Funkcja                                | Opis                                                  |
|----------------------------------------|-------------------------------------------------------|
| `File file = SD.open("/plik.txt")`     | Otwiera plik do odczytu                               |
| `File file = SD.open("/plik.txt", FILE_WRITE)` | Otwiera plik do zapisu (tworzy, jeśli nie istnieje) |
| `file.println("tekst")`                | Zapisuje linię tekstu do otwartego pliku             |
| `file.read()`, `file.parseInt()`       | Czyta dane z otwartego pliku                         |
| `file.close()`                         | Zamyka otwarty plik (zawsze wymagane!)              |
| `SD.exists("/plik.txt")`               | Sprawdza, czy plik istnieje                         |
| `SD.remove("/plik.txt")`               | Usuwa wskazany plik                                 |

---

### 📜 Odczyt danych z pliku

```cpp
File file = SD.open("/save.txt");
if (file) {
  int value = file.parseInt(); // Odczytuje liczbę z pliku
  file.close();
}
```

---

### 💾 Zapis danych do pliku

```cpp
File file = SD.open("/save.txt", FILE_WRITE);
if (file) {
  file.println(123); // Zapisuje liczbę do pliku
  file.close();
}
```

---

### 🧹 Usuwanie pliku

```cpp
if (SD.exists("/save.txt")) {
  SD.remove("/save.txt"); // Usuwa plik
}
```

---

### 📌 Zasady i dobre praktyki

- **Ścieżki** zawsze zaczynają się od `/`, np. `"/highscore.txt"`
- System plików: FAT16 / FAT32
- Zawsze **zamykaj plik** po odczycie/zapisie (`file.close()`)
- Nie otwieraj/zamykaj plików zbyt często w `loop()` — oszczędzaj RAM i cykle
- Pliki są przechowywane na karcie SD w głównym katalogu

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

1. Zainstaluj środowisko `Arduino IDE`.
2. Pobierz bibliotekę `InConsoleLib` i umieść folder w `Arduino/libraries`.
3. Zainstaluj wymagane bibliotekę **TFT_eSPI** przez Menadżera bibliotek Arduino.
4. Zastąpi plik `User_setup.h` znajdujący się w `Adruino/libraries/TFT_eSPI` na ten dostarczony w tym projekcie.
5. Upewnij się, że masz zainstalowaną platformę ESP32 (esp32/arduino).
6. Uruchom ponownie Arduino IDE.
7. Podłącz ESP32 i skompiluj swoje projekty.

---

## 🔧 Szczegóły implementacji i tipy

- **Dwa SPI:**  
  - TFT używa HSPI (piny 21/22/16)  
  - SD używa VSPI (piny 23/19/18/5)  
  To jest kluczowe, bo mieszanie może powodować błędy komunikacji.

- **Przyciski:** Są podciągnięte wewnętrznie (INPUT_PULLUP) – stan niski oznacza naciśnięcie.

- **Buzzer:** prosty on/off na pin 33 sterowany tranzystorem.

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
> Źródło: [https://github.com/InGraw-Co/InConsoleLib  ](https://github.com/InGraw-Co/InConsoleLib)
> 
> Licencja: CC BY-NC 4.0

---

[![License: CC BY-NC 4.0](https://licensebuttons.net/l/by-nc/4.0/88x31.png)](https://creativecommons.org/licenses/by-nc/4.0/)


📈 *Z InConsole nauczysz się lutowania, programowania i elektroniki krok po kroku – pełna kontrola sprzętu, zero magii, tylko jasny kod i działanie.*
