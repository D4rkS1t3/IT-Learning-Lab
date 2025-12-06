#  WARSTWA APLIKACJI — KOMPLETNE OPRACOWANIE (Model TCP/IP i OSI)

Warstwa aplikacji to **najwyższa warstwa** modelu **TCP/IP** i **OSI**. Tu działają programy, które widzimy i używamy:
* przeglądarki (Chrome, Firefox)
* komunikatory (Messenger, Discord)
* poczta e-mail
* klient FTP / SFTP / SSH
* gry online
* usługi systemowe (DNS, DHCP)

> **Kluczowa Rola:** Warstwa aplikacji **nie wysyła pakietów** - tworzy dane, które niższe warstwy (transportowa, sieciowa itd.) pakują, adresują i przesyłają.

---

## 1. Co Robi Warstwa Aplikacji?

1.  **Tworzy i interpretuje dane** - HTML, wiadomości, pliki, API.
2.  **Korzysta z protokołów aplikacyjnych** - HTTP, DNS, SMTP itd.
3.  **Określa zasady komunikacji** - metody, komunikaty, formaty.
4.  **Używa portów**, aby rozróżniać różne usługi sieciowe na tym samym hoście.
5.  **Zapewnia interfejs użytkownika** - wyświetlanie stron, odbiór maili, logowanie, formularze.

> **Wbudowane Protokoły:** Każda aplikacja ma swój protokół: Przeglądarka $\rightarrow$ **HTTP/HTTPS**, Klient poczty $\rightarrow$ **SMTP/IMAP/POP3**, Klient FTP $\rightarrow$ **FTP/SFTP** itd.

---

## 2.  Jak Działa Aplikacja? (Przykład: Przeglądarka → Strona WWW)

Zanim żądanie HTTP zostanie wysłane, przeglądarka musi nawiązać połączenie TCP z serwerem - **zawsze z użyciem adresu IP**.

**Przykład adresu:** `http://www.strona.pl/plik.html`

**KROK 1 — Analiza URL:** Przeglądarka rozbija adres na: protokół: `http`, nazwę domeny: `www.strona.pl`, zasób: `/plik.html`, port: `80`.

**KROK 2 — Zapytanie DNS o Adres IP:** Przeglądarka pyta resolver DNS: „Jaki jest adres IP dla `www.strona.pl`?”. DNS odpowiada np.: **`212.56.93.112`**.

**KROK 3 — Nawiązanie Połączenia TCP (3-way handshake)**

Połączenie odbywa się z adresem `212.56.93.112:80`. Sekwencja: **SYN** $\rightarrow$ serwer, **SYN-ACK** $\rightarrow$ klient, **ACK** $\rightarrow$ serwer.

**KROK 4 — Wysyłanie Żądania HTTP:** Przeglądarka wysyła żądanie:
* `GET /plik.html HTTP/1.1`
* `Host: www.strona.pl` (Konieczne dla **Virtual Hosts**).
* `User-Agent: Mozilla/5.0`
* `Accept-Language: pl`

**KROK 5 — Odpowiedź Serwera:** Serwer zwraca:
* Nagłówek odpowiedzi, np.: `HTTP/1.1 200 OK`, `Server: Apache/2.4`, `Content-Type: text/html`.
* Treść żądanego pliku HTML.
* W przypadku błędu: `HTTP/1.1 404 Not Found`.

**KROK 6 - Renderowanie Strony:**
* Przeglądarka analizuje kod.
* Pobiera dodatkowe zasoby (CSS, JS, obrazki).
* Renderuje stronę.

> **Podsumowanie Procesu WWW:** URL $\rightarrow$ DNS $\rightarrow$ Połączenie TCP $\rightarrow$ Żądanie HTTP $\rightarrow$ Odpowiedź HTTP $\rightarrow$ Renderowanie strony. To jest **absolutny fundament** działania całej sieci WWW.

---

## 3. Najważniejsze Protokoły Warstwy Aplikacji (Z Opisem Technicznym)

### HTTP / HTTPS — Strony WWW

| Protokół | Port | Rola |
| :--- | :--- | :--- |
| **HTTP** | **80** | Nieszyfrowany transfer danych |
| **HTTPS** | **443** | **HTTP + TLS** (Szyfrowanie) |

**Metody HTTP (Najważniejsze):** **GET** (pobranie), **POST** (wysłanie danych/formularze), **PUT** (zapis/aktualizacja), **DELETE** (usunięcie), **HEAD** (tylko nagłówki), **OPTIONS** (lista metod).
**Kody HTTP:** **200 OK**, **301/302** (Przekierowanie), **400** (Błąd klienta), **404** (Nie znaleziono), **500** (Błąd serwera), **503** (Przeciążenie).

### DNS — Zamiana Domen na IP

* **Port:** **53** (UDP/TCP)
* **DNS wykonuje:** Tłumaczenie domen na IP (**record A/AAAA**), Obsługę rekordów **MX** (poczta), Przechowywanie **stref DNS**, Korzystanie z **cache**.

**Hierarchia DNS:**
* **Root Servers** (13 logicznych) $\rightarrow$ Wiedzą, gdzie są serwery TLD.
* **TLD Servers** (.pl, .com) $\rightarrow$ Wiedzą, kto jest **Autorytatywnym DNS** dla danej domeny.
* **Authoritative DNS Server** $\rightarrow$ Serwer właściciela domeny. Prowadzi rekordy: **A** (IPv4), **AAAA** (IPv6), **MX**, **TXT**, **CNAME**.

