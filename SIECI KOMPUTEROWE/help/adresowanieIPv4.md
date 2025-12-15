# IPv4 — teoria + zadania (podsieć, CIDR, VLSM)

## 1) IPv4 — podstawy

**IPv4** to 32-bitowy adres logiczny zapisywany jako 4 oktety (0–255), np. `192.168.1.10`.

### Adres IPv4 składa się z dwóch części
- **część sieciowa (Network ID)** – wspólna dla całej podsieci
- **część hosta (Host ID)** – identyfikuje urządzenie w tej podsieci

Granice między nimi wyznacza **maska podsieci** (np. `255.255.255.0`) lub zapis **CIDR** (np. `/24`).

### CIDR i maska
- `/24` oznacza: 24 bity „1” w masce, 8 bitów „0” dla hostów  
  `255.255.255.0`
- ogólnie:  
  **liczba bitów hosta = 32 − prefix**  

### Najważniejsze adresy w podsieci
- **adres sieci (Network address)**: wszystkie bity hosta = 0  
- **broadcast**: wszystkie bity hosta = 1  
- **hosty**: wartości pomiędzy (bez sieci i broadcast)

### Liczba hostów w podsieci
Jeśli mamy `h` bitów hosta, to:
- **adresów łącznie**: `2^h`
- **hostów użytecznych**: `2^h − 2` (odejmujemy sieć i broadcast)

### Skok podsieci (increment)
Skok zależy od maski. Najczęściej liczymy go w tym oktecie, gdzie maska nie jest 255 ani 0:
- skok = `256 − wartość_oktetu_maski`  
Np. `/26` → maska `255.255.255.192` → skok `256 − 192 = 64`  
Podsieci zaczynają się co 64: `.0, .64, .128, .192`

### VLSM (Variable Length Subnet Mask) — zasady
W VLSM tworzymy podsieci o różnych rozmiarach. Procedura:
1) sortujemy wymagania hostów **malejąco**
2) dla każdej sieci dobieramy najmniejszą podsieć, która pomieści hosty  
3) przydzielamy kolejne zakresy od początku puli

---

## Zadanie 1 – Podział sieci na określoną liczbę podsieci

**Dany adres:** `192.168.10.0/24`  
**Podział:** na **6 podsieci** o równej liczbie hostów.

### Rozwiązanie
Aby mieć ≥6 podsieci, potrzebujemy liczby będącej potęgą 2: `8` podsieci.  
Z `/24` pożyczamy **3 bity** → **/27**.

- **Nowa maska:** `/27` = `255.255.255.224`  
- **Adresów w podsieci:** `2^(32−27)=32`  
- **Hostów:** `32−2=30`  
- **Skok podsieci:** `256−224 = 32`

Wypisujemy pierwsze 6 podsieci:

1) **192.168.10.0/27**  
- sieć: `192.168.10.0`  
- hosty: `192.168.10.1 – 192.168.10.30`  
- broadcast: `192.168.10.31`  
- maska: `255.255.255.224`

2) **192.168.10.32/27**  
- sieć: `192.168.10.32`  
- hosty: `192.168.10.33 – 192.168.10.62`  
- broadcast: `192.168.10.63`

3) **192.168.10.64/27**  
- sieć: `192.168.10.64`  
- hosty: `192.168.10.65 – 192.168.10.94`  
- broadcast: `192.168.10.95`

4) **192.168.10.96/27**  
- sieć: `192.168.10.96`  
- hosty: `192.168.10.97 – 192.168.10.126`  
- broadcast: `192.168.10.127`

5) **192.168.10.128/27**  
- sieć: `192.168.10.128`  
- hosty: `192.168.10.129 – 192.168.10.158`  
- broadcast: `192.168.10.159`

6) **192.168.10.160/27**  
- sieć: `192.168.10.160`  
- hosty: `192.168.10.161 – 192.168.10.190`  
- broadcast: `192.168.10.191`

---

## Zadanie 2 – Minimalna liczba hostów

**Dany adres:** `10.0.0.0/8`  
Wydzielamy podsieć na **min. 500 hostów**.

### Rozwiązanie
Szukamy najmniejszej liczby bitów hosta `h`, aby:
`2^h − 2 ≥ 500`

`2^9 = 512 → 512 − 2 = 510` hostów   
Zatem `h = 9`, więc prefix:
`32 − 9 = /23`

- **Nowa maska:** `/23` = `255.255.254.0`
- **Maks. liczba hostów:** `510`

Pierwsza taka podsieć w `10.0.0.0/8`:
- **adres sieci:** `10.0.0.0/23`
- **broadcast:** `10.0.1.255`
- **hosty:** `10.0.0.1 – 10.0.1.254`

---

## Zadanie 3 – VLSM

**Sieć bazowa:** `172.16.0.0/16`  
Wymagania:

| Sieć | Wymagane hosty |
|------|----------------|
| A    | 1000 |
| B    | 200 |
| C    | 50 |
| D    | 10 |

### Rozwiązanie (VLSM — od największej)

**A: 1000 hostów**  
`2^10 − 2 = 1022` → **/22**  
- maska: `/22` = `255.255.252.0`
- sieć: `172.16.0.0/22`
- broadcast: `172.16.3.255`
- hosty: `172.16.0.1 – 172.16.3.254`

