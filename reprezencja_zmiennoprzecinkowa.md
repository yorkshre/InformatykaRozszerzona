## 1. Reprezentacja Stałoprzecinkowa (Stałopozycyjna)

W reprezentacji stałoprzecinkowej określoną liczbę bitów przeznacza się na część całkowitą, a określoną na część ułamkową. Położenie przecinka jest sztywne i niezmienne dla wszystkich liczb w danej zmiennej.

* **Główna wada:** Bardzo ograniczony zakres wartości. Dla dużych liczb brakuje dokładności ułamkowej, a dla bardzo małych – zakresu całkowitego.
* **Przykład z podręcznika (C++ przekształcony na Python):** Wielokrotne dodawanie małej liczby dziesiętnej (np. `0.1`) prowadzi do widocznego błędu sumowania.

### Przykład w Pythonie: Błędy precyzji sumowania

```python
N = 100000
liczba = 0.1
suma = 0.0

for _ in range(N):
    suma += liczba

print(f"Wynik dodawania: {suma}")        # Wynik niedokładny (np. 9998.99...)
print(f"Oczekiwany wynik: {N * liczba}")  # 10000.0
print(f"Błąd bezwzględny: {abs(suma - (N * liczba))}")
```
## 2. Reprezentacja Zmiennoprzecinkowa (Zmiennopozycyjna)

Standard IEEE 754 pozwala reprezentować szeroki zakres liczb za pomocą notacji wykładniczej:
$$x = (-1)^{\text{znak}} \times \text{mantysa} \times 2^{\text{cecha}}$$

Liczba składa się z trzech pól:
* **Znak** (0 dla dodatnich, 1 dla ujemnych)
* **Cecha** (wykładnik zapisany w kodzie z nadmiarem / bias)[cite: 1]
* **Mantysa** (część ułamkowa w postaci znormalizowanej)[cite: 1]

---

## 3. Typy Zmiennoprzecinkowe w Praktyce (Standard IEEE 754)

| Nazwa typu | Rozmiar | Znak | Cecha | Mantysa | Zakres i dokładność |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **float** (pojedyncza) | 4 bajty (32 bity)| 1 bit] | 8 bitów (bias 127) | 23 bity | $\sim 1.18 \cdot 10^{-38}$ do $3.40 \cdot 10^{38}$ (dokładność ~7 cyfr) |
| **double** (podwójna) | 8 bajtów (64 bity) | 1 bit | 11 bitów (bias 1023) | 52 bity | $\sim 2.23 \cdot 10^{-308}$ do $1.79 \cdot 10^{308}$ (dokładność ~15 cyfr) |

## Zadanie 1: Analiza liczby w standardzie IEEE 754 (Pojedyncza precyzja)

**Zadanie:** Dana jest 32-bitowa liczba zmiennoprzecinkowa w formacie IEEE 754, której reprezentacja szesnastkowa to `0xC1200000`. Wyznacz jej znak, cechę, mantysę oraz wartość dziesiętną.

### Krok 1: Konwersja z systemu szesnastkowego na binarny
Liczba szesnastkowa `0xC1200000` składa się z 8 cyfr heksadecymalnych, co daje dokładnie 32 bity (każda cyfra to 4 bity):
* `C` = `1100`
* `1` = `0001`
* `2` = `0010`
* `0` = `0000` (x5)

Złożone w jeden ciąg 32 bitów:
`1100 0001 0010 0000 0000 0000 0000 0000`

### Krok 2: Podział na pola zgodnie ze standardem IEEE 754 (float)
Format pojedynczej precyzji składa się z:
1. **Bit znaku ($S$):** 1. bit (indeks 0)
2. **Cecha ($E$):** kolejne 8 bitów (indeksy 1–8)
3. **Mantysa ($M$):** pozostałe 23 bity (indeksy 9–31)

Rozpisując nasz ciąg bitów:
* **Znak ($S$):** `1` $\rightarrow$ Liczba jest ujemna ($(-1)^1 = -1$).
* **Cecha ($E$):** `1000 0010`
* **Mantysa ($M$):** `010 0000 0000 0000 0000 0000`

### Krok 3: Obliczenie wartości cechy
Wartość binarna `1000 0010` to w systemie dziesiętnym:
$$128 + 2 = 130$$

Ponieważ cecha w IEEE 754 jest zapisywana w kodzie z nadmiarem (bias) równym $127$ dla typu `float`, rzeczywisty wykładnik potęgi dwójki ($e$) wynosi:
$$e = E - \text{bias} = 130 - 127 = 3$$