#### DNS - Pełny Proces Działania (Pełna Ścieżka Zapytania) 
1.  **Krok 0 - Sprawdzenie Cache:** Sprawdza cache przeglądarki i systemowy cache DNS (`ipconfig /displaydns`).
2.  **Krok 1 - Zapytanie do Lokalnego Resolvera:** Pytanie trafia do lokalnego resolvera (router, 8.8.8.8).
3.  **Krok 2 — Zapytania Rekurencyjne:**
    * Resolver pyta **ROOT** o TLD .pl.
    * Resolver pyta **TLD .pl** o Autorytatywny DNS dla `strona.pl`.
    * Resolver pyta **Autorytatywny DNS** o IP.
    * Resolver zapisuje odpowiedź do cache (na czas **TTL**).
4.  **Krok 3 — Odpowiedź dla Komputera:** Resolver odsyła IP, a przeglądarka nawiązuje połączenie TCP.

### SMTP / POP3 / IMAP — Poczta Elektroniczna

* **SMTP** (Porty: **25 / 465 / 587**) — Wysyłanie poczty.
* **POP3** (Porty: **110 / 995**) — Odbieranie poczty na komputer (ściąganie).
* **IMAP** (Porty: **143 / 993**) — Odbieranie poczty na serwerze (synchronizacja, foldery).

**Agenci pocztowi:** **MUA** (Mail User Agent - klient), **MTA** (Mail Transfer Agent - serwer wysyłający), **MDA** (Mail Delivery Agent - dostarczający do skrzynki).

**Proces dostarczania maila:** Użytkownik $\rightarrow$ MUA $\rightarrow$ MTA. MTA sprawdza odbiorcę: jeśli lokalny $\rightarrow$ MDA dostarcza. Jeśli zewnętrzny $\rightarrow$ MTA wysyła do innego MTA $\rightarrow$ MDA $\rightarrow$ skrzynka odbiorcza.

### FTP / FTPS — Przesyłanie Plików

* **Port 21** – Połączenie **sterujące** (komendy).
* **Port 20** – Kanał **danych** (wymaga dwóch połączeń).
* **FTPS** = FTP + TLS (szyfrowanie).

### SSH + SFTP — Zdalne Logowanie

* **Port:** **22**.
* **Zapewnia:** Szyfrowane połączenie, bezpieczne logowanie. Jest następcą nieszyfrowanego **TELNETA**.

**Jak działa SSH Krok po Kroku:**
1.  Nawiązanie połączenia **TCP** (port 22).
2.  Serwer wysyła swój **klucz publiczny**. Klient zapisuje go w `known_hosts` (ochrona przed **MITM**).
3.  **Uzgodnienie algorytmów** szyfrowania (AES/ChaCha20) i uwierzytelnienia (RSA/ED25519).
4.  **Generowanie klucza sesji** (Session Key): Klient generuje klucz, szyfruje go kluczem publicznym serwera i wysyła. Cała komunikacja jest teraz szyfrowana tym kluczem.
5.  **Uwierzytelnianie użytkownika:**
    * A) Hasło (przesyłane szyfrowane).
    * B) **Klucze SSH** (najbezpieczniejsze): Klient podpisuje wyzwanie kluczem prywatnym $\rightarrow$ Serwer weryfikuje kluczem publicznym (z `authorized_keys`).
6.  **Sesja terminalowa:** Wszystko (polecenia, odpowiedź serwera, pliki SCP/SFTP) jest szyfrowane.

### DHCP — Automatyczny Przydział Adresów IP

* **Porty:** **67** (serwer), **68** (klient).
* **DHCP przydziela:** Adres IP, maskę, bramę, DNS, czas dzierżawy.

---

## 4. Porty TCP/UDP — Identyfikacja Usług

| Port | Protokół | Zastosowanie |
| :--- | :--- | :--- |
| **80** | HTTP | Strony WWW |
| **443** | HTTPS | Szyfrowane strony WWW |
| **53** | DNS | Zamiana nazw |
| **25** | SMTP | Poczta – wysyłanie |
| **110** | POP3 | Poczta – odbiór |
| **143** | IMAP | Poczta – foldery |
| **21** | FTP | Połączenie sterujące |
| **22** | SSH | Zdalne logowanie |
| **23** | Telnet | Nieszyfrowany terminal |
| **3306** | MySQL | Baza danych |
| **5432** | PostgreSQL | Baza danych |

---

## 5. Bezpieczeństwo Warstwy Aplikacji

**Najczęstsze Zagrożenia:**
* **Przechwycenie danych w HTTP** (rozwiązanie: HTTPS).
* **Phishing** (fałszywe strony) i **malware** przesyłające dane.
* **Podatności w serwerach** (np. SQL injection, XSS).
* **Podatne serwery FTP/Telnet**.

**Dlatego Obecnie:**
* HTTP $\rightarrow$ zastąpiono **HTTPS**
* FTP $\rightarrow$ zastępowany **SFTP/FTPS**
* Telnet $\rightarrow$ zastąpiony **SSH**

---

## 6. Najważniejsze Podsumowanie

Warstwa aplikacji to wszystkie protokoły i programy, które **tworzą treść** przesyłaną przez Internet - strony, pocztę, pliki, loginy, wiadomości. Dane powstają tu, a niższe warstwy odpowiadają tylko za ich przesłanie.