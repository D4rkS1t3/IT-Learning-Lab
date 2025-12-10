# SYSTEMY LICZBOWE I KONWERSJE — METODA UNIWERSALNA (BINARNA)

W informatyce używamy głównie czterech systemów pozycyjnych:
* **Dziesiętny (Dec, base 10)**
* **Dwójkowy (Bin, base 2)**
* **Ósemkowy (Oct, base 8)**
* **Szesnastkowy (Hex, base 16)**

> **Najważniejszy jest system binarny.** Używamy go jako **uniwersalnego pośrednika** w każdej konwersji (np. Hex $\rightarrow$ Bin $\rightarrow$ Dec). Ta strategia gwarantuje poprawność i ujednolica wszystkie metody przeliczania.

---

## 1. Dlaczego Komputery Używają Systemu Dwójkowego?

Powód jest **technologiczny i sprzętowy**:
* Układy elektroniczne łatwo rozróżniają **dwa stabilne stany**:
    * **0 (OFF):** Niski poziom napięcia / Brak napięcia.
    * **1 (ON):** Wysoki poziom napięcia / Obecność napięcia.
* System binarny jest **najbardziej odporny na zakłócenia** i błędy, ponieważ zwiększenie liczby stanów (np. 10 w systemie Dec) zwiększa ryzyko błędnej interpretacji.
* **Bramki logiczne** (AND, OR, NOT) operują naturalnie na bitach.

Dlatego wszystko w komputerze jest **binarną reprezentacją**:
* Adresy IP/MAC, instrukcje procesora, dane w pamięci.

---

## 2. Zastosowania Poszczególnych Systemów

### **Binarny (0/1)**
* Podstawowa reprezentacja danych i logiki w elektronice.
* Adresacja pamięci, instrukcje maszynowe, maski sieciowe.

### **Hex (Szesnastkowy)**
* **Właściwość:** $\mathbf{1 \text{ znak hex} = 4 \text{ bity}}$. Jest to **skompresowany** zapis długich ciągów binarnych.
* **Zastosowania:**
    * Adresy **MAC** (`AA:BB:CC:DD:EE:FF`).
    * Kody kolorów w sieci/webie (np. `#FFA500`).
    * Zapis danych w debugowaniu i sieci (`0xFF`, `0x1A`).
    * Adresy **IPv6**.

### **Octal (Ósemkowy)**
* **Właściwość:** $\mathbf{1 \text{ cyfra octal} = 3 \text{ bity}}$. Skuteczny skrót dla 3-bitowych grup.
* **Zastosowania:**
    * Ustawienia **praw dostępu** w systemach Unix/Linux (`chmod 755`), gdzie każda cyfra reprezentuje 3 bity uprawnień (r-w-x).

### **Dziesiętny**
* **Zastosowanie:** Interfejs dla człowieka. Komputer przetwarza go na system binarny, aby móc działać. W sieciach: adresy IPv4, porty TCP/UDP.

---

## 3. Najważniejsza Tabela Konwersji (4-bitowe Grupy) 

| Dec | Bin  | Oct | Hex |
| :---: | :---: | :---: | :---: |
| 0 | 0000 | 0 | 0 |
| 7 | 0111 | 7 | 7 |
| 8 | 1000 | 10 | 8 |
| 10 | 1010 | 12 | A |
| 15 | 1111 | 17 | F |

---

## 4. Uniwersalna Metoda Konwersji (Przez Binarny)

Metoda opiera się na trzech krokach:
1.  **Liczba $\rightarrow$ Binarna** (Rozwinięcie potęgowe).
2.  **Binarna $\rightarrow$ Grupowanie** (Po 3 lub 4 bity, zależnie od celu).
3.  **Grupowanie $\rightarrow$ System Docelowy** (Hex lub Oct).

### 4.1. Dziesiętny $\leftrightarrow$ Binarny (Rozwijanie Potęg)

#### A) Dziesiętny $\rightarrow$ Binarny
* **Metoda:** Rozpisywanie liczby na **sumę potęg 2**.
* **Przykład:** $156_{10}$
    * $156 = 128 + 16 + 8 + 4$ ($2^7 + 2^4 + 2^3 + 2^2$)
    * **Wynik:** $\mathbf{10011100_2}$

