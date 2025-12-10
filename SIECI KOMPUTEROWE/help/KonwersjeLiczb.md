# KONWERSJA LICZB — PEŁNE I PROSTE OPRACOWANIE

Systemy liczbowe są podstawą całej informatyki. Reprezentują wartości za pomocą różnych podstaw (baz).

* **Dziesiętny (Dec)** – Podstawa **10**
* **Dwójkowy (Bin)** – Podstawa **2**
* **Ósemkowy (Okt)** – Podstawa **8**
* **Szesnastkowy (Hex)** – Podstawa **16**

> **W Informatyce:** Najczęściej używamy: **Binarny ↔ Szesnastkowy ↔ Dziesiętny**.

---

## 1. Systemy Liczbowe — Krótkie Wyjaśnienie

### System Dziesiętny (Base 10)
* **Cyfry:** 0–9
* **Zasada:** Wartość to suma iloczynów cyfry i potęgi podstawy (10).
* **Przykład:** $482 = 4 \cdot 10^2 + 8 \cdot 10^1 + 2 \cdot 10^0$

### System Dwójkowy (Binarny, Base 2) 
* **Cyfry:** 0 i 1 (bity)
* **Zasada:** Każda pozycja to potęga 2.
* **Przykład:** $1011_2 = 1 \cdot 2^3 + 0 \cdot 2^2 + 1 \cdot 2^1 + 1 \cdot 2^0 = 8 + 0 + 2 + 1 = 11_{10}$

### System Ósemkowy (Oktalny, Base 8)
* **Cyfry:** 0–7
* **Zastosowanie:** Rzadziej, np. do ustawiania uprawnień w systemach Linux (`chmod 755`).
* **Kluczowa Właściwość:** $1$ cyfra ósemkowa $= 3$ bity binarne.

### System Szesnastkowy (Hex, Base 16)
* **Cyfry:** 0–9 oraz A, B, C, D, E, F.
    * A=10, B=11, C=12, D=13, E=14, F=15.
* **Zasada:** Każda pozycja to potęga 16.
* **Przykład:** $1A_{16} = 1 \cdot 16^1 + 10 \cdot 16^0 = 16 + 10 = 26_{10}$
* **Kluczowa Właściwość:** $1$ cyfra szesnastkowa $= 4$ bity binarne (jest to **najbardziej kompaktowy** zapis danych binarnych).

---

## 2. Tabela Wartości (Fundament Konwersji)

Ta tabela pokazuje, jak wygląda 4-bitowa reprezentacja (tzw. nibble):

| Wartość Dziesiętna | Binarnie | Hex |
| :---: | :---: | :---: |
| 0 | 0000 | 0 |
| 1 | 0001 | 1 |
| 7 | 0111 | 7 |
| 8 | 1000 | 8 |
| 10 | 1010 | A |
| 15 | 1111 | F |

---

## 3. Konwersje Między Systemami

### 3.1. Dziesiętny $\rightarrow$ Binarny
**Metoda dzielenia przez 2.**

**Przykład:** Konwersja $25_{10}$
* $25 / 2 = 12$ reszta **1**
* $12 / 2 = 6$ reszta **0**
* $6 / 2 = 3$ reszta **0**
* $3 / 2 = 1$ reszta **1**
* $1 / 2 = 0$ reszta **1**

> **Wynik:** Czytamy reszty od dołu do góry: $\mathbf{25_{10} = 11001_2}$ 

### 3.2. Binarny $\rightarrow$ Dziesiętny
**Metoda sumowania potęg.**

**Przykład:** Konwersja $101101_2$
$1 \cdot 2^5 + 0 \cdot 2^4 + 1 \cdot 2^3 + 1 \cdot 2^2 + 0 \cdot 2^1 + 1 \cdot 2^0$
$= 32 + 0 + 8 + 4 + 0 + 1$
$= 45$

> **Wynik:** $\mathbf{101101_2 = 45_{10}}$

