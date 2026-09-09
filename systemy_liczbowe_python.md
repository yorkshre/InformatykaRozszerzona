# Systemy liczbowe w Pythonie

## System binarny, ósemkowy i szesnastkowy

---

## 1. Wprowadzenie

System liczbowy określa sposób zapisywania liczb za pomocą określonego zestawu cyfr.

W informatyce najczęściej wykorzystujemy:

- **system dziesiętny** – podstawa 10,
- **system binarny** – podstawa 2,
- **system ósemkowy** – podstawa 8,
- **system szesnastkowy** – podstawa 16.

W języku Python możemy zarówno zapisywać liczby w różnych systemach, jak i konwertować je pomiędzy systemami.

---

# 2. System dziesiętny

System dziesiętny ma podstawę **10**.

Wykorzystuje cyfry:

```text
0 1 2 3 4 5 6 7 8 9
```

Jest to system, którego używamy na co dzień.

### Przykład

Liczba:

```text
245
```

może zostać przedstawiona jako:

```text
2 · 10² + 4 · 10¹ + 5 · 10⁰
```

czyli:

```text
2 · 100 + 4 · 10 + 5 = 245
```

---

# 3. System binarny

System binarny ma podstawę **2**.

Wykorzystuje tylko dwie cyfry:

```text
0 1
```

System binarny jest szczególnie ważny w informatyce, ponieważ komputery wykorzystują reprezentację opartą na dwóch stanach.

Można je przykładowo interpretować jako:

```text
0 – brak sygnału
1 – obecność sygnału
```

## Przykład

Rozważmy liczbę:

```text
101101₂
```

Rozpisujemy ją według potęg liczby 2:

```text
1 · 2⁵ + 0 · 2⁴ + 1 · 2³ + 1 · 2² + 0 · 2¹ + 1 · 2⁰
```

Obliczamy:

```text
32 + 0 + 8 + 4 + 0 + 1 = 45
```

Zatem:

```text
101101₂ = 45₁₀
```

---

# 4. System ósemkowy

System ósemkowy ma podstawę **8**.

Wykorzystuje cyfry:

```text
0 1 2 3 4 5 6 7
```

W systemie ósemkowym nie występują cyfry `8` i `9`.

## Przykład

```text
345₈
```

Rozpisujemy:

```text
3 · 8² + 4 · 8¹ + 5 · 8⁰
```

czyli:

```text
3 · 64 + 4 · 8 + 5
```

```text
192 + 32 + 5 = 229
```

Zatem:

```text
345₈ = 229₁₀
```

---

# 5. System szesnastkowy

System szesnastkowy ma podstawę **16**.

Wykorzystuje cyfry:

```text
0 1 2 3 4 5 6 7 8 9 A B C D E F
```

Litery oznaczają wartości:

| Znak | Wartość |
|---|---:|
| A | 10 |
| B | 11 |
| C | 12 |
| D | 13 |
| E | 14 |
| F | 15 |

## Przykład

```text
2D₁₆
```

Litera `D` oznacza wartość `13`.

```text
2 · 16¹ + 13 · 16⁰
```

```text
2 · 16 + 13 = 45
```

Zatem:

```text
2D₁₆ = 45₁₀
```

---

# 6. Porównanie systemów liczbowych

| System | Podstawa | Cyfry | Przykład |
|---|---:|---|---|
| Dziesiętny | 10 | 0–9 | `45` |
| Binarny | 2 | 0–1 | `101101` |
| Ósemkowy | 8 | 0–7 | `55` |
| Szesnastkowy | 16 | 0–9, A–F | `2D` |

---

# 7. Zapisywanie liczb w różnych systemach w Pythonie

Python pozwala zapisywać liczby w różnych systemach za pomocą specjalnych prefiksów.

## System binarny

Prefiks:

```text
0b
```

Przykład:

```python
liczba = 0b101101

print(liczba)
```

Wynik:

```text
45
```

---

