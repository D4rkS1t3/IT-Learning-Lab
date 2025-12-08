# WARSTWA TRANSPORTOWA – OPRACOWANIE

Warstwa transportowa (TCP/UDP) działa **pomiędzy warstwą aplikacji a warstwą sieciową (IP)**. 
Odpowiada za logiczne dostarczanie danych **między procesami** (aplikacjami) na dwóch różnych urządzeniach.

To tutaj działają główne protokoły:
* **TCP (Transmission Control Protocol)** – protokół połączeniowy, niezawodny.
* **UDP (User Datagram Protocol)** – protokół bezpołączeniowy, szybki.

---

## 1. Zadania Warstwy Transportowej

1.  **Multiplexing / Demultiplexing**
    * Rozróżnienie wielu usług i aplikacji na jednym hoście za pomocą **portów** (np. 80 = HTTP, 443 = HTTPS, 22 = SSH).
2.  **Podział i składanie danych**
    * Duże dane z aplikacji są dzielone na **segmenty/datagramy TCP/UDP**.
    * Segmenty/datagramy trafiają do IP, które tworzy pakiety.
3.  **Ustalanie Portów**
    * **Port źródłowy:** Identyfikacja aplikacji u nadawcy.
    * **Port docelowy:** Identyfikacja usługi u odbiorcy.
4.  **Niezawodność (w TCP)**
    * Gwarancja dostarczenia, poprawności i kolejności segmentów.
    * Osiągana przez: **potwierdzenia (ACK)**, **retransmisje**, **numerację sekwencyjną**.
5.  **Kontrola Przepływu i Przeciążenia (w TCP)**
    * **Flow Control:** Odbiorca reguluje tempo, chroniąc się przed przeciążeniem.
    * **Congestion Control:** Nadawca reguluje tempo, chroniąc sieć przed przeciążeniem.
6.  **Komunikacja Szybkiego Typu (UDP)**
    * Nieniezawodna (best-effort), minimalny narzut nagłówka.

---

## 2. Nagłówek TCP – Pełna Interpretacja 

Nagłówek TCP jest rozbudowany i zawiera mechanizmy niezbędne do ustanawiania połączenia, sterowania przepływem i niezawodności.

| Pole (bity) | Wartość | Opis |
| :--- | :--- | :--- |
| **Source Port (16b)** | Port nadawcy. | Identyfikuje proces źródłowy. |
| **Destination Port (16b)** | Port odbiorcy. | Identyfikuje proces docelowy. |
| **Sequence Number (32b)** | Numer sekwencyjny. | Numer **pierwszego bajtu** w segmencie. Umożliwia składanie strumienia. |
| **Acknowledgment Number (32b)** | Numer potwierdzenia. | **Oczekiwany kolejny** numer sekwencyjny od drugiej strony (**ACK = x+1**). |
| Data offset (4b) | Długość nagłówka TCP. | Wskazuje, gdzie zaczynają się dane. |
| **Flags (9b)** | Flagi sterujące. | Używane do kontroli stanu połączenia. |
| **Window Size (16b)** | Rozmiar okna. | Kluczowy dla kontroli przepływu. Określa ile danych odbiorca może przyjąć. |
| Checksum (16b) | Suma kontrolna. | Kontrola błędów dla nagłówka i danych segmentu. |
| Urgent pointer (16b) | Wskaźnik danych URG. | Określa koniec danych pilnych. |

### Najważniejsze Flagi TCP:

* **SYN:** Ustanawianie połączenia.
* **ACK:** Potwierdzenie odbioru.
* **FIN:** Zakończenie połączenia.
* **RST:** Reset połączenia (natychmiastowe zerwanie).
* **PSH:** Wymusza natychmiastowe dostarczenie danych do aplikacji.
* **URG:** Oznacza dane pilne.

### Najważniejsze Opcje TCP:

* **MSS (Maximum Segment Size):** Największy segment (dane) akceptowany przez odbiorcę.
* **Window Scale:** Rozszerzenie okna TCP ponad 16 bitów.
* **Selective ACK (SACK):** Pozwala selektywnie potwierdzać tylko brakujące fragmenty.
* **Timestamp:** Używany do precyzyjnego pomiaru **RTT** (Round Trip Time).

---

## 3. Uzgadnianie Trójetapowe (Three-way Handshake)

TCP jest protokołem połączeniowym, co oznacza, że musi uzgodnić i ustanowić stan połączenia przed wymianą danych.

1.  **SYN** $\rightarrow$ **Klient** wysyła do serwera (ustala swój numer sekwencyjny **x**).
2.  **SYN-ACK** $\leftarrow$ **Serwer** odpowiada do klienta (ustala swój numer sekwencyjny **y** oraz wysyła potwierdzenie **ACK x+1**).
3.  **ACK** $\rightarrow$ **Klient** potwierdza do serwera (wysyła potwierdzenie **ACK y+1**).

### Cel Handshake:

* **Weryfikacja Gotowości:** Potwierdza, że obie strony są aktywne i gotowe do komunikacji.
* **Ustalenie Numeracji:** Ustanawia początkowe numery sekwencyjne (ISN), niezbędne do składania strumienia danych.
* **Ustalenie Parametrów:** Negocjuje opcje TCP, takie jak **MSS** i **Window Scale**.
* **Zapobieganie Duplikatom:** Chroni przed segmentami należącymi do poprzednich, zakończonych połączeń.

---

## 4. Zamknięcie Połączenia TCP (Four-Packet Handshake)

Zamknięcie połączenia TCP jest **symetryczne**, co oznacza, że każda strona musi oddzielnie zasygnalizować chęć zakończenia wysyłania danych.

### Proces Zamykania 

1.  **FIN** $\rightarrow$ **Nadawca** wysyła flagę FIN (kończy wysyłanie danych).
2.  **ACK** $\leftarrow$ **Odbiorca** potwierdza odebranie FIN.
3.  **FIN** $\leftarrow$ **Odbiorca** wysyła własną flagę FIN (kończy wysyłanie swoich danych).
4.  **ACK** $\rightarrow$ **Nadawca** potwierdza odebranie FIN.

> **Stan TIME-WAIT:** Po zamknięciu, jedna ze stron (zazwyczaj klient) przechodzi w stan **TIME-WAIT**, aby zapewnić, że wszystkie ewentualne duplikaty segmentów z sieci zostaną usunięte.

---

## 5. Okno TCP — Kontrola Przepływu i Wydajności

Okno TCP to ilość danych (w bajtach), które nadawca może wysłać, zanim musi otrzymać **potwierdzenie (ACK)** od odbiorcy.

### Rodzaje Mechanizmów Okna:

#### 1. Flow Control (Kontrola Przepływu)
* **Kto kontroluje:** **Odbiorca** (za pomocą pola **Window Size** w nagłówku).
* **Cel:** Chroni odbiorcę przed **przepełnieniem bufora** i utratą danych.

#### 2. Congestion Control (Kontrola Przeciążenia Sieci)
* **Kto kontroluje:** **Nadawca** (za pomocą wewnętrznego algorytmu, np. **TCP Cubic, TCP Reno**).
* **Cel:** Chroni **sieć** przed przeciążeniem (ang. *congestion*), czyli nadmiernym zatorami w routerach.

### Sposób Działania Congestion Control:

* **Start (Slow Start):** Okno rośnie **wykładniczo** (szybko).
* **Działanie Ciągłe:** Gdy sieć działa dobrze, okno rośnie wolniej, **liniowo** (Congestion Avoidance).
* **Reakcja na stratę:** Gdy wykryje utratę segmentu (brak ACK), nadawca **drastycznie zmniejsza okno**, by odciążyć sieć.
* Dzięki temu mechanizmowi Internet działa stabilnie.

---

## 6. Retransmisje, ACK i RTT

TCP musi aktywnie zarządzać niezawodnością.

