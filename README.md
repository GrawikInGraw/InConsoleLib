# InConsoleLib

📦 **Biblioteka do obsługi zestawu InConsole na ESP32**

Stworzona z myślą o młodych konstruktorach, graczach i hobbystach elektroniki. Łatwa, przejrzysta i kompletna biblioteka do Twojego zestawu edukacyjnego z ESP32.

---

## 🚀 Moduły w bibliotece

- 🧠 `InConsole` – inicjalizacja zestawu
- 🧰 `InConsoleDebug` – szybkie debugowanie przez Serial
- 📡 `InConsoleBT` – komunikacja Bluetooth z funkcją parowania
- 🌐 `InConsoleWifi` – tryb klienta WiFi i Access Point
- 🔋 `InConsoleBAT` – monitorowanie napięcia baterii
- 💾 `InConsoleSD` – odczyt/zapis plików i wczytywanie BMP z SD

---

## 🧰 InConsoleDebug – debug dla każdego

```cpp
serial.begin(9600);                // Start Serial
serial.send("Witaj");              // Wyslij tekst bez nowej linii
serial.sendln("Debug OK");         // Tekst z nową linią
serial.sendln(1234);                // Liczba z nową linią
String input = serial.readInput();  // Wczytaj linię z Serial (blokujące)
```

---

## 🔋 InConsoleBAT – bateria pod kontrolą

```cpp
battery.begin();                      // Start ADC
float voltage = battery.getBatteryVoltage();   // Napięcie (V)
float percent = battery.getBatteryPercent();   // Poziom naładowania (%)
float voltage = readVoltage();                 // Surowe napięcie (z ADC)
int adc = readADC();                           // Surowa wartość ADC
```

**Stałe wbudowane:**
- Pin ADC: 36
- Zakres: 0–4095 (12-bit)
- Napięcie skalowane z PC817: 1.5–2.1V → 3.0–4.2V

---

## 📡 InConsoleBT – Bluetooth parowanie i czat

```cpp
BT.begin();                        // Start BT
BT.update();                       // Pętla BT
BT.pairDevices("1234");           // Kod parowania
BT.waitForPairing();              // Czekanie na drugie ESP
bool ok = BT.isPaired();          // Czy połączono?
BT.sendMessage("Hej!");          // Wyślij wiadomość
String msg = BT.receiveMessage(); // Odbierz wiadomość
```

---

## 🌐 InConsoleWifi – połącz się z siecią lub zostań routerem

### Tryb klienta WiFi

```cpp
wifi.connectToWiFi("SSID", "PASS");     // Połącz z siecią
bool ok = wifi.isConnected();            // Czy połączenie działa?
String ip = wifi.getLocalIP().toString(); // IP urządzenia
String json = wifi.get("http://example.com"); // Pobierz JSON
```

### Tryb Access Point (hotspot z serwerem)

```cpp
wifi.startAP("InConsole", "12345678"); // Stwórz sieć
wifi.setHTML("<h1>Witaj!</h1>");       // HTML strony
wifi.update();                          // Obsługa zapytań HTTP
```

---

## 💾 InConsoleSD – obsługa karty SD

```cpp
SD.begin();                                     // Start SD
SD.writeFile("/test.txt", "Dane zapisu");       // Zapisz plik
String content = SD.readFile("/test.txt");     // Odczytaj plik
SD.appendFile("/test.txt", "\nDopisane!");      // Dopisz do pliku
SD.deleteFile("/usun.txt");                    // Usuń plik
bool isThere = SD.exists("/test.txt");         // Czy plik istnieje?
```

📷 Wczytaj BMP do tablicy:
```cpp
uint8_t* bmpData;
size_t bmpSize;
if (SD.loadBMP("/image.bmp", &bmpData, &bmpSize)) {
  // dane są dostępne w bmpData, bmpSize
  free(bmpData); // nie zapomnij zwolnić pamięci!
}
```

---

## 💡 Przykłady

- `Serial.ino` – test komunikacji debug
- `wifi_client.ino` – pobieranie JSON z internetu
- `wifi_ap.ino` – lokalny serwer z HTML
- `Chat.ino` – czat BT z parowaniem
- `Level.ino` – poziom baterii + napięcie

---

## 🧰 Instalacja

1. Pobierz bibliotekę i skopiuj folder `InConsoleLib` do `Arduino/libraries`
2. Upewnij się, że masz zainstalowaną platformę ESP32
3. Restart Arduino IDE

---

## 👨‍💻 Autor

Mateusz Lademann (Mati) – twórca InGraw Co.

---

📈 *Zaprojektowane z myślą o rozwoju – ucz się, koduj, baw się i twórz przyszłość z InConsole!*

