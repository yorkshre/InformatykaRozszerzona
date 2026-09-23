# Lekcja: Analiza błędów w obliczeniach komputerowych

## 1. Temat lekcji

**Analiza błędów w obliczeniach komputerowych – liczby zmiennoprzecinkowe, błędy reprezentacji, zaokrągleń i obcięcia**

---



# 2. Wprowadzenie

Komputery wykonują obliczenia z ogromną szybkością. Nie oznacza to jednak, że każda liczba i każdy wynik są przechowywane z matematyczną idealną dokładnością.

Komputer ma ograniczoną pamięć i wykorzystuje skończoną liczbę bitów do przechowywania wartości.

Problem jest szczególnie widoczny w przypadku **liczb zmiennoprzecinkowych**.

Przykładowo:

```python
print(0.1 + 0.2)
```

może dać:

```text
0.30000000000000004
```

Matematycznie:

```text
0.1 + 0.2 = 0.3
```

Dlaczego komputer otrzymuje inną wartość?

Odpowiedź związana jest ze sposobem reprezentowania liczb w pamięci komputera.

---

# 3. Jak komputer przechowuje liczby?

Komputery wykorzystują system binarny, czyli system o podstawie 2.

W systemie dziesiętnym mamy cyfry:

```text
0 1 2 3 4 5 6 7 8 9
```

W systemie binarnym mamy tylko:

```text
0 1
```

Liczby całkowite można stosunkowo łatwo reprezentować binarnie.

Większy problem pojawia się dla części ułamkowych.

Przykład:

```text
0.5
```

można zapisać dokładnie w systemie binarnym:

```text
0.1₂
```

Podobnie:

```text
0.25 = 0.01₂
```

Natomiast:

```text
0.1
```

ma w systemie binarnym nieskończone rozwinięcie:

```text
0.00011001100110011...₂
```

Komputer nie może przechowywać nieskończonej liczby cyfr. Musi więc zapisać przybliżenie.

To prowadzi do **błędu reprezentacji**.

---

# 4. Błąd reprezentacji

## Definicja

**Błąd reprezentacji** (*representation error*) powstaje dlatego, że dana liczba nie może być dokładnie przedstawiona w używanym przez komputer formacie.

Przykładem jest:

```python
x = 0.1
```

Możemy sprawdzić reprezentację za pomocą:

```python
x = 0.1

print(x)
print(format(x, ".20f"))
```

Możemy otrzymać wynik zbliżony do:

```text
0.1
0.10000000000000000555
```

Oznacza to, że wewnętrzna reprezentacja liczby jest bardzo bliska `0.1`, ale nie jest z nią matematycznie identyczna.

---

# 5. Standard IEEE 754

Liczby zmiennoprzecinkowe są zazwyczaj przechowywane zgodnie ze standardem **IEEE 754**.

W typowym przypadku liczby pojedynczej lub podwójnej precyzji liczba jest reprezentowana za pomocą kilku elementów.

Dla typowego `float` podwójnej precyzji wykorzystywane są:

- 1 bit znaku,
- 11 bitów wykładnika,
- 52 bity części ułamkowej znaczącej (mantysy, przy dodatkowym ukrytym bicie dla liczb znormalizowanych).

Łącznie:

```text
64 bity
```

W Pythonie typ `float` jest w typowej implementacji CPython zgodny z formatem podwójnej precyzji IEEE 754.

---

# 6. Błąd zaokrąglenia

## Definicja

**Błąd zaokrąglenia** (*rounding error*) powstaje, gdy wynik działania wymaga większej precyzji, niż komputer może przechować.

Załóżmy, że komputer dysponuje ograniczoną liczbą cyfr znaczących.

Jeżeli wynik wygląda tak:

```text
3.14159265358979323846
```

a dostępna precyzja pozwala zachować tylko część cyfr, wynik zostanie zaokrąglony.

Przykład:

```python
x = 1 / 3

print(x)
```

Wartość matematyczna:

```text
0.333333333333333...
```

nie ma skończonego rozwinięcia dziesiętnego.

Komputer zapisuje więc jej przybliżenie.

---

# 7. Błąd obcięcia