### Krok 4: Wyznaczenie mantysy
Mantysa w postaci znormalizowanej ma domyślny (ukryty) bit równy $1$ przed przecinkiem:
$$\text{Mantysa} = 1 + \sum_{i=1}^{23} b_i \cdot 2^{-i}$$

W naszym przypadku bity mantysy to po przecinku: `010` (resztę stanowią zera):
* Pierwszy bit po przecinku ($2^{-1}$): `0` $\rightarrow 0$
* Drugi bit po przecinku ($2^{-2}$): `1` $\rightarrow 0.25$
* Trzeci bit po przecinku ($2^{-3}$): `0` $\rightarrow 0$

Zatem wartość mantysy wynosi:
$$1 + 0.25 = 1.25$$

### Krok 5: Złożenie całości (Obliczenie wartości dziesiętnej)
Korzystając ze wzoru:
$$x = (-1)^{\\text{znak}} \\times \\text{mantysa} \\times 2^{\\text{cecha}}$$
$$x = (-1)^1 \\times 1.25 \\times 2^3 = -1 \\times 1.25 \\times 8 = -10.0$$

* **Wynik:** Szukana liczba dziesiętna to **$-10,0$**.
### Weryfikacja w Pythonie:
Możesz sprawdzić ten wynik bezpośrednio w kodzie Pythona:

```python
import struct

# Konwersja z zapisu szesnastkowego 0xC1200000 na float
hex_str = "C1200000"
packed_bytes = bytes.fromhex(hex_str)
liczba = struct.unpack('!f', packed_bytes)[0]

print(f"Obliczona wartość: {liczba}")  # Wynik: -10.0
```

## Zadanie 2: Pułapki arytmetyki zmiennoprzecinkowej

**Zadanie:** Wyjaśnij pojęcie **błędu ulotnego (ang. *rounding error*)** oraz podaj przykład operacji matematycznej, w której prawo łączności dodawania ($(a + b) + c = a + (b + c)$) **nie jest spełnione** przy użyciu typów zmiennoprzecinkowych komputera.

### 1. Czym jest błąd ulotny (zaokrągleń)?
Błąd ulotny (ang. *rounding error* lub *round-off error*) wynika z faktu, że komputery przechowują liczby rzeczywiste na skończonej liczbie bitów (np. 32 lub 64 bity). Wiele liczb w układzie dziesiętnym (np. $0.1$) ma w systemie binarnym nieskończone rozwinięcie okresowe. Ponieważ mantysa mieści tylko ograniczoną liczbę bitów, końcówka rozwinięcia binarnego jest bezwzględnie odcinana (zaokrąglana). Kumulacja takich mikro-błędów w długich ciągach obliczeń (np. pętlach sumujących) prowadzi do zauważalnych odchyleń od wartości teoretycznych.

### 2. Dlaczego prawo łączności dodawania nie działa w komputerze?
W matematyce analitycznej dodawanie jest łączne: $(a + b) + c = a + (b + c)$. 
W informatyce, ze względu na konieczność zaokrąglania wyników do określonej precyzji na każdym kroku operacji, reguła ta przestaje obowiązywać, zwłaszcza gdy operujemy na liczbach o **skrajnie różnych rzędach wielkości**.

#### Przykład demonstracyjny w Pythonie:
Wyobraźmy sobie dodawanie bardzo dużej liczby do bardzo małej, a następnie drugiej dużej liczby. 

```python
# Przykład ilustrujący utratę precyzji przy różnych rzędach wielkości:
a = 1e16
b = -1e16
c = 1.0

# Operacja 1: (a + b) + c
krok1_lewa = (a + b) + c   # (1e16 + (-1e16)) + 1.0 -> 0.0 + 1.0 = 1.0

# Operacja 2: a + (b + c)
# Ponieważ 'b' (-1e16) i 'c' (1.0) różnią się o 16 rzędów wielkości,
# 'c' "nie mieści się" w mantysie typu float/double przy dodawaniu do 'b',
# przez co zostaje całkowicie zignorowane (pochłonięte)!
krok2_prawa = a + (b + c)  # 1e16 + (-1e16 + 1.0) -> 1e16 + (-1e16) = 0.0

print(f"Wynik (a + b) + c = {krok1_lewa}")
print(f"Wynik a + (b + c) = {krok2_prawa}")
print(f"Czy są równe? {krok1_lewa == krok2_prawa}")  # Zwróci False!
```
