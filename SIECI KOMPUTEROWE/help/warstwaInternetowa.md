# WARSTWA INTERNETOWA (SIECIOWA)

Warstwa Internetowa (IP layer, Network Layer) jest **trzecią warstwą** modelu TCP/IP.
Odpowiada za **adresację logiczną**, **trasowanie (routing)** i **przekazywanie pakietów** między **różnymi sieciami**.

To tutaj działają kluczowe protokoły:
* **IP** (IPv4, IPv6)
* **ARP** (Address Resolution Protocol)
* **ICMP** (ping, błędy)
* **NAT** (Network Address Translation)
* **Protokoły routingu** (OSPF, BGP, RIP)

> **Kluczowa Rola:** Warstwa sieciowa działa na zasadzie **best-effort** (najlepiej jak się da) — nie dba o niezawodność ani kolejność pakietów (to zadanie TCP).

---

## 1. Zadania Warstwy Sieciowej

1.  **Adresowanie logiczne (IP):** Każde urządzenie w sieci globalnej otrzymuje unikalny adres IP.
2.  **Routing:** Routery wybierają optymalną drogę dla pakietu od źródła do celu.
3.  **Przekazywanie pakietów (Forwarding):** Router decyduje, do którego interfejsu wysłać pakiet, bazując na tablicy routingu.
4.  **Fragmentacja i składanie pakietów:** Dzielenie pakietu, jeśli jest większy niż MTU (Maximum Transmission Unit).
5.  **Mapowanie IP -> MAC (ARP):** Tłumaczenie adresów logicznych na fizyczne (w LAN).
6.  **Diagnostyka (ICMP):** Używany do testowania (ping, traceroute) i obsługi błędów sieci.
7.  **Translacja Adresów (NAT):** Umożliwia współdzielenie publicznych adresów IP.

---

## 2. Najważniejsze Protokoły Warstwy Sieciowej

* **IP (Internet Protocol):** Główny protokół, tworzy pakiety i adresuje urządzenia.
* **ARP (Address Resolution Protocol):** Tłumaczy **IP na MAC** w sieci lokalnej (LAN).
* **ICMP (Internet Control Message Protocol):** Wysyła komunikaty kontrolne (np. **Time Exceeded, Destination Unreachable**).
* **NAT (Network Address Translation):** Zmienia adresy prywatne na publiczne (SNAT, DNAT, PAT).

**Protokoły routingu:**
* **OSPF** (Open Shortest Path First): Zaawansowany routing wewnętrzny (IGP) dla dużych sieci firmowych.
* **BGP** (Border Gateway Protocol): Routing globalny (EGP), **kręgosłup Internetu**, używany między operatorami.

---

## 3. Protokół IPv4 – Szczegółowy Opis

IPv4 to **32-bitowy** protokół adresowania (4,3 miliarda adresów).

* **Format:** AAAA.BBBB.CCCC.DDDD (np. 192.168.1.10).
* **Cechy:** Brak wbudowanego szyfrowania, wymaga NAT z powodu wyczerpania adresów.
* **Fragmentacja:** Może być wykonywana przez routery po drodze (choć obecnie jest niezalecana).

## 4. Budowa Pakietu IPv4 


| Pole | Rozmiar | Opis |
| :--- | :--- | :--- |
| **Wersja** | 4b | Zawsze `4` dla IPv4. |
| **TTL (Time To Live)** | 8b | Zmniejsza się o 1 na każdym routerze. Po osiągnięciu 0 pakiet jest wyrzucany (zapobieganie **pętlom routingu**). |
| **Protokół** | 8b | Wskazuje protokół wyższej warstwy, który znajduje się w polu Danych (np. **6 = TCP, 17 = UDP, 1 = ICMP**). |
| **Adresy** | 32b/32b | Adres źródłowy i docelowy. **Nie zmieniają się** w trakcie podróży (oprócz NAT). |
| Identyfikacja, Flagi, Offset | | Służą do kontroli fragmentacji. |
| Suma kontrolna | 16b | Tylko dla nagłówka, nie dla danych. |
| Dane | Zmienna | Segment TCP, UDP lub komunikat ICMP. |

---

## 5. Adresowanie IPv4 — Omówienie

Adres IP jest logicznym adresem i składa się z: **adresu sieci** oraz **adresu hosta**. Granicę określa **maska podsieci**.

### Najpopularniejsze maski:
* `/24` $\rightarrow$ 255.255.255.0 (254 hosty)
* `/16` $\rightarrow$ 255.255.0.0 (65534 hosty)
* `/8` $\rightarrow$ 255.0.0.0

### Adresy Prywatne (RFC 1918)
Te zakresy **nie są routowane** w publicznym Internecie:
| Zakres | Maska |
| :--- | :--- |
| **10.0.0.0** | /8 |
| **172.16.0.0** | /12 (172.16.0.0 do 172.31.255.255) |
| **192.168.0.0** | /16 |

### Adresy Specjalne
* **127.0.0.1** – Loopback (adres testowy).
* **0.0.0.0** – Nieokreślony (domyślny w tablicach routingu).
* **Adres sieci:** Pierwszy adres w podsieci.
* **Broadcast:** Ostatni adres w podsieci (wysyłanie do wszystkich hostów w danej sieci).

---

## 6. ARP – Mapowanie IP -> MAC

ARP (Address Resolution Protocol) działa **tylko w sieciach lokalnych** (LAN), jest niezbędny do wysłania pakietu IP przez fizyczny interfejs (Ethernet, Wi-Fi).

1.  Gdy host A chce wysłać pakiet do hosta B w tej samej sieci, ale nie zna jego MAC.
2.  Host A wysyła **broadcast ARP** do wszystkich hostów w LAN: „Kto ma IP `192.168.1.20`?”
3.  Host B odpowiada (Unicast): „Ja mam, mój MAC to `AA-BB-CC-...`”.
4.  Host A zapisuje tę parę w **ARP cache**.