## System ósemkowy

Prefiks:

```text
0o
```

Przykład:

```python
liczba = 0o55

print(liczba)
```

Wynik:

```text
45
```

---

## System szesnastkowy

Prefiks:

```text
0x
```

Przykład:

```python
liczba = 0x2D

print(liczba)
```

Wynik:

```text
45
```

---

# 8. Dziesiętny → binarny

Do konwersji liczby dziesiętnej na binarną służy funkcja:

```python
bin()
```

### Przykład

```python
liczba = 45

print(bin(liczba))
```

Wynik:

```text
0b101101
```

Prefiks `0b` oznacza system binarny.

Jeżeli chcemy otrzymać tylko cyfry:

```python
liczba = 45

print(bin(liczba)[2:])
```

Wynik:

```text
101101
```

---

# 9. Dziesiętny → ósemkowy

Do konwersji używamy funkcji:

```python
oct()
```

### Przykład

```python
liczba = 45

print(oct(liczba))
```

Wynik:

```text
0o55
```

Bez prefiksu:

```python
print(oct(liczba)[2:])
```

Wynik:

```text
55
```

---

# 10. Dziesiętny → szesnastkowy

Do konwersji wykorzystujemy funkcję:

```python
hex()
```

### Przykład

```python
liczba = 45

print(hex(liczba))
```

Wynik:

```text
0x2d
```

Jeżeli chcemy otrzymać wielkie litery:

```python
print(hex(liczba)[2:].upper())
```

Wynik:

```text
2D
```

---

# 11. Binarny → dziesiętny

Do konwersji wykorzystujemy funkcję:

```python
int()
```

Drugi argument funkcji określa podstawę systemu.

Dla systemu binarnego używamy:

```python
int(liczba, 2)
```

### Przykład

```python
liczba = "101101"

wynik = int(liczba, 2)

print(wynik)
```

Wynik:

```text
45
```

Czyli:

```text
101101₂ = 45₁₀
```

---

# 12. Ósemkowy → dziesiętny

Dla systemu ósemkowego podajemy podstawę `8`.

```python
liczba = "55"

wynik = int(liczba, 8)

print(wynik)
```

Wynik:

```text
45
```

Czyli:

```text
55₈ = 45₁₀
```

---

# 13. Szesnastkowy → dziesiętny

Dla systemu szesnastkowego podajemy podstawę `16`.

```python
liczba = "2D"

wynik = int(liczba, 16)

print(wynik)
```

Wynik:

```text
45
```

Czyli:

```text
2D₁₆ = 45₁₀
```

---

# 14. Najważniejsze funkcje Pythona

| Operacja | Python |
|---|---|
| Dziesiętny → binarny | `bin()` |
| Dziesiętny → ósemkowy | `oct()` |
| Dziesiętny → szesnastkowy | `hex()` |
| Binarny → dziesiętny | `int(x, 2)` |
| Ósemkowy → dziesiętny | `int(x, 8)` |
| Szesnastkowy → dziesiętny | `int(x, 16)` |

### Przykład

```python
liczba = 45

print(bin(liczba))
print(oct(liczba))
print(hex(liczba))
```

Wynik:

```text
0b101101
0o55
0x2d
```

---

# 15. Konwersja pomiędzy systemami

W Pythonie wygodnym sposobem konwersji pomiędzy systemami jest wykorzystanie systemu dziesiętnego jako pośredniego.

Schemat:

```text
system źródłowy
       ↓
system dziesiętny
       ↓
system docelowy
```

## Przykład

Zamieniamy:

```text
101101₂
```

na system szesnastkowy.

Najpierw:

```python
liczba = int("101101", 2)
```

Otrzymujemy:

```text
45
```

Następnie:

```python
print(hex(liczba))
```

Wynik:

```text
0x2d
```

Czyli:

```text
101101₂ = 2D₁₆
```

---

# 16. Binarny → szesnastkowy

System binarny i szesnastkowy są ze sobą szczególnie związane.