### 3.3. Dziesiętny $\rightarrow$ Hex
**Metoda dzielenia przez 16.**

**Przykład:** Konwersja $254_{10}$
* $254 / 16 = 15$ reszta $\mathbf{14 \text{ (E)}}$
* $15 / 16 = 0$ reszta $\mathbf{15 \text{ (F)}}$

> **Wynik:** Czytamy reszty od dołu do góry: $\mathbf{254_{10} = FE_{16}}$

### 3.4. Hex $\rightarrow$ Dziesiętny
**Metoda sumowania potęg 16.**

**Przykład:** Konwersja $3B_{16}$
$3 \cdot 16^1 + 11 \cdot 16^0 = 48 + 11 = 59$

> **Wynik:** $\mathbf{3B_{16} = 59_{10}}$

### 3.5. Binarny $\leftrightarrow$ Hex (Najszybsza Konwersja)

#### Binarny $\rightarrow$ Hex
**Grupowanie po 4 bity (od prawej).**
* **Przykład:** $11010110_2$
    * $1101 = D$
    * $0110 = 6$
    * **Wynik:** $\mathbf{D6_{16}}$

#### Hex $\rightarrow$ Binarny
**Rozwijanie każdej cyfry na 4 bity.**
* **Przykład:** $AF_{16}$
    * $A = 1010$
    * $F = 1111$
    * **Wynik:** $\mathbf{10101111_2}$

### 3.6. Binarny $\leftrightarrow$ Oktalny
#### Binarny $\rightarrow$ Oktalny
**Grupowanie po 3 bity (od prawej).**
* **Przykład:** $101110_2$
    * $101 = 5$
    * $110 = 6$
    * **Wynik:** $\mathbf{56_8}$

#### Oktalny $\rightarrow$ Binarny
**Rozwijanie każdej cyfry na 3 bity.**
* **Przykład:** $73_8$
    * $7 = 111$
    * $3 = 011$
    * **Wynik:** $\mathbf{111011_2}$

---

## 4. Konwersje Praktyczne (Maski Sieci)

Znajomość konwersji jest kluczowa w sieciach, zwłaszcza przy analizie masek podsieci (CIDR).

| Dziesiętna Wartość | Binarnie |
| :---: | :---: |
| 255 | **11111111** |
| 254 | **11111110** |
| 240 | **11110000** |
| 224 | **11100000** |
| 192 | **11000000** |
| 128 | **10000000** |
| 0 | 00000000 |

**Przykład: Maska /24**
Maska /24 oznacza, że 24 bity są ustawione na 1 (część sieci).
$\mathbf{255.255.255.0} = 11111111.11111111.11111111.00000000$

---

## 5. Szybkie Triki i Zasady

* **1. Hex $\leftrightarrow$ Binarny jest natychmiastowy:** Ponieważ 1 cyfra Hexa to dokładnie 4 bity, konwersja polega tylko na zamianie/grupowaniu.
* **2. Potęgi dwójki:** Warto znać potęgi $2^0$ do $2^{10}$ (np. $2^8 = 256$, $2^{10} = 1024$) — ułatwia to natychmiastową konwersję Bin $\rightarrow$ Dec.
* **3. Błąd:** Najczęstszy błąd to zapomnienie, że system jest **pozycyjny** (wartość zależy od pozycji i potęgi podstawy).
* **4. Notacja:** Pamiętaj o indeksie dolnym: $\mathbf{25_{10}}$ (dziesiętnie), $\mathbf{11001_2}$ (binarnie), $\mathbf{FE_{16}}$ (hex).

---

## 6. Podsumowanie

Systemy liczbowe są **fundamentem** adresacji, sieci, pamięci i programowania.

**Najważniejsze umiejętności:**
* Konwersja **dziesiętny $\leftrightarrow$ binarny** (metodą potęg i dzielenia).
* Konwersja **binarny $\leftrightarrow$ hex** (metodą grupowania 4-bitowego).
* Interpretacja **binarnej maski sieci** w kontekście CIDR (/X).