## Definicja

**Błąd obcięcia** (*truncation error*) powstaje wtedy, gdy nieskończony proces matematyczny zastępujemy procesem skończonym.

Przykładem jest szereg Taylora dla funkcji sinus:

\[
\sin(x)=x-\frac{x^3}{3!}+\frac{x^5}{5!}-\frac{x^7}{7!}+\dots
\]

W programie nie wykonujemy nieskończonej liczby działań.

Możemy wykorzystać np. trzy składniki:

\[
\sin(x)\approx x-\frac{x^3}{3!}+\frac{x^5}{5!}
\]

Pominięte składniki powodują błąd obcięcia.

---

# 8. Przykład błędu obcięcia w Pythonie

```python
import math

x = 0.5

przyblizenie = x - x**3 / 6 + x**5 / 120

print("Przybliżenie:", przyblizenie)
print("Wartość dokładniejsza:", math.sin(x))

blad = abs(math.sin(x) - przyblizenie)

print("Błąd bezwzględny:", blad)
```

Możemy zwiększyć dokładność, dodając kolejne składniki szeregu.

---

# 9. Porównanie rodzajów błędów

| Rodzaj błędu | Przyczyna | Przykład |
|---|---|---|
| Błąd reprezentacji | Liczba nie może być dokładnie zapisana w danym formacie | `0.1` |
| Błąd zaokrąglenia | Wynik ma większą precyzję niż dostępny format | `1 / 3` |
| Błąd obcięcia | Zakończenie nieskończonego procesu | przybliżenie `sin(x)` |
| Błąd bezwzględny | Różnica wartości dokładnej i przybliżonej | `abs(x - xp)` |
| Błąd względny | Błąd odniesiony do wartości dokładnej | `abs(x-xp)/abs(x)` |

---

# 10. Błąd bezwzględny

Błąd bezwzględny informuje, o ile wynik przybliżony różni się od wartości dokładnej.

Oznaczenia:

- `x` – wartość dokładna,
- `xp` – wartość przybliżona.

Wzór:

\[
\Delta x=|x-x_p|
\]

## Przykład

Wartość dokładna:

```text
x = 10
```

Wartość przybliżona:

```text
xp = 9.8
```

Obliczenie:

\[
\Delta x=|10-9.8|=0.2
\]

Zatem:

```text
Błąd bezwzględny = 0.2
```

---

# 11. Błąd względny

Błąd względny uwzględnia skalę wartości.

Wzór:

\[
\delta x=\frac{|x-x_p|}{|x|}
\]

Błąd względny w procentach:

\[
\delta x=\frac{|x-x_p|}{|x|}\cdot100\%
\]

## Przykład

Dane:

```text
x = 10
xp = 9.8
```

Błąd bezwzględny:

\[
|10-9.8|=0.2
\]

Błąd względny:

\[
\frac{0.2}{10}=0.02
\]

Błąd względny procentowy:

\[
0.02\cdot100\%=2\%
\]

Otrzymujemy:

```text
Błąd bezwzględny = 0.2
Błąd względny = 0.02
Błąd względny = 2%
```

---

# 12. Ćwiczenie – obliczanie błędów

Dane:

```text
Wartość dokładna:     25
Wartość przybliżona:  24.7
```

Oblicz:

1. błąd bezwzględny,
2. błąd względny,
3. błąd względny w procentach.

### Rozwiązanie

\[
\Delta x=|25-24.7|=0.3
\]

\[
\delta x=\frac{0.3}{25}=0.012
\]

\[
0.012\cdot100\%=1.2\%
\]

Odpowiedź:

```text
Błąd bezwzględny = 0.3
Błąd względny = 0.012
Błąd względny = 1.2%
```

---

# 13. Problem z `float` w Pythonie

Sprawdź program:

```python
a = 0.1
b = 0.2

print(a + b)
```

Możemy otrzymać:

```text
0.30000000000000004
```

Teraz sprawdź:

```python
print(0.1 + 0.2 == 0.3)
```

Wynik może być:

```text
False
```

Nie oznacza to błędu języka Python.

Jest to konsekwencja reprezentacji liczb zmiennoprzecinkowych.

---

# 14. Dlaczego `==` może być problemem?

Porównanie:

```python
a == b
```

sprawdza, czy wartości są identyczne.

W przypadku liczb zmiennoprzecinkowych bardzo mała różnica może powodować wynik:

```text
False
```

mimo że z punktu widzenia naszego problemu obie liczby powinny być traktowane jako równe.

Dlatego często stosujemy tolerancję.

---

# 15. Tolerancja EPS

`EPS` oznacza przyjętą tolerancję błędu.

Przykład:

```python
EPS = 1e-9
```

czyli:

\[
EPS=0.000000001
\]

Zamiast:

```python
if a == b:
```

stosujemy:

```python
if abs(a - b) < EPS:
    print("Liczby są wystarczająco bliskie")
```

## Przykład

```python
a = 0.1 + 0.2
b = 0.3

EPS = 1e-9

if abs(a - b) < EPS:
    print("Liczby są wystarczająco bliskie")
else:
    print("Liczby różnią się zbyt mocno")
```

---

# 16. Jak działa `abs()`?

Funkcja:

```python
abs()
```

zwraca wartość bezwzględną.

Przykłady:

```python
print(abs(5))
print(abs(-5))
print(abs(2.5 - 3.0))
```

Wynik:

```text
5
5
0.5
```

W porównaniu liczb zmiennoprzecinkowych:

```python
abs(a - b)
```

oznacza odległość pomiędzy wartościami `a` i `b`.

---

# 17. Symulacja błędów numerycznych

Poniższy program wielokrotnie dodaje `0.1` do wartości `x`.

Celem jest osiągnięcie wartości `1.0` z tolerancją `EPS`.

```python
def symulacja_bledow_numerycznych():
    EPS = 0.01

    x = 0.0
    krok = 0.1
    cel = 1.0

    print("=== SYMULACJA BŁĘDÓW REPREZENTACJI ===")
    print(
        f"Dodawanie kroku {krok} do osiągnięcia "
        f"celu {cel} z dokładnością EPS = {EPS}\n"
    )

    print(
        f"{'Nr iteracji':<12} | "
        f"{'Wartość x':<25} | "
        f"{'Błąd bezwzględny':<20}"
    )

    print("-" * 65)

    iteracja = 0

    while abs(x - cel) > EPS:
        x += krok
        iteracja += 1

        blad_bezwzgledny = abs(cel - x)

        print(
            f"{iteracja:<12} | "
            f"{x:<25.10f} | "
            f"{blad_bezwzgledny:<20.10f}"
        )

    print("-" * 65)

    print("\n[Podsumowanie końcowe pętli]")
    print(f"Liczba wykonanych iteracji: {iteracja}")
    print(f"Ostateczna wartość x:       {x:.10f}")

    x_dokladne = 1.0
    x_przyblizone = x

    blad_bezwzgledny = abs(
        x_dokladne - x_przyblizone
    )

    blad_wzgledny = (
        blad_bezwzgledny /
        abs(x_dokladne)
    )

    blad_wzgledny_proc = (
        blad_wzgledny * 100
    )

    print(
        f"Błąd bezwzględny: "
        f"{blad_bezwzgledny:.10f}"
    )

    print(
        f"Błąd względny: "
        f"{blad_wzgledny:.10f} "
        f"({blad_wzgledny_proc:.4f}%)"
    )


if __name__ == "__main__":
    symulacja_bledow_numerycznych()
```

---

# 18. Analiza programu krok po kroku

## Krok 1 – ustalenie tolerancji

```python
EPS = 0.01
```

Program będzie uznawał wartość za wystarczająco bliską celu, jeśli różnica będzie nie większa niż przyjęta tolerancja.

---

## Krok 2 – wartości początkowe

```python
x = 0.0
krok = 0.1
cel = 1.0
```

Oznaczają:

- `x` – aktualną wartość,
- `krok` – wartość dodawaną w każdej iteracji,
- `cel` – wartość, którą chcemy osiągnąć.

---

## Krok 3 – warunek pętli

```python
while abs(x - cel) > EPS:
```

Pętla działa tak długo, jak długo:

\[
|x-cel|>EPS
\]

---

## Krok 4 – zwiększenie wartości

```python
x += krok
```

jest skróconym zapisem:

```python
x = x + krok
```

---

## Krok 5 – obliczenie błędu

```python
blad_bezwzgledny = abs(cel - x)
```

Obliczamy odległość aktualnej wartości od wartości docelowej.

---

# 19. Co by się stało bez EPS?

Możemy napisać:

```python
x = 0.0

while x != 1.0:
    x += 0.1
```

Jest to niebezpieczne.

Jeżeli komputer otrzyma:

```text
0.9999999999999999
```

to warunek:

```python
x != 1.0
```

będzie nadal prawdziwy.

Po kolejnym dodaniu `0.1` możemy przekroczyć wartość `1.0`.

Program może wtedy nigdy nie uzyskać dokładnie:

```text
1.0
```

Dlatego w algorytmach numerycznych stosuje się tolerancję.

---

# 20. Kumulacja błędów

Błędy zaokrągleń mogą się kumulować.

Przykład:

```python
x = 0.0

for i in range(1000000):
    x += 0.1

print(x)
```

Matematycznie oczekujemy:

```text
100000
```

Jednak wynik obliczeń zmiennoprzecinkowych może nie być dokładnie taki.

Każde pojedyncze przybliżenie jest bardzo małe, ale milion operacji może spowodować zauważalną różnicę.

---

# 21. Przykład kumulacji błędu

```python
x = 0.0

for i in range(10):
    x += 0.1

print("Wynik:", x)
print("Błąd:", abs(1.0 - x))
```

Program pozwala zobaczyć różnicę pomiędzy wartością oczekiwaną i obliczoną.

---

# 22. Jak ograniczać błędy?

## Zasada 1 – nie używaj bezpośrednio `==` dla `float`

Zamiast:

```python
if a == b:
```

często stosuj:

```python
if abs(a - b) < EPS:
```

---

## Zasada 2 – dobierz odpowiednie EPS

Przykładowe wartości:

```python
EPS = 0.01
```

```python
EPS = 0.000001
```

```python
EPS = 1e-9
```

```python
EPS = 1e-12
```

Im mniejsze `EPS`, tym większej dokładności wymagamy.

Nie ma jednak jednej uniwersalnej wartości EPS dla wszystkich problemów.

---

# 23. Typ `float`

Python wykorzystuje typ:

```python
float
```

do przechowywania liczb zmiennoprzecinkowych.

Przykład:

```python
x = 3.14

print(type(x))
```

Wynik:

```text
<class 'float'>
```

W typowej implementacji CPython `float` odpowiada liczbie zmiennoprzecinkowej podwójnej precyzji IEEE 754.

Duża precyzja nie oznacza jednak, że każda liczba rzeczywista może być reprezentowana dokładnie.

---

# 24. `Decimal` – alternatywa dla `float`

W sytuacjach wymagających kontrolowanej arytmetyki dziesiętnej można użyć modułu:

```python
decimal
```

Przykład:

```python
from decimal import Decimal

a = Decimal("0.1")
b = Decimal("0.2")

print(a + b)
```

Wynik:

```text
0.3
```

Wartości `Decimal` najlepiej tworzyć z napisów:

```python
Decimal("0.1")
```

zamiast:

```python
Decimal(0.1)
```

---

# 25. Kiedy `Decimal` może być przydatny?

`Decimal` może być szczególnie użyteczny w zastosowaniach, w których istotna jest dokładność dziesiętna, np.:

- obliczenia finansowe,
- ceny,
- podatki,
- rozliczenia,
- wartości pieniężne.

Nie oznacza to jednak, że `Decimal` jest zawsze lepszy od `float`. Wybór typu zależy od zastosowania.

---

# 26. Najważniejsze wzory

## Błąd bezwzględny

\[
\boxed{\Delta x=|x-x_p|}
\]

## Błąd względny

\[
\boxed{
\delta x=
\frac{|x-x_p|}{|x|}
}
\]

## Błąd względny w procentach

\[
\boxed{
\delta x=
\frac{|x-x_p|}{|x|}
\cdot100\%
}
\]

---

# 27. Ćwiczenia dla ucznia

## Ćwiczenie 1 – liczba `0.1`

Uruchom:

```python
x = 0.1

print(x)
print(format(x, ".20f"))
```

### Odpowiedz:

1. Co pokazuje drugi wynik?
2. Dlaczego `0.1` nie jest przechowywane idealnie?
3. Czy każda liczba ułamkowa ma problem z reprezentacją binarną?

---

## Ćwiczenie 2 – `0.1 + 0.2`

Uruchom:

```python
a = 0.1
b = 0.2
c = 0.3

print(a + b)
print(a + b == c)
```

### Odpowiedz:

1. Jaki otrzymałeś wynik?
2. Dlaczego porównanie może zwrócić `False`?
3. Jak można poprawić program?

---

## Ćwiczenie 3 – EPS

Napisz program:

```python
a = 0.1 + 0.2
b = 0.3

EPS = 1e-9

if abs(a - b) < EPS:
    print("Liczby są równe z przyjętą dokładnością")
else:
    print("Liczby są różne")
```

Następnie przetestuj:

```python
EPS = 0.1
EPS = 0.01
EPS = 0.000001
EPS = 1e-9
EPS = 1e-12
```

---

## Ćwiczenie 4 – błąd bezwzględny i względny

Napisz program dla:

```python
x = 100
xp = 98.5
```

Program powinien wyświetlić:

- błąd bezwzględny,
- błąd względny,
- błąd względny w procentach.

---

## Ćwiczenie 5 – szereg Taylora

Napisz program obliczający:

\[
\sin(x)\approx
x-\frac{x^3}{3!}+\frac{x^5}{5!}
\]

Następnie porównaj wynik z:

```python
math.sin(x)
```

Oblicz błąd bezwzględny.

---

## Ćwiczenie 6 – eksperyment z dokładnością

Utwórz program:

```python
x = 0.0

for i in range(10):
    x += 0.1

print(x)
```

Następnie zmień liczbę iteracji na:

```text
10
100
1000
10000
100000
```

Sprawdź, jak zmienia się wynik.

---

## Ćwiczenie 7 – porównanie `float` i `Decimal`

Porównaj:

```python
a = 0.1
b = 0.2

print(a + b)
```

z:

```python
from decimal import Decimal

a = Decimal("0.1")
b = Decimal("0.2")

print(a + b)
```

Wyjaśnij różnicę.

---

# 29. Zadanie problemowe

Napisz program, który:

1. pobierze od użytkownika wartość dokładną,
2. pobierze wartość przybliżoną,
3. obliczy błąd bezwzględny,
4. obliczy błąd względny,
5. obliczy błąd względny w procentach,
6. wyświetli wszystkie wyniki.

Przykładowy program:

```python
x = float(input("Podaj wartość dokładną: "))
xp = float(input("Podaj wartość przybliżoną: "))

blad_bezwzgledny = abs(x - xp)
blad_wzgledny = blad_bezwzgledny / abs(x)
blad_proc = blad_wzgledny * 100

print("Błąd bezwzględny:", blad_bezwzgledny)
print("Błąd względny:", blad_wzgledny)
print("Błąd względny (%):", blad_proc, "%")
```

Uwaga: dla `x = 0` wzór na błąd względny nie może być bezpośrednio zastosowany, ponieważ prowadziłby do dzielenia przez zero.

---

# 30. Zadanie – analiza gotowego programu

Przeanalizuj:

```python
EPS = 0.001

x = 0.0
krok = 0.1
cel = 1.0

while abs(x - cel) > EPS:
    x += krok

print(x)
```

### Odpowiedz:

1. Co oznacza `EPS`?
2. Co oznacza `krok`?
3. Co oznacza `cel`?
4. Co sprawdza warunek `while`?
5. Dlaczego zastosowano `abs()`?
6. Dlaczego nie zastosowano `x != cel`?
7. Ile razy wykonywana jest pętla?
8. Czy wynik musi być dokładnie równy `1.0`?

---

# 31. Mini-test

### 1. Dlaczego `0.1` może być niedokładnie przechowywane?

A. Ponieważ Python nie obsługuje liczb ułamkowych  
B. Ponieważ komputer korzysta z reprezentacji binarnej  
C. Ponieważ `0.1` jest liczbą całkowitą  
D. Ponieważ pamięć komputera jest nieskończona  

**Prawidłowa odpowiedź: B**

---

### 2. Który zapis oblicza błąd bezwzględny?

A.

```python
x + xp
```

B.

```python
x * xp
```

C.

```python
abs(x - xp)
```

D.

```python
x / xp
```

**Prawidłowa odpowiedź: C**

---

### 3. Co oznacza `EPS`?

A. Rodzaj procesora  
B. Tolerancję błędu  
C. Liczbę iteracji  
D. Typ danych  

**Prawidłowa odpowiedź: B**

---

### 4. Które porównanie jest bezpieczniejsze dla liczb zmiennoprzecinkowych?

A.

```python
a == b
```

B.

```python
abs(a - b) < EPS
```

C.

```python
a != b
```

D.

```python
a > b
```

**Prawidłowa odpowiedź: B**

---

### 5. Czym jest błąd obcięcia?

A. Błędem wynikającym z wyłączenia komputera  
B. Błędem wynikającym z przerwania nieskończonego procesu matematycznego  
C. Błędem składniowym  
D. Błędem wynikającym z braku pamięci RAM  

**Prawidłowa odpowiedź: B**

---

# 32. Zadanie podsumowujące

Napisz program wykonujący eksperyment:

1. Ustaw:

```python
x = 0.0
```

2. Wykonaj 100 dodawań:

```python
x += 0.1
```

3. Oblicz wartość oczekiwaną:

```python
100 * 0.1
```

4. Oblicz błąd bezwzględny.
5. Oblicz błąd względny.
6. Wyświetl wynik.
7. Powtórz eksperyment dla:
   - 10 iteracji,
   - 100 iteracji,
   - 1000 iteracji,
   - 10000 iteracji,
   - 100000 iteracji.

### Pytanie

Czy wraz ze wzrostem liczby operacji błąd zawsze rośnie?

**Uzasadnij odpowiedź na podstawie wyników eksperymentu.**

---

# 33. Najważniejsze informacje do zapamiętania

> **Komputer nie wykonuje wszystkich obliczeń na idealnych liczbach rzeczywistych.**

Najważniejsze fakty:

1. Komputer przechowuje dane za pomocą skończonej liczby bitów.
2. Liczby są reprezentowane w systemie binarnym.
3. Nie wszystkie liczby dziesiętne mają skończoną reprezentację binarną.
4. `0.1` jest przykładem liczby, której nie można dokładnie przedstawić w typowym formacie `float`.
5. Błąd reprezentacji wynika ze sposobu zapisu liczby.
6. Błąd zaokrąglenia wynika z ograniczonej precyzji.
7. Błąd obcięcia wynika z zastąpienia procesu nieskończonego procesem skończonym.
8. Błąd bezwzględny mierzy bezpośrednią różnicę.
9. Błąd względny uwzględnia skalę wartości.
10. Nie należy bezkrytycznie porównywać liczb `float` za pomocą `==`.
11. Do porównań można stosować tolerancję `EPS`.
12. Błędy mogą się kumulować podczas wielu operacji.
13. Większa precyzja zmniejsza problem, ale nie eliminuje go całkowicie.
14. W określonych zastosowaniach można wykorzystać `Decimal`.

---

# 34. Dobre praktyki programistyczne

### Unikaj:

```python
if a == b:
```

gdy `a` i `b` są wynikami obliczeń zmiennoprzecinkowych.

### Stosuj:

```python
EPS = 1e-9

if abs(a - b) < EPS:
    ...
```

### Pamiętaj:

```text
float ≠ idealna liczba rzeczywista
```

oraz:

```text
większa liczba operacji → możliwość kumulacji błędów
```

---

# 35. Podsumowanie lekcji

Błędy numeryczne są naturalną konsekwencją sposobu, w jaki komputery przechowują i przetwarzają liczby.

Najważniejsze rodzaje błędów to:

- **błąd reprezentacji** – związany ze sposobem zapisu liczby,
- **błąd zaokrąglenia** – związany z ograniczoną precyzją,
- **błąd obcięcia** – związany z zakończeniem nieskończonego procesu,
- **błąd bezwzględny** – określający bezpośrednią różnicę,
- **błąd względny** – określający błąd w stosunku do wartości dokładnej.

W programowaniu szczególnie ważne jest poprawne porównywanie liczb zmiennoprzecinkowych.

Zamiast:

```python
a == b
```

często należy zastosować:

```python
abs(a - b) < EPS
```

Dzięki temu program może uwzględniać niewielkie różnice wynikające z reprezentacji i zaokrągleń.

---

# 36. Zadanie domowe

Napisz program, który przeprowadzi eksperyment z liczbą `0.1`.

Program powinien:

1. wykonać `10`, `100`, `1000`, `10000` i `100000` dodawań,
2. dla każdej liczby iteracji obliczyć wynik,
3. obliczyć wartość oczekiwaną,
4. obliczyć błąd bezwzględny,
5. obliczyć błąd względny,
6. wyświetlić wyniki w formie tabeli.

### Przykładowy format tabeli

```text
Iteracje | Wynik | Wartość oczekiwana | Błąd bezwzględny | Błąd względny
---------|-------|--------------------|------------------|--------------
10       | ...   | 1.0                | ...              | ...
100      | ...   | 10.0               | ...              | ...
1000     | ...   | 100.0              | ...              | ...
10000    | ...   | 1000.0             | ...              | ...
100000   | ...   | 10000.0            | ...              | ...
```

### Pytanie końcowe

Na podstawie eksperymentu odpowiedz:

> **Czy zwiększanie liczby operacji może wpływać na dokładność obliczeń zmiennoprzecinkowych? Wyjaśnij dlaczego.**

---

# 37. Słowniczek pojęć

| Pojęcie | Znaczenie |
|---|---|
| `float` | Typ danych służący do przechowywania liczb zmiennoprzecinkowych |
| IEEE 754 | Standard reprezentacji liczb zmiennoprzecinkowych |
| Błąd reprezentacji | Błąd wynikający ze sposobu zapisu liczby |
| Błąd zaokrąglenia | Błąd wynikający z ograniczonej precyzji |
| Błąd obcięcia | Błąd wynikający z zakończenia nieskończonego procesu |
| Błąd bezwzględny | `abs(x - xp)` |
| Błąd względny | `abs(x-xp)/abs(x)` |
| EPS | Przyjęta tolerancja porównania |
| Mantysa | Część reprezentacji liczby określająca jej znaczące cyfry |
| Kumulacja błędu | Narastanie błędów podczas kolejnych operacji |
| `Decimal` | Typ zapewniający kontrolowaną arytmetykę dziesiętną |

---

# 38. Schemat do zapamiętania

```text
LICZBA MATEMATYCZNA
        ↓
REPREZENTACJA BINARNA
        ↓
OGRANICZONA LICZBA BITÓW
        ↓
PRZYBLIŻENIE
        ↓
BŁĘDY NUMERYCZNE
        ↓
┌──────────────────────┐
│ reprezentacji         │
│ zaokrąglenia          │
│ obcięcia              │
└──────────────────────┘
        ↓
ANALIZA BŁĘDU
        ↓
┌──────────────────────┐
│ błąd bezwzględny     │
│ błąd względny        │
└──────────────────────┘
        ↓
BEZPIECZNE PORÓWNANIA
        ↓
abs(a - b) < EPS
```

---

# 39. Konkluzja

Poprawne wykonywanie obliczeń komputerowych wymaga nie tylko znajomości składni języka programowania, ale również zrozumienia ograniczeń reprezentacji liczb.

Programista powinien pamiętać, że komputer pracuje na przybliżeniach. Szczególnie istotne jest to w programach matematycznych, naukowych, finansowych oraz wszędzie tam, gdzie wymagana jest wysoka dokładność.

**Najważniejsza zasada lekcji:**

```python
# Nie zakładaj idealnej dokładności float.
# Uwzględniaj tolerancję błędu.

if abs(a - b) < EPS:
    print("Wartości są wystarczająco bliskie")
```