Wynika to z zależności:

```text
16 = 2⁴
```

Jedna cyfra szesnastkowa odpowiada czterem bitom.

## Tabela konwersji

| Binarnie | Szesnastkowo |
|---|---|
| `0000` | `0` |
| `0001` | `1` |
| `0010` | `2` |
| `0011` | `3` |
| `0100` | `4` |
| `0101` | `5` |
| `0110` | `6` |
| `0111` | `7` |
| `1000` | `8` |
| `1001` | `9` |
| `1010` | `A` |
| `1011` | `B` |
| `1100` | `C` |
| `1101` | `D` |
| `1110` | `E` |
| `1111` | `F` |

### Przykład

```text
10110111₂
```

Dzielimy liczbę na grupy po cztery bity:

```text
1011 0111
```

Następnie:

```text
1011 = B
0111 = 7
```

Otrzymujemy:

```text
10110111₂ = B7₁₆
```

Python:

```python
liczba = int("10110111", 2)

print(hex(liczba))
```

Wynik:

```text
0xb7
```

---

# 17. Binarny → ósemkowy

Ponieważ:

```text
8 = 2³
```

jedna cyfra ósemkowa odpowiada trzem bitom.

### Przykład

```text
10110111₂
```

Dzielimy od prawej strony na grupy po trzy:

```text
010 110 111
```

Następnie:

```text
010 = 2
110 = 6
111 = 7
```

Otrzymujemy:

```text
10110111₂ = 267₈
```

Python:

```python
liczba = int("10110111", 2)

print(oct(liczba))
```

Wynik:

```text
0o267
```

---

# 18. Pobieranie liczby od użytkownika

Funkcja `input()` pozwala pobrać dane od użytkownika.

## Przykład

Program pobierający liczbę binarną:

```python
liczba = input("Podaj liczbę binarną: ")

dziesietna = int(liczba, 2)

print("Wartość dziesiętna:", dziesietna)
```

Przykład działania:

```text
Podaj liczbę binarną: 101101
Wartość dziesiętna: 45
```

---

# 19. Konwerter liczby binarnej

Możemy rozbudować program:

```python
liczba = input("Podaj liczbę binarną: ")

dziesietna = int(liczba, 2)

print("Dziesiętnie:", dziesietna)
print("Ósemkowo:", oct(dziesietna))
print("Szesnastkowo:", hex(dziesietna))
```

Dla:

```text
101101
```

otrzymamy:

```text
Dziesiętnie: 45
Ósemkowo: 0o55
Szesnastkowo: 0x2d
```

---

# 20. Uniwersalny konwerter

Możemy napisać program obsługujący systemy:

- binarny,
- ósemkowy,
- szesnastkowy.

```python
liczba = input("Podaj liczbę: ")
podstawa = int(input("Podaj podstawę systemu (2, 8 lub 16): "))

dziesietna = int(liczba, podstawa)

print("Dziesiętnie:", dziesietna)
print("Binarnie:", bin(dziesietna))
print("Ósemkowo:", oct(dziesietna))
print("Szesnastkowo:", hex(dziesietna))
```

### Przykład

Dane:

```text
Podaj liczbę: 2D
Podaj podstawę systemu (2, 8 lub 16): 16
```

Wynik:

```text
Dziesiętnie: 45
Binarnie: 0b101101
Ósemkowo: 0o55
Szesnastkowo: 0x2d
```

---

# 21. Konwersja bez używania gotowych funkcji

Na zadaniach algorytmicznych warto znać sposób działania konwersji.

Przy zamianie liczby dziesiętnej na inny system wykorzystujemy:

- dzielenie całkowite `//`,
- resztę z dzielenia `%`.

## Przykład

Zamieniamy `45` na system binarny.

```text
45 : 2 = 22 reszta 1
22 : 2 = 11 reszta 0
11 : 2 = 5  reszta 1
5  : 2 = 2  reszta 1
2  : 2 = 1  reszta 0
1  : 2 = 0  reszta 1
```

Reszty:

```text
1 0 1 1 0 1
```

Czytamy je od dołu:

```text
101101
```

Zatem:

```text
45₁₀ = 101101₂
```

---

# 22. Własna funkcja konwersji

Możemy stworzyć funkcję, która konwertuje liczbę dziesiętną na system o podanej podstawie.

```python
def konwersja(liczba, podstawa):
    cyfry = "0123456789ABCDEF"
    wynik = ""

    if liczba == 0:
        return "0"

    while liczba > 0:
        reszta = liczba % podstawa
        wynik = cyfry[reszta] + wynik
        liczba = liczba // podstawa

    return wynik
```

### Test programu

```python
print(konwersja(45, 2))
print(konwersja(45, 8))
print(konwersja(45, 16))
```

Wynik:

```text
101101
55
2D
```

---

# 23. Przykłady zadań

## Zadanie 1

Zamień:

```text
110101₂
```

na system dziesiętny.

### Rozwiązanie

```text
1 · 2⁵ + 1 · 2⁴ + 0 · 2³ + 1 · 2² + 0 · 2¹ + 1 · 2⁰
```

```text
32 + 16 + 0 + 4 + 0 + 1 = 53
```

Odpowiedź:

```text
53
```

Python:

```python
print(int("110101", 2))
```

---

## Zadanie 2

Zamień:

```text
73₈
```

na system dziesiętny.

```python
print(int("73", 8))
```

Wynik:

```text
59
```

---

## Zadanie 3

Zamień:

```text
3A₁₆
```

na system dziesiętny.

`A = 10`

```text
3 · 16 + 10 = 58
```

Python:

```python
print(int("3A", 16))
```

Wynik:

```text
58
```

---

## Zadanie 4

Zamień `100` z systemu dziesiętnego na:

- binarny,
- ósemkowy,
- szesnastkowy.

```python
liczba = 100

print(bin(liczba))
print(oct(liczba))
print(hex(liczba))
```

Wynik:

```text
0b1100100
0o144
0x64
```

Czyli:

```text
100₁₀ = 1100100₂ = 144₈ = 64₁₆
```

---

# 24. Ćwiczenia dla ucznia

## Ćwiczenie 1

Napisz program, który zamieni liczbę:

```text
75₁₀
```

na:

1. system binarny,
2. system ósemkowy,
3. system szesnastkowy.

---

## Ćwiczenie 2

Zamień na system dziesiętny:

```text
101010₂
```

---

## Ćwiczenie 3

Zamień na system dziesiętny:

```text
127₈
```

---

## Ćwiczenie 4

Zamień na system dziesiętny:

```text
FF₁₆
```

---

## Ćwiczenie 5

Zamień:

```text
100000₁₀
```

na system binarny.

---

## Ćwiczenie 6

Zamień:

```text
100₁₀
```

na system ósemkowy.

---

## Ćwiczenie 7

Zamień:

```text
100₁₀
```

na system szesnastkowy.

---

## Ćwiczenie 8

Zamień:

```text
11110000₂
```

na system szesnastkowy.

---

## Ćwiczenie 9

Zamień:

```text
10110110₂
```

na system ósemkowy.

---

## Ćwiczenie 10

Napisz program, który pobiera od użytkownika liczbę binarną i wyświetla jej wartość dziesiętną.

---

## Ćwiczenie 11

Napisz program, który pobiera liczbę dziesiętną i wyświetla ją w:

- systemie binarnym,
- systemie ósemkowym,
- systemie szesnastkowym.

---

## Ćwiczenie 12

Napisz program, który pobiera:

```text
liczbę
podstawę systemu
```

i konwertuje liczbę do systemu dziesiętnego.

Program powinien obsługiwać podstawy:

```text
2
8
16
```

---

## Ćwiczenie 13 – poziom trudniejszy

Napisz funkcję:

```python
def zamien(liczba, podstawa):
```

która zamieni liczbę dziesiętną na system o podstawie:

```text
2
8
16
```

Nie używaj funkcji:

```python
bin()
oct()
hex()
```

---

## Ćwiczenie 14 – analiza kodu

Przeanalizuj program:

```python
x = 0b101010
y = 0x10

print(x)
print(y)
print(x + y)
```

Podaj wynik działania programu.

---

## Ćwiczenie 15 – zadanie maturalne

Napisz program, który dla liczby:

```text
156₁₀
```

wyświetli jej reprezentację:

- binarną,
- ósemkową,
- szesnastkową.

---

# 25. Klucz odpowiedzi

## Ćwiczenie 1

```text
75₁₀ = 1001011₂
75₁₀ = 113₈
75₁₀ = 4B₁₆
```

---

## Ćwiczenie 2

```text
101010₂ = 42₁₀
```

Python:

```python
int("101010", 2)
```

---

## Ćwiczenie 3

```text
127₈ = 87₁₀
```

Python:

```python
int("127", 8)
```

---

## Ćwiczenie 4

```text
FF₁₆ = 255₁₀
```

Python:

```python
int("FF", 16)
```

---

## Ćwiczenie 5

```text
100000₁₀ = 11000011010100000₂
```

---

## Ćwiczenie 6

```text
100₁₀ = 144₈
```

---

## Ćwiczenie 7

```text
100₁₀ = 64₁₆
```

---

## Ćwiczenie 8

```text
11110000₂ = F0₁₆
```

---

## Ćwiczenie 9

```text
10110110₂ = 266₈
```

---

## Ćwiczenie 14

```text
x = 42
y = 16
x + y = 58
```

Wynik programu:

```text
42
16
58
```

---

## Ćwiczenie 15

```text
156₁₀ = 10011100₂
156₁₀ = 234₈
156₁₀ = 9C₁₆
```

---

# 26. Ściąga – najważniejsze informacje

## System binarny

```text
Podstawa: 2
Cyfry: 0–1
Prefiks: 0b
Funkcja: bin()
```

## System ósemkowy

```text
Podstawa: 8
Cyfry: 0–7
Prefiks: 0o
Funkcja: oct()
```

## System szesnastkowy

```text
Podstawa: 16
Cyfry: 0–9, A–F
Prefiks: 0x
Funkcja: hex()
```

## Konwersja do systemu dziesiętnego

```python
int("101101", 2)
int("55", 8)
int("2D", 16)
```

## Konwersja z systemu dziesiętnego

```python
bin(45)
oct(45)
hex(45)
```

---

# 27. Najważniejsze do zapamiętania

```text
2  → bin()
8  → oct()
16 → hex()
```

W drugą stronę:

```text
binarny      → int(x, 2)
ósemkowy     → int(x, 8)
szesnastkowy → int(x, 16)
```

### Przykład

```python
liczba = 45

print(bin(liczba))       # 0b101101
print(oct(liczba))       # 0o55
print(hex(liczba))       # 0x2d

print(int("101101", 2))  # 45
print(int("55", 8))      # 45
print(int("2D", 16))     # 45
```

---

# 28. Podsumowanie

Systemy liczbowe są ważnym elementem informatyki.

Do najważniejszych systemów należą:

| System | Podstawa |
|---|---:|
| Dziesiętny | 10 |
| Binarny | 2 |
| Ósemkowy | 8 |
| Szesnastkowy | 16 |

W Pythonie najważniejsze funkcje związane z systemami liczbowymi to:

```python
bin()
oct()
hex()
int()
```

Warto również znać operatory:

```python
%
```

czyli resztę z dzielenia oraz:

```python
//
```

czyli dzielenie całkowite.

Znajomość systemów liczbowych pozwala lepiej rozumieć sposób reprezentowania danych przez komputer oraz rozwiązywać zadania algorytmiczne i maturalne z informatyki.
