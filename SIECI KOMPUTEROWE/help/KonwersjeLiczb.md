#  KONWERSJA LICZB — METODA UNIWERSALNA PRZEZ SYSTEM BINARNY

Systemy liczbowe:
* **Dziesiętny (Dec)** – Podstawa **10**
* **Dwójkowy (Bin)** – Podstawa **2**
* **Ósemkowy (Okt)** – Podstawa **8**
* **Szesnastkowy (Hex)** – Podstawa **16**

> **Metoda Uniwersalna:** Konwersja z dowolnego systemu na dowolny (np. Hex $\leftrightarrow$ Dec, Hex $\leftrightarrow$ Okt) jest **najbardziej niezawodna i jednolita**, gdy odbywa się pośrednio przez **system binarny**.

---

## 1. Systemy Liczbowe i Ich Zastosowanie

### System Dwójkowy (Binarny, Base 2) 
* **Cyfry:** 0 i 1 (bity)
* **Wartość:** Każda pozycja to potęga $2$ ($2^0, 2^1, 2^2, \dots$).
* **Dlaczego Binarnie?** Wynika to z **ograniczeń technologicznych** i natury sprzętu. Komputer operuje stanami elektrycznymi: włączony (1) i wyłączony (0). Cała informatyka sprowadza się do reprezentacji danych jako ciągów tych dwóch stanów.

### System Dziesiętny (Decymalny, Base 10)
* **Cyfry:** 0–9
* **Zastosowanie:** Interfejs użytkownika, wprowadzanie danych przez człowieka. Komputer musi zamienić ten format na binarny do przetwarzania.

### System Szesnastkowy (Hex, Base 16)
* **Cyfry:** 0–9, A–F (A=10, F=15)
* **Właściwość:** $1$ cyfra Hexa $= 4$ bity. Jest to najbardziej **kompaktowy** zapis danych binarnych.
* **Zastosowanie:**
    * **Adresacja Sieci:** Adresy **MAC** (np. `00-1A-2B-3C-4D-5E`).
    * **Programowanie:** Reprezentacja kolorów (np. `#FF0000` dla czerwonego) i pamięci.
    * **Kody Błędów i Hash'e:** Reprezentacja wartości skrótów (np. MD5, SHA256).

### System Ósemkowy (Oktalny, Base 8)
* **Cyfry:** 0–7
* **Właściwość:** $1$ cyfra Oktalna $= 3$ bity. Używany, gdy potrzeba szybkiego i skróconego zapisu 3-bitowych grup.
* **Zastosowanie:** Ustawienia **uprawnień plików** w systemach Linux/Unix (np. `chmod 755` — gdzie każda cyfra reprezentuje 3 bity uprawnień: R-W-X).

---

## 2. Tabela Wartości (4-bitowe Grupy)

| Wartość Dziesiętna | Binarnie (4 bity) | Hex |
| :---: | :---: | :---: |
| 0 | 0000 | 0 |
| 7 | 0111 | 7 |
| 10 | 1010 | A |
| 15 | 1111 | F |

---

## 3. Metoda Konwersji Uniwersalnej (Przez Binarny)

**Wszystkie konwersje opierają się na jednej z dwóch operacji na bitach:**
1.  **Grupowanie (np. z Bin na Hex).**
2.  **Rozwijanie (np. z Hex na Bin).**

### 3.1. Dziesiętny $\leftrightarrow$ Binarny

#### A) Dziesiętny $\rightarrow$ Binarny
* **Metoda:** Dzielenie przez 2 (daje reszty, które są cyframi binarnymi).
* **Przykład:** $25_{10} \rightarrow 11001_2$ (jak w poprzednich notatkach).

#### B) Binarny $\rightarrow$ Dziesiętny
* **Metoda:** Sumowanie potęg 2.
* **Przykład:** $101101_2 \rightarrow 45_{10}$ (jak w poprzednich notatkach).

### 3.2. Konwersje Skrócone (Bin $\leftrightarrow$ Hex i Bin $\leftrightarrow$ Okt)