#### B) Binarny $\rightarrow$ Dziesiętny
* **Metoda:** Sumowanie potęg 2 dla każdego bitu ustawionego na 1.
* **Przykład:** $101101_2$
    * $1 \cdot 2^5 + 0 \cdot 2^4 + 1 \cdot 2^3 + 1 \cdot 2^2 + 0 \cdot 2^1 + 1 \cdot 2^0 = 32 + 8 + 4 + 1 = 45$
    * **Wynik:** $\mathbf{45_{10}}$

### 4.2. Binarny $\leftrightarrow$ Hex (Grupowanie po 4 Bity)

#### A) Bin $\rightarrow$ Hex
* **Metoda:** Grupuj bity po **cztery** (od prawej), zamień na cyfry Hex. 
* **Przykład:** $11010110_2$
    * $1101 = D$
    * $0110 = 6$
    * **Wynik:** $\mathbf{D6_{16}}$

#### B) Hex $\rightarrow$ Bin
* **Metoda:** Rozwiń każdą cyfrę Hexa na **cztery** bity.
* **Przykład:** $AF_{16}$
    * $A = 1010$
    * $F = 1111$
    * **Wynik:** $\mathbf{10101111_2}$

### 4.3. Binarny $\leftrightarrow$ Octal (Grupowanie po 3 Bity)

#### A) Bin $\rightarrow$ Oct
* **Metoda:** Grupuj bity po **trzy** (od prawej), zamień na cyfry Octal. 
* **Przykład:** $101110_2$
    * $101 = 5$
    * $110 = 6$
    * **Wynik:** $\mathbf{56_8}$

#### B) Oct $\rightarrow$ Bin
* **Metoda:** Rozwiń każdą cyfrę Octalną na **trzy** bity.
* **Przykład:** $73_8$
    * $7 = 111$
    * $3 = 011$
    * **Wynik:** $\mathbf{111011_2}$

---

## 5. Konwersje Pośrednie (Uniwersalna Metoda W Działaniu)

### 5.1. Hex $\rightarrow$ Dziesiętny (Przez Binarny)
1.  **Hex $\rightarrow$ Bin:** $3A_{16} \rightarrow 00111010_2$
2.  **Bin $\rightarrow$ Dec:** $32 + 16 + 8 + 2 = \mathbf{58_{10}}$

### 5.2. Dziesiętny $\rightarrow$ Hex (Przez Binarny)
1.  **Dec $\rightarrow$ Bin:** $230_{10} \rightarrow 11100110_2$
2.  **Bin $\rightarrow$ Hex:** Grupuj po 4: $1110$ ($E$) $0110$ ($6$) $\rightarrow \mathbf{E6_{16}}$

### 5.3. Octal $\rightarrow$ Hex (Przez Binarny)
1.  **Oct $\rightarrow$ Bin:** $73_8 \rightarrow 111011_2$
2.  **Bin $\rightarrow$ Hex:** Grupuj po 4: $0011$ ($3$) $1011$ ($B$) $\rightarrow \mathbf{3B_{16}}$

---

## 6. Konwersje do Wszystkich Systemów w Jednym Przykładzie

Weźmy liczbę: **$125_{10}$**

### 1. Krok Centralny: $125_{10} \rightarrow$ Binarny
$125 = 64 + 32 + 16 + 8 + 4 + 1$
$\rightarrow \mathbf{01111101_2}$

### 2. Bin $\rightarrow$ Hex
Grupujemy po 4: $0111$ ($7$) $1101$ ($D$)
$\rightarrow \mathbf{7D_{16}}$

### 3. Bin $\rightarrow$ Oct
Grupujemy po 3: $011$ ($3$) $111$ ($7$) $101$ ($5$)
$\rightarrow \mathbf{375_8}$

---

## 7. Podsumowanie Najważniejszych Zasad

1.  **Metoda Uniwersalna:** Zawsze zamieniaj liczbę na **binarną** i używaj jej jako podstawy do grupowania/rozwijania do systemu docelowego.
2.  **Hex i Oct** są skrótami binarnego, dlatego konwersja jest natychmiastowa:
    * Hex $\leftrightarrow$ Bin: **4 bity**.
    * Oct $\leftrightarrow$ Bin: **3 bity**.
3.  **Decimal $\leftrightarrow$ Binary** wymaga rozwijania lub dzielenia potęg.

---

## 8. Najważniejsze Jedno Zdanie

> **Zawsze zamieniamy liczby na binarne, a dopiero potem grupujemy według systemu docelowego — to najprostsza i najpewniejsza metoda konwersji, która działa dla wszystkich systemów informatycznych.**