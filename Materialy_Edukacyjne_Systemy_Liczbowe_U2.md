# Materiał dydaktyczny: Systemy liczbowe i kod U2 z zadaniami i programowaniem w Pythonie

Kompleksowy przewodnik i materiał edukacyjny zawierający teorię na temat systemów liczbowych, konwersji, kodu U2, przykłady kodu oraz zestaw zadań do samodzielnego rozwiązania.

---

## 1. Systemy liczbowe w informatyce

System liczbowy to zbiór zasad i cyfr służących do zapisu liczb. W informatyce kluczowe znaczenie mają cztery systemy:

* **Dziesiętny (Dec):** baza 10, cyfry 0–9 (używany na co dzień przez ludzi).
* **Binarny (Bin):** baza 2, cyfry 0 i 1 (podstawa działania elektroniki cyfrowej).
* **Ósemkowy (Oct):** baza 8, cyfry 0–7.
* **Szesnastkowy (Hex):** baza 16, cyfry 0–9 oraz litery A–F (A=10, B=11, C=12, D=13, E=14, F=15). Stosowany m.in. w adresach pamięci i reprezentacji kolorów.

> **Szybka konwersja dziesiętno-binarna:** Dzielimy liczbę przez 2 i zapisujemy reszty. Czytamy je od dołu do góry. Np. dla $13_{10}$: $13/2=6$ (reszta **1**), $6/2=3$ (reszta **0**), $3/2=1$ (reszta **1**), $1/2=0$ (reszta **1**). Wynik: **$1101_2$**.

---

## 2. Kod U2 (Uzupełnienie do dwóch)

Tradycyjny zapis ze znakiem ma wadę istnienia dwóch zer ($+0$ i $-0$). Współczesne komputery rozwiązują to kodem **U2**:

1. **Krok 1:** Zapisz wartość bezwzględną liczby w kodzie binarnym na $n$ bitach.
2. **Krok 2 (Negacja/U1):** Odwróć wszystkie bity (zamień 0 na 1, a 1 na 0).
3. **Krok 3 (U2):** Dodaj 1 do otrzymanego wyniku (z uwzględnieniem przeniesień).

*Przykład (-5 na 4 bitach):* $+5 \rightarrow <code>0101</code> \rightarrow$ U1: <code>1010</code> $\rightarrow$ U2 (+1): **<code>1011</code>**.

---

## 3. Zadania praktyczne w Pythonie

### Ćwiczenie 1: Podstawowy konwerter systemów liczbowych
```python
def konwertuj_liczbe():
    try:
        wejscie = int(input("Podaj liczbę całkowitą: "))
        print(f"System dziesiętny:    {wejscie}")
        print(f"System binarny:      {bin(wejscie)}")
        print(f"System ósemkowy:     {oct(wejscie)}")
        print(f"System szesnastkowy: {hex(wejscie)}")
    except ValueError:
        print("To nie jest poprawna liczba całkowita!")

konwertuj_liczbe()
```

### Ćwiczenie 2: Generowanie 8-bitowego kodu U2
```python
def na_kod_u2(liczba):
    if not (-128 <= liczba <= 127):
        return "Liczba spoza 8-bitowego zakresu U2"
    if liczba >= 0:
        return bin(liczba)[2:].zfill(8)
    else:
        return bin((1 << 8) + liczba)[2:]

print(f"Liczba  5 w U2: {na_kod_u2(5)}")     # Oczekiwane: 00000101
print(f"Liczba -5 w U2: {na_kod_u2(-5)}")    # Oczekiwane: 11111011
```

---

## 4. Zadania do samodzielnego rozwiązania

### Zadanie A: Konwersje i analiza bez U2
* **Część papierowa (ręczna):**
  * Zamień liczbę szesnastkową **$2F_{16}$** na system dziesiętny ($10$).
  * Następnie zamień tę samą liczbę **$2F_{16}$** bezpośrednio na system binarny ($2$), rozpisując każdą cyfrę szesnastkową na 4 bity.
* **Część programistyczna (Python):** Napisz funkcję `binarny_na_dziesietny(tekst_binarny)`, która zamienia string binarny na liczbę dziesiętną bez użycia wbudowanego `int(..., 2)`.

```python
def binarny_na_dziesietny(tekst_binarny):
    wynik = 0
    for bit in tekst_binarny:
        wynik = wynik * 2 + int(bit)
    return wynik

# Test funkcji:
# print(binarny_na_dziesietny("1101"))  # Powinno zwrócić 13
# print(binarny_na_dziesietny("1010"))  # Powinno zwrócić 10
```

### Zadanie B: Analiza i kodowanie w U2
* **Część papierowa (ręczna):**
  * Zapisz liczbę **$-13$** w postaci 8-bitowego kodu U2 (podaj wartość $+13$ binarnie, kod U1 oraz kod U2 po dodaniu jedynki).
* **Część programistyczna (Python):** Napisz funkcję `czytaj_u2(tekst_u2)`, która przyjmuje 8-bitowy string w kodzie U2 i zwraca wartość dziesiętną.

```python
def czytaj_u2(tekst_u2):
    if tekst_u2[0] == '1':
        waga_starszego = -128
        reszta_wartosc = int(tekst_u2[1:], 2)
        return waga_starszego + reszta_wartosc
    else:
        return int(tekst_u2, 2)

# Testy:
# print(czytaj_u2("11111011"))  # Oczekiwane: -5
# print(czytaj_u2("00000101"))  # Oczekiwane: 5