#### A) Hex $\leftrightarrow$ Binarny (Grupowanie/Rozwijanie po 4 Bity)
* **Hex $\rightarrow$ Bin:** Rozwiń każdą cyfrę Hexa na **4 bity**.
    * **Przykład:** $AF_{16} \rightarrow 1010$ (A) $1111$ (F) $\rightarrow \mathbf{10101111_2}$
* **Bin $\rightarrow$ Hex:** Pogrupuj bity po 4 (od prawej) i zamień na Hex.
    * **Przykład:** $11010110_2 \rightarrow 1101$ (D) $0110$ (6) $\rightarrow \mathbf{D6_{16}}$

#### B) Oktalny $\leftrightarrow$ Binarny (Grupowanie/Rozwijanie po 3 Bity)
* **Okt $\rightarrow$ Bin:** Rozwiń każdą cyfrę Oktalną na **3 bity**.
    * **Przykład:** $73_8 \rightarrow 111$ (7) $011$ (3) $\rightarrow \mathbf{111011_2}$
* **Bin $\rightarrow$ Okt:** Pogrupuj bity po 3 (od prawej) i zamień na Oktal.
    * **Przykład:** $101110_2 \rightarrow 101$ (5) $110$ (6) $\rightarrow \mathbf{56_8}$

### 3.3. Konwersje Pośrednie (Uniwersalna Metoda)

Aby zamienić dowolny system na dowolny, zawsze używaj systemu binarnego jako kroku pośredniego.

#### Przykład 1: Hex $\rightarrow$ Dziesiętny
1.  **Hex $\rightarrow$ Binarny** (Rozwiń po 4 bity).
    * $3B_{16} \rightarrow 0011$ (3) $1011$ (B) $\rightarrow 111011_2$
2.  **Binarny $\rightarrow$ Dziesiętny** (Sumuj potęgi 2).
    * $1 \cdot 2^5 + 1 \cdot 2^4 + 1 \cdot 2^3 + 0 \cdot 2^2 + 1 \cdot 2^1 + 1 \cdot 2^0$
    * $32 + 16 + 8 + 0 + 2 + 1 = \mathbf{59_{10}}$

#### Przykład 2: Ósemkowy $\rightarrow$ Szesnastkowy
1.  **Oktalny $\rightarrow$ Binarny** (Rozwiń po 3 bity).
    * $73_8 \rightarrow 111011_2$
2.  **Binarny $\rightarrow$ Szesnastkowy** (Grupuj po 4 bity).
    * Dopełnij zerami z lewej, by mieć pełne grupy: $0011 \ 1011$
    * $0011 = 3$
    * $1011 = B$
    * **Wynik:** $\mathbf{3B_{16}}$

---

## 4. Konwersje Praktyczne w Sieciach (CIDR)

Zastosowanie konwersji binarnych do masek sieci jest kluczowe, ponieważ maska CIDR (`/X`) jest **liczbą bitów ustawionych na 1**.

**Przykład: Maska /20**
Maska składa się z 20 jedynek i 12 zer (łącznie 32 bity).
$11111111.11111111.11110000.00000000$
* 1. oktet: $11111111_2 = 255_{10}$
* 2. oktet: $11111111_2 = 255_{10}$
* 3. oktet: $11110000_2 = 240_{10}$ ($128+64+32+16$)
* 4. oktet: $00000000_2 = 0_{10}$
* **Wynik:** Maska $/20$ to $\mathbf{255.255.240.0}$

---

## 5. Podsumowanie

**Kluczowe Zasady Konwersji:**
1.  **Binarny to język komputera.** Wszystko musi zostać do niego zamienione.
2.  **Hex i Oktal** są używane jako **skróty** do zapisywania długich ciągów binarnych.
3.  Konwersja między Hex i Bin (4 bity) lub Okt i Bin (3 bity) jest natychmiastowa.
4.  Wszystkie inne konwersje (np. Hex $\leftrightarrow$ Dec) są najpewniejsze, gdy używają **systemu binarnego jako pośrednika**.