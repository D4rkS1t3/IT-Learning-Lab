# WARSTWA APLIKACJI — KOMPLETNE OPRACOWANIE (Model TCP/IP i OSI)

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
3.  **Określa zasady komunikacji** - metody, formaty, struktury komunikatów.
4.  **Używa portów**, aby rozróżniać różne usługi sieciowe na tym samym hoście.
5.  **Zapewnia interfejs użytkownika** - strony, okna, formularze, logowanie.

**Wbudowane Protokoły:** Każda aplikacja ma swój protokół: Przeglądarka -> **HTTP/HTTPS**, Klient poczty -> **SMTP/IMAP/POP3**, Klient FTP -> **FTP/SFTP** itd.

---

## 2. Jak Działa Aplikacja? (Przykład: Przeglądarka → Strona WWW)

Zanim żądanie HTTP zostanie wysłane, przeglądarka musi nawiązać połączenie TCP z serwerem - **zawsze z użyciem adresu IP**.

**KROK 1 — Analiza URL:** Przeglądarka rozbija adres `http://www.strona.pl/plik.html` na: protokół: `http`, nazwę domeny: `www.strona.pl`, zasób: `/plik.html`, port: `80`.

**KROK 2 — Zapytanie DNS o Adres IP:** Przeglądarka pyta resolver DNS o IP. DNS odpowiada np.: **`212.56.93.112`**.

**KROK 3 — Nawiązanie Połączenia TCP (3-way handshake) 

Połączenie odbywa się z adresem `212.56.93.112:80`. Sekwencja: **SYN** -> serwer, **SYN-ACK** -> klient, **ACK** -> serwer.

**KROK 4 — Wysyłanie Żądania HTTP:** Przeglądarka wysyła żądanie:
* `GET /plik.html HTTP/1.1`
* `Host: www.strona.pl`
* ...
W nagłówku **Host** znajduje się nazwa domeny (dla **Virtual Hosts**).

**KROK 5 — Odpowiedź Serwera:** Serwer zwraca:
* Nagłówek odpowiedzi, np.: `HTTP/1.1 200 OK`, `Content-Type: text/html`.
* Treść pliku, np. kod HTML.
* W przypadku błędu: `HTTP/1.1 404 Not Found`.

**KROK 6 - Renderowanie Strony:**
* Przeglądarka analizuje HTML.
* Pobiera dodatkowe zasoby (CSS, JS, obrazy).
* Renderuje stronę.

**Podsumowanie Procesu WWW:** URL -> DNS -> TCP -> HTTP -> odpowiedź -> renderowanie

---

## 3.  Najważniejsze Protokoły Warstwy Aplikacji

### HTTP / HTTPS — Strony WWW

| Protokół | Port | Szyfrowanie |
| :--- | :--- | :--- |
| **HTTP** | **80** | Brak |
| **HTTPS** | **443** | **TLS** (Szyfrowanie) |

**Metody HTTP:** **GET** (pobranie), **POST** (wysłanie danych), **PUT**, **DELETE**, **HEAD**, **OPTIONS**.
**Kody HTTP:** **200 OK**, **301/302** (Przekierowanie), **404** (Nie znaleziono), **500** (Błąd serwera).

### DNS — Zamiana Domen na IP

* **Port:** **53** (UDP/TCP)
* **Rola:** Tłumaczenie nazw domenowych na adresy IP.
* **Odpowiada za:** **rekordy A/AAAA**, **rekordy MX**, **cache’owanie** odpowiedzi.

#### DNS - Pełny Proces Działania 


1.  **KROK 0 - Sprawdzanie Cache:** System sprawdza cache przeglądarki i systemowy cache DNS.
2.  **KROK 1 - Zapytanie do Lokalnego Resolvera:** Resolver (router, 8.8.8.8) jest pytany o IP.
3.  **KROK 2 — Zapytania Rekurencyjne:** Resolver pyta **ROOT SERVERS** -> **TLD .pl** -> **Autorytatywny DNS** -> otrzymuje **IP**.
4.  **KROK 3 — Odpowiedź dla Komputera:** Komputer otrzymuje IP.

### SMTP / POP3 / IMAP — Poczta Elektroniczna

| Protokół | Porty (Typowy) | Rola |
| :--- | :--- | :--- |
| **SMTP** | 25, 465 (S), 587 | **Wysyłanie** poczty |
| **POP3** | 110, 995 (S) | **Odbieranie** (ściąganie) |
| **IMAP** | 143, 993 (S) | **Odbieranie** (synchronizacja) |

**Agenci pocztowi:** **MUA** (klient), **MTA** (transfer), **MDA** (dostarczanie).

### FTP / FTPS — Przesyłanie Plików

* **Port 21** – Połączenie **sterujące**.
* **Port 20** – Kanał **danych**.
* **FTPS** = FTP + TLS.

### SSH + SFTP — Zdalne Logowanie i Przesyłanie Plików

* **Port:** **22**.
* **Zapewnia:** Szyfrowane połączenie, bezpieczne uwierzytelnianie.

**Działanie SSH:** Nawiązanie TCP -> Serwer wysyła **klucz publiczny** -> Uzgodnienie algorytmów -> Generowanie i wymiana **klucza sesji** -> **Uwierzytelnianie** (Hasło lub **Klucze SSH**) -> Zdalna sesja jest **szyfrowana**.

### DHCP — Automatyczny Przydział Adresów IP

* **Porty:** **67** (serwer), **68** (klient).
* **Przydziela:** Adres IP, maskę, bramę, DNS.

---

## 4. Porty TCP/UDP — Identyfikacja Usług

| Port | Protokół | Zastosowanie |
| :--- | :--- | :--- |
| **80** | HTTP | Strony WWW |
| **443** | HTTPS | Szyfrowane strony WWW |
| **53** | DNS | Zamiana nazw |
| **25** | SMTP | Poczta (wysyłanie) |
| **22** | SSH | Zdalny dostęp |

---

## 5.  Bezpieczeństwo Warstwy Aplikacji

**Zagrożenia:** Przechwycenie danych w HTTP, phishing, podatności aplikacji (SQLi).
**Współczesne Standardy:** HTTP -> **HTTPS**, FTP -> **SFTP/FTPS**, Telnet -> **SSH**.

---

## 6. Najważniejsze Podsumowanie

Warstwa aplikacji to miejsce, gdzie **powstaje treść** przesyłana przez Internet — strony, maile, pliki, loginy, wiadomości. Niższe warstwy dbają tylko o dostarczenie tych danych do celu.