1.  **ACK (Potwierdzenia):** Odbiorca informuje, że dane dotarły. ACK number = oczekiwany kolejny bajt.
2.  **Retransmisje:** Gdy ACK nie nadejdzie w określonym czasie (Timeout) lub gdy nadejdą **trzy duplikaty ACK**, segment jest wysyłany ponownie.
3.  **Timeout (RTO):** TCP dynamicznie oblicza czas oczekiwania na ACK, bazując na pomiarach **RTT (Round Trip Time)** – czasu, w jakim segment i jego potwierdzenie wracają.
4.  **SACK (Selective ACK):** Odbiorca może precyzyjnie poinformować, które fragmenty odebrał, a które **konkretnie brakuje**, minimalizując niepotrzebne retransmisje.

---

## 7. Protokół UDP – Prosty i Szybki (Connectionless)

UDP jest minimalnym protokołem transportowym, skupiającym się na szybkości kosztem niezawodności.

### Nagłówek UDP:

* Port źródłowy
* Port docelowy
* Długość datagramu
* Suma kontrolna

### Cechy UDP:

* **Brak połączenia** (stateless).
* **Brak retransmisji, okna i numerów sekwencji.**
* **Brak gwarancji kolejności** ani dostarczenia.
* **Zalety:** Minimalne opóźnienie (latencja), bardzo mały narzut nagłówka.
* **Wady:** Możliwe straty danych, duplikaty.

### Zastosowania UDP:

* **DNS** (większość zapytań)
* **DHCP**
* **Gry Online** (krytyczna jest latencja, straty są tolerowane)
* **VoIP i Streaming** (krótka utrata jest lepsza niż opóźnienie)

---

## 8. TCP vs UDP — Szybkie Porównanie

| Cecha | TCP | UDP |
| :--- | :--- | :--- |
| **Połączenie** | Tak (**Stateful**) | Nie (**Stateless**) |
| **Niezawodność** | Wysoka (Gwarantowana dostawa) | Brak (Best-effort) |
| **Kolejność danych** | Gwarantowana | Brak gwarancji |
| **Ponowne wysyłanie** | Tak (Retransmisje) | Nie |
| **Kontrola ruchu** | Tak (Flow i Congestion Control) | Nie |
| **Opóźnienia** | Większe | Bardzo małe |
| **Zastosowanie** | WWW, SSH, Poczta, FTP | Gry, VoIP, DNS, Streaming |

---

## 9. Polecenie NETSTAT — Analiza Połączeń

`netstat` to narzędzie systemowe kluczowe do analizy stanu warstwy transportowej.

### Windows:
* `netstat -an`: Wyświetla aktywne połączenia i porty nasłuchujące w formie numerycznej.
* `netstat -ano`: Dodaje kolumnę z **PID** (identyfikatorem procesu).

### Stany Połączeń TCP:

* **LISTENING:** Serwer czeka na połączenie SYN.
* **ESTABLISHED:** Połączenie jest aktywne, dane są przesyłane.
* **TIME-WAIT:** Połączenie zostało zamknięte, ale system czeka na usunięcie duplikatów.
* **CLOSE-WAIT:** Serwer odebrał FIN od klienta, ale sam jeszcze nie zamknął swojego końca.

---

## 10. Podsumowanie Warstwy Transportowej

Warstwa transportowa:
* Dostarcza dane **proces $\rightarrow$ proces**.
* Używa **portów** do identyfikacji usług.
* **TCP** zapewnia niezawodność, kolejność i kontrolę przepływu.
* **UDP** zapewnia szybkość i minimalne opóźnienia.
* **Handshake TCP** umożliwia kontrolowaną komunikację.
* **Okno TCP** reguluje tempo transmisji (Flow i Congestion Control).
* **NETSTAT** pozwala analizować połączenia i problemy w czasie rzeczywistym.

To jedna z najważniejszych warstw całego modelu sieciowego, decydująca o wydajności i niezawodności komunikacji.