**B: 200 hostów**  
`2^8 − 2 = 254`  → **/24**  
- maska: `/24` = `255.255.255.0`
- sieć: `172.16.4.0/24`
- broadcast: `172.16.4.255`
- hosty: `172.16.4.1 – 172.16.4.254`

**C: 50 hostów**  
`2^6 − 2 = 62`  → **/26**  
- maska: `/26` = `255.255.255.192`
- sieć: `172.16.5.0/26`
- broadcast: `172.16.5.63`
- hosty: `172.16.5.1 – 172.16.5.62`

**D: 10 hostów**  
`2^4 − 2 = 14`  → **/28**  
- maska: `/28` = `255.255.255.240`
- sieć: `172.16.5.64/28`
- broadcast: `172.16.5.79`
- hosty: `172.16.5.65 – 172.16.5.78`

---

## Zadanie 4 – Identyfikacja podsieci

**Sieć:** `192.168.5.0/26`

### Rozwiązanie
`/26` w sieci klasy /24 oznacza pożyczenie `2` bitów → `2^2 = 4` podsieci.

- **liczba podsieci:** `4`
- **bity hosta:** `32−26 = 6`
- **hosty w podsieci:** `2^6 − 2 = 62`
- maska: `255.255.255.192`
- skok: `256 − 192 = 64`

Podsieci i broadcast:
1) `192.168.5.0/26` → broadcast `192.168.5.63`
2) `192.168.5.64/26` → broadcast `192.168.5.127`
3) `192.168.5.128/26` → broadcast `192.168.5.191`
4) `192.168.5.192/26` → broadcast `192.168.5.255`

---

## Zadanie 5 – Do której podsieci należy host?

**Sieć:** `130.50.0.0/20`  
**Host:** `130.50.5.123`

### Rozwiązanie
`/20` → maska `255.255.240.0`  
Skok w 3. oktecie: `256 − 240 = 16` → zakresy: 0–15, 16–31, 32–47, ...

Host ma 3. oktet = `5`, więc należy do zakresu `0–15`.

- **adres sieci:** `130.50.0.0/20`
- **broadcast:** `130.50.15.255`
- **hosty:** `130.50.0.1 – 130.50.15.254`

---

## Zadanie 6 – Podział na podsieci o określonym rozmiarze

**Sieć:** `192.168.100.0/24`  
Podsieci po **30 hostów**.

### Rozwiązanie
30 hostów → potrzebujemy 32 adresów → `/27` (hostów 30)

- maska: `/27` = `255.255.255.224`
- skok: 32

Wszystkie podsieci (/27) w tej /24:

1) `192.168.100.0/27`  
- hosty: `192.168.100.1 – 192.168.100.30`  
- broadcast: `192.168.100.31`

2) `192.168.100.32/27`  
- hosty: `192.168.100.33 – 192.168.100.62`  
- broadcast: `192.168.100.63`

3) `192.168.100.64/27`  
- hosty: `192.168.100.65 – 192.168.100.94`  
- broadcast: `192.168.100.95`

4) `192.168.100.96/27`  
- hosty: `192.168.100.97 – 192.168.100.126`  
- broadcast: `192.168.100.127`

5) `192.168.100.128/27`  
- hosty: `192.168.100.129 – 192.168.100.158`  
- broadcast: `192.168.100.159`

6) `192.168.100.160/27`  
- hosty: `192.168.100.161 – 192.168.100.190`  
- broadcast: `192.168.100.191`

7) `192.168.100.192/27`  
- hosty: `192.168.100.193 – 192.168.100.222`  
- broadcast: `192.168.100.223`

8) `192.168.100.224/27`  
- hosty: `192.168.100.225 – 192.168.100.254`  
- broadcast: `192.168.100.255`

---

## Zadanie 7 – Projektowanie sieci z wykorzystaniem VLSM

**Sieć bazowa:** `192.168.0.0/24`  
Potrzebujemy podsieci:
- Router1 – 60 hostów
- Router2 – 25 hostów
- Router3 – 10 hostów

### Rozwiązanie (VLSM — od największej)

**Router1: 60 hostów**  
`2^6 − 2 = 62`  → **/26**  
- maska: `/26` = `255.255.255.192`
- sieć: `192.168.0.0/26`
- broadcast: `192.168.0.63`
- hosty: `192.168.0.1 – 192.168.0.62`

**Router2: 25 hostów**  
`2^5 − 2 = 30`  → **/27**  
- maska: `/27` = `255.255.255.224`
- sieć: `192.168.0.64/27`
- broadcast: `192.168.0.95`
- hosty: `192.168.0.65 – 192.168.0.94`

**Router3: 10 hostów**  
`2^4 − 2 = 14`  → **/28**  
- maska: `/28` = `255.255.255.240`
- sieć: `192.168.0.96/28`
- broadcast: `192.168.0.111`
- hosty: `192.168.0.97 – 192.168.0.110`

---

## Szybka checklista (żeby nie popełniać błędów)
- zawsze liczymy hosty: `2^(32−prefix) − 2`
- dla „równej liczby podsieci” używamy potęg 2
- skok podsieci: `256 − (oktetu maski)`
- w VLSM przydzielamy od największej do najmniejszej