---

## 7. ICMP — Diagnostyka Sieci

ICMP jest protokołem pomocniczym (nie transportowym) i jest kapsułkowany w pakietach IP.

**Najważniejsze Komunikaty:**
* **Echo Request / Echo Reply:** Podstawa działania polecenia **ping**.
* **Time Exceeded:** Wyrzucany przez router, gdy **TTL osiągnie 0** (kluczowy dla **traceroute**).
* **Destination Unreachable:** Informuje o problemie z dotarciem do celu lub portu (np. brak serwera).

---

## 8. NAT – Translacja Adresów

NAT jest mechanizmem routera zmieniającym adresy IP w nagłówku pakietu, najczęściej umożliwiając wielu hostom prywatnym korzystanie z jednego **publicznego adresu IP**.

* **SNAT (Source NAT):** Zmienia adres **źródłowy** (pakiet wychodzący z LAN do Internetu).
* **DNAT (Destination NAT):** Zmienia adres **docelowy** (przekierowanie z Internetu na hosta w LAN, np. port forwarding).
* **PAT (Port Address Translation):** Najczęściej używany. Używa **różnych numerów portów źródłowych** dla każdego hosta, aby mapować wiele prywatnych IP na jedno publiczne (zwany też **NAT Overload**).

---

## 9. Routing — Jak Pakiety Znajdują Drogę? 

**Routing** to proces wybierania najlepszej trasy dla pakietu IP, wykonywany **wyłącznie na podstawie adresów IP** i tablic routingu.

**Proces Routera (Forwarding):**
1.  Router odbiera pakiet.
2.  Odczytuje adres **IP docelowy**.
3.  Sprawdza **tablicę routingu**.
4.  Wybiera najlepszy wpis (zasada **najdłuższego dopasowania maski**).
5.  Określa **Next-Hop** (następny router lub interfejs wyjściowy).
6.  Tworzy nową ramkę MAC i wysyła pakiet dalej.

**Przykład Wpisu:** `0.0.0.0/0` $\rightarrow$ Next-Hop `192.168.1.1` to **Default Route (Brama Domyślna)**, używana, gdy nie ma bardziej szczegółowego dopasowania (pakiety do Internetu).

---

## 10. Rodzaje Routingu

### Statyczny Routing
* **Opis:** Administrator ręcznie konfiguruje wszystkie trasy.
* **Zalety:** Prosty, bezpieczny, przewidywalny.
* **Wady:** Nie skaluje się, brak automatycznej reakcji na awarie.

### Dynamiczny Routing
* **Opis:** Routery automatycznie wymieniają informacje o trasach.

**Protokoły:**
* **IGP (Interior Gateway Protocols) - wewnątrz AS:** OSPF, EIGRP (Cisco).
* **EGP (Exterior Gateway Protocol) - między AS:** BGP (ustala globalne trasy w Internecie).

**Metryki Routingu:** Protokoły używają metryk do określenia **"najlepszej"** trasy:
* **OSPF:** Koszt (oparty na przepustowości).
* **RIP:** Ilość skoków (hops).

---

## 11. IPv6 – Krótka Charakterystyka

IPv6 jest następcą IPv4, rozwiązującym problem braku adresów.

* **Adresacja:** **128-bitowa** (niemal nieskończona liczba adresów).
* **Format:** Szesnastkowy (np. `2001:db8:acad:1::1`).
* **Cechy:** Wbudowane mechanizmy autokonfiguracji (SLAAC) i wbudowane wsparcie dla **IPsec** (szyfrowania).
* **Brak NAT:** NAT jest niepotrzebny, ponieważ każdy host może mieć publiczny adres.
* **Brak Fragmentacji:** Fragmentacja może być wykonywana tylko przez host źródłowy (routery jej nie wykonują).

---

## 12. Traceroute — Jak Działa?

Traceroute mapuje drogę pakietu, wykorzystując mechanizm **TTL** i komunikaty **ICMP Time Exceeded**.
1.  Wysyła pierwszy pakiet z **TTL=1**. Pierwszy router go wyrzuca i odsyła **ICMP Time Exceeded** (zdradzając swój adres).
2.  Wysyła drugi pakiet z **TTL=2**. Drugi router go wyrzuca i odsyła ICMP.
3.  Proces powtarza się, aż pakiet dotrze do celu.

---

## 13. Jak Przebiega Przesył Pakietu IP – Przykład

Host A (192.168.1.10) do Serwera B (142.250.180.14):
1.  Warstwa TCP tworzy segment.
2.  Warstwa IP tworzy pakiet (źródło: 192.168.1.10, cel: 142.250.180.14, TTL np. 64).
3.  Host A ustala, że cel jest poza LAN $\rightarrow$ pakiet musi trafić do **bramy domyślnej** (routera).
4.  ARP znajduje MAC routera.
5.  Ramka Ethernet jest wysyłana do routera.
6.  Router odczytuje IP docelowe, sprawdza **tablicę routingu**.
7.  Router wybiera **next-hop**, usuwa starą ramkę MAC i tworzy nową. **Zmniejsza TTL o 1**.
8.  Pakiet trafia do serwera. Serwer odpowiada pakietem zwrotnym.

---

## 14. Podsumowanie Warstwy Sieciowej

Warstwa sieciowa (Internetu) odpowiada za:
* **Adresowanie (IP)** i **Routing** (wybór trasy).
* **TTL**, **Fragmentację** i pomocnicze protokoły (**ARP, ICMP**).
* **Przekazywanie pakietów** między różnymi sieciami (routery).
* Umożliwia globalną komunikację (Internet).