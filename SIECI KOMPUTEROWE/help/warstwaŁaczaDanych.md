# WARSTWA ŁĄCZA DANYCH (Data Link Layer)

Warstwa łącza danych to **druga warstwa modeli OSI i TCP/IP**.  
Jej zadaniem jest **dostarczenie danych w obrębie jednej sieci lokalnej (LAN)** oraz zapewnienie dostępu do medium transmisyjnego.

Warstwa ta stanowi **pomost między warstwą sieci (IP) a warstwą fizyczną (kable, fale radiowe)**.

---

## 1. Główne zadania warstwy łącza danych

Warstwa łącza danych:

- odbiera **pakiety IP** z warstwy sieciowej
- **enkapsuluje pakiety w ramki**
- dodaje **adresy MAC nadawcy i odbiorcy**
- kontroluje **dostęp do medium transmisyjnego**
- wykrywa błędy transmisji (CRC)
- zapewnia komunikację **tylko w obrębie jednej sieci lokalnej**

Najważniejsze:  
**warstwa łącza danych nie zna tras i nie routuje pakietów** — zajmuje się wyłącznie lokalnym dostarczeniem ramek.

---

## 2. Podwarstwy warstwy łącza danych

Warstwa łącza danych dzieli się na dwie podwarstwy:

### LLC (Logical Link Control)
- identyfikuje protokół warstwy sieci (IPv4, IPv6)
- umożliwia korzystanie z jednej karty sieciowej przez różne protokoły
- realizowana programowo (sterowniki)

### MAC (Media Access Control)
- odpowiada za **adresy MAC**
- określa **zasady dostępu do medium**
- realizowana sprzętowo (karta sieciowa)

---

## 3. Adres MAC — adres fizyczny

Adres MAC:
- ma **48 bitów**
- zapisywany jest w formacie szesnastkowym  
  np. `AA:BB:CC:DD:EE:FF`
- nadawany jest **fabrycznie** i zapisany w pamięci ROM karty
- identyfikuje **interfejs sieciowy**, nie urządzenie logiczne

Adres MAC:
- działa **tylko lokalnie**
- **nie przechodzi przez routery**
- zmienia się na każdym „skoku” pakietu przez router

---

## 4. Ramka warstwy łącza danych (Ethernet)

Dane z warstwy sieci (pakiet IP) są enkapsulowane w **ramkę Ethernet**.

### Struktura ramki Ethernet

- **Preambuła + SFD** – synchronizacja odbiornika
- **MAC docelowy**
- **MAC źródłowy**
- **Typ / Długość** – np. IPv4, IPv6
- **Dane** – pakiet IP (46–1500 bajtów)
- **FCS (CRC)** – suma kontrolna

### Rozmiary
- minimalny rozmiar ramki: **64 bajty**
- maksymalny rozmiar ramki: **1518 bajtów**
- ramki VLAN (802.1Q): do **1522 bajtów**

Jeśli dane są krótsze niż 46 bajtów → ramka jest **dopełniana (padding)**.

---

## 5. Enkapsulacja i dekapsulacja — krok po kroku

1. Aplikacja tworzy dane
2. Warstwa transportowa dodaje porty
3. Warstwa sieciowa dodaje adresy IP → **pakiet**
4. Warstwa łącza danych:
   - dodaje adresy MAC
   - tworzy **ramkę**
5. Ramka trafia do medium (kabel / Wi-Fi)

Po stronie odbiorcy proces odbywa się **odwrotnie**.

---

## 6. Komunikacja lokalna i zdalna (rola MAC)

### Jeśli host docelowy jest w tej samej sieci:
- ramka zawiera **MAC hosta docelowego**

### Jeśli host docelowy jest w innej sieci:
- ramka zawiera **MAC routera (bramy domyślnej)**

Adres IP **zawsze pozostaje adresem końcowym**,  
adres MAC **zmienia się na każdym segmencie sieci**.

---

## 7. Protokół ARP (Address Resolution Protocol)

ARP służy do:
**mapowania adresu IP → adres MAC**

### Dlaczego ARP jest potrzebny?
- aplikacje i IP znają **adresy IP**
- warstwa łącza danych potrzebuje **adresów MAC**

---

### Jak działa ARP?

1. Host chce wysłać dane do IP, ale nie zna MAC
2. Wysyła **ARP Request (broadcast)**:
   - MAC docelowy: `FF:FF:FF:FF:FF:FF`
   - pytanie: „Kto ma IP X.X.X.X?”
3. Wszystkie urządzenia odbierają ramkę
4. Tylko właściwe urządzenie odpowiada **ARP Reply**
5. Nadawca zapisuje wynik w **tablicy ARP**

### Tablica ARP
- przechowuje pary IP ↔ MAC
- wpisy są **tymczasowe** (kilka minut)
- w Windows:
**arp -a**


---

## 8. Ethernet — standard warstwy łącza danych

Ethernet to **najpopularniejszy standard LAN**.

- rozwijany przez IEEE
- standardy: **802.2 (LLC)** i **802.3 (MAC + fizyczna)**

### Dlaczego Ethernet odniósł sukces?
- prostota
- niezawodność
- skalowalność
- niski koszt
- możliwość rozwoju prędkości

---

## 9. Ewolucja Ethernetu

### Stare wersje
- kabel koncentryczny
- topologia magistrali
- metoda CSMA/CD
- kolizje
- niska wydajność

### Koncentratory (HUB)
- topologia gwiazdy fizycznie
- logicznie nadal magistrala
- jedna domena kolizyjna
- wysyłanie ramek do wszystkich

### Przełączniki (SWITCH)
- logiczna topologia punkt–punkt
- brak kolizji
- pełny dupleks
- dedykowana przepustowość
- podstawowe urządzenie LAN dziś

---

## 10. Przełączniki i tablica MAC

Switch:
- działa w **warstwie 2**
- używa **adresów MAC**
- posiada **tablicę MAC**

### Uczenie się MAC
- switch odczytuje MAC źródłowy
- zapamiętuje: MAC → port

### Flooding
- jeśli MAC docelowy nieznany
- ramka trafia na wszystkie porty (oprócz wejściowego)
- po odpowiedzi tablica się uzupełnia

---

## 11. Standardy Ethernet (przykłady)

- **Fast Ethernet** – 100 Mb/s (100BASE-TX)
- **Gigabit Ethernet** – 1 Gb/s (1000BASE-T)
- **10 Gigabit Ethernet** – 10 Gb/s
- Ethernet po światłowodzie: SX / LX
- VLAN: 802.1Q

---

## 12. Podsumowanie najważniejszych faktów

- warstwa łącza danych działa **lokalnie**
- używa **adresów MAC**
- tworzy **ramki**
- odpowiada za **dostęp do medium**
- ARP mapuje IP → MAC
- switch = kluczowe urządzenie warstwy 2
- router = granica domeny rozgłoszeniowej

---

## Jedno najważniejsze zdanie

> **Warstwa łącza danych dostarcza ramki w obrębie jednej sieci lokalnej, używając adresów MAC i zapewniając fizyczny dostęp do medium transmisyjnego.**

