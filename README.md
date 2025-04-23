# InConsoleLib

Biblioteka do obsługi zestawu InConsole na ESP32.
Zawiera moduły: ekran, joystick, buzzer, przyciski, Bluetooth, WiFi, debug i monitorowanie baterii.

---

## 📦 Moduły biblioteki

- **InConsole** – podstawowa inicjalizacja zestawu
- **InConsoleDebug** – łatwa obsługa komunikacji debug przez Serial
- **InConsoleBT** – komunikacja Bluetooth z funkcją parowania
- **InConsoleWifi** – tryb klienta WiFi oraz Access Point + prosty serwer
- **InConsoleBAT** – monitorowanie napięcia i poziomu naładowania baterii

---

## 🪛 InConsoleDebug

```cpp
serial.begin(9600);         // Inicjalizacja portu szeregowego
serial.send("Witaj");       // Wysyła tekst bez nowej linii
serial.sendln("Debug OK");  // Wysyła tekst z nową linią
serial.sendln(1234);        // Wysyła liczbę z nową linią
serial.send("1234");       // Wysyła liczbę bez nowej linii
String input = serial.readInput(); // Wczytuje całą linię z Serial (blokujące)
```

---

## 🔋 InConsoleBAT

Monitorowanie napięcia i poziomu naładowania akumulatora.

```cpp
battery.begin();                         // Inicjalizacja pomiaru baterii na pinie ADC
float voltage = battery.getBatteryVoltage();   // Zwraca napięcie baterii w V (np. 3.85V)
float percent = battery.getBatteryPercent();       // Zwraca poziom naładowania (0-100%)
float voltage = readVoltage();      // Zwraca napięcie na pinnie 36
int adc = readADC();        // Zwraca wartość ADC(tylko do debugowania, raczej nie używane)
```

**Domyślne stałe napięcia (zdefiniowane w bibliotece):**
- Napięcie minimalne: 3.0V
- Napięcie maksymalne: 4.2V
- Wejście z optoizolatora: 1.5V – 2.1V skalowane na 3.0V – 4.2V

---

## 📡 InConsoleBT

Bluetooth z funkcją parowania przez wspólny kod:

```cpp
BT.begin();                      // Inicjalizacja Bluetooth
BT.pairDevices("2137");         // Ustaw kod parowania
BT.waitForPairing();            // Oczekiwanie na drugie urządzenie
bool paired = BT.isPaired();    // Czy urządzenia są połączone?
BT.sendMessage("test");        // Wyślij wiadomość do drugiego ESP32
String msg = BT.receiveMessage();  // Odczytaj otrzymaną wiadomość (jeśli przyszła)
```

---

## 🌐 InConsoleWifi

### Tryb klienta (ESP32 łączy się z siecią)

```cpp
wifi.connectToWiFi("SSID", "PASS");      // Połącz z siecią WiFi
bool ok = wifi.isConnected();            // Sprawdź połączenie
String ip = wifi.getLocalIP().toString();           // Pobierz IP
String json = wifi.get("http://example.com");         // Pobierz dane z internetu
```

### Tryb Access Point + Serwer

```cpp
wifi.startAP("Nazwa", "12345678");       // Utwórz własny hotspot
wifi.setHTML("<h1>Hello</h1>");          // Ustaw zawartość HTML
// W loopie:
wifi.update();                           // Obsługa żądań HTTP
```

---

## 💡 Przykłady

- `examples/Serial/Serial.ino` – testowanie i debugowanie esp32
- `examples/WIFI/wifi_client/wifi_client.ino` – pobieranie JSON-a z internetu
- `examples/WIFI/wifi_ap/wifi_ap.ino` – hotspot z custom HTML
- `examples/BT/Chat/Chat.ino` – czat przez Bluetooth z kodem parowania
- `examples/BAT/Level/Level.ino` – poziom naładowania oraz informacje o stanie baterii

---

## 📁 Instalacja

1. Skopiuj folder `InConsoleLib` do katalogu `Arduino/libraries`
2. Upewnij się, że używasz ESP32 i masz wgrany sterownik

---

## 🧠 Autor

Mateusz Lademann (Mati) – InGraw Co.

## POWODZENIA
