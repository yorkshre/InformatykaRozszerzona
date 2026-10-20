# Lekcja: Dodawanie liczb w różnych systemach pozycyjnych – Python

Materiał opracowany na podstawie przesłanych stron z podręcznika. Obejmuje dodawanie liczb binarnych, dodawanie w dowolnym systemie pozycyjnym oraz implementację algorytmów w języku Python.

## 1. Cele lekcji

Po zakończeniu lekcji uczeń:

- wyjaśnia, jak działa dodawanie pisemne w systemie dwójkowym;
- wykonuje działania arytmetyczne w systemach o różnych podstawach;
- rozumie pojęcie przeniesienia do kolejnej pozycji;
- zapisuje algorytm dodawania liczb w postaci programu w Pythonie;
- potrafi rozszerzyć program tak, aby obsługiwał różne systemy pozycyjne.

## 2. System pozycyjny – przypomnienie

W systemie pozycyjnym wartość cyfry zależy od jej położenia w liczbie. Podstawa systemu określa, ile różnych cyfr można wykorzystać.

| System | Podstawa | Dozwolone cyfry |
|---|---:|---|
| Dwójkowy (binarny) | 2 | 0, 1 |
| Ósemkowy | 8 | 0–7 |
| Dziesiętny | 10 | 0–9 |
| Szesnastkowy | 16 | 0–9, A–F |

Przykładowo liczba `1011₂` oznacza:

`1 · 2³ + 0 · 2² + 1 · 2¹ + 1 · 2⁰ = 11₁₀`

Indeks dolny wskazuje podstawę systemu, w którym zapisana jest liczba.

## 3. Dodawanie liczb binarnych

W systemie dwójkowym występują tylko dwie cyfry: `0` i `1`.

| Działanie | Wynik | Przeniesienie |
|---|---:|---:|
| 0 + 0 | 0 | 0 |
| 0 + 1 | 1 | 0 |
| 1 + 0 | 1 | 0 |
| 1 + 1 | 0 | 1 |
| 1 + 1 + 1 | 1 | 1 |

Najważniejsza zasada: jeśli suma cyfr wynosi co najmniej 2, zapisujemy resztę z dzielenia przez 2, a iloraz całkowity staje się przeniesieniem.

Przykład:

```text
   1011
 + 0110
 -------
  10001
```

Sprawdzenie w systemie dziesiętnym:

- `1011₂ = 11₁₀`
- `0110₂ = 6₁₀`
- `11 + 6 = 17`
- `10001₂ = 17₁₀`

Wynik jest poprawny.

## 4. Algorytm dodawania liczb binarnych

Algorytm dodaje dwie liczby zapisane jako napisy złożone z zer i jedynek. Nie trzeba zamieniać ich na liczby dziesiętne.

1. Wczytaj dwie liczby binarne.
2. Wyrównaj ich długości, dopisując z lewej strony zera.
3. Rozpocznij dodawanie od ostatniej cyfry.
4. Oblicz sumę dwóch cyfr i przeniesienia.
5. Zapisz cyfrę wyniku jako resztę z dzielenia sumy przez 2.
6. Wyznacz nowe przeniesienie przez dzielenie całkowite sumy przez 2.
7. Po przejściu przez wszystkie cyfry uwzględnij ewentualne końcowe przeniesienie.

### Przykład w Pythonie – dodawanie liczb binarnych

```python
a = input("Podaj pierwszą liczbę binarną: ")
b = input("Podaj drugą liczbę binarną: ")

# Sprawdzenie poprawności danych
if not a or not b or any(c not in "01" for c in a + b):
    print("Błąd: podaj liczby zawierające tylko 0 i 1.")
else:
    # Wyrównanie długości liczb
    dlugosc = max(len(a), len(b))
    a = a.zfill(dlugosc)
    b = b.zfill(dlugosc)

    wynik = ""
    przeniesienie = 0

    # Dodawanie od prawej strony
    for i in range(dlugosc - 1, -1, -1):
        suma = int(a[i]) + int(b[i]) + przeniesienie

        cyfra = suma % 2
        przeniesienie = suma // 2

        wynik = str(cyfra) + wynik

    # Obsługa ostatniego przeniesienia
    if przeniesienie:
        wynik = "1" + wynik

    print("Suma:", wynik)
```

Przykładowe działanie:

```text
Podaj pierwszą liczbę binarną: 1011
Podaj drugą liczbę binarną: 110
Suma: 10001
```

### Wyjaśnienie instrukcji

| Instrukcja | Znaczenie |
|---|---|
| `zfill(dlugosc)` | Uzupełnia liczbę zerami z lewej strony. |
| `range(dlugosc - 1, -1, -1)` | Przechodzi po indeksach od prawej do lewej. |
| `int(a[i])` | Zamienia pojedynczy znak `0` lub `1` na liczbę całkowitą. |
| `suma % 2` | Wyznacza cyfrę wyniku w systemie binarnym. |
| `suma // 2` | Oblicza przeniesienie do następnej pozycji. |
| `wynik = str(cyfra) + wynik` | Dopisuje cyfrę na początku tworzonego wyniku. |

Program nie korzysta z gotowej funkcji konwersji liczby binarnej. Dodawanie jest wykonywane krok po kroku, tak jak na papierze.

## 5. Dodawanie w dowolnym systemie pozycyjnym

Zasady dodawania można zastosować nie tylko do systemu binarnego. W systemie o podstawie `p` cyfrę wyniku obliczamy ze wzoru:

`cyfra = suma % p`

Natomiast przeniesienie wynosi:

`przeniesienie = suma // p`

W Pythonie zapisujemy to odpowiednio jako:

```python
cyfra = suma % podstawa
przeniesienie = suma // podstawa
```

### Przykład dla systemu czwórkowego

`3₄ + 2₄ = 11₄`

Dlaczego? Ponieważ `3 + 2 = 5` w systemie dziesiętnym, a `5 = 11₄`.

W systemie czwórkowym używamy tylko cyfr `0`, `1`, `2`, `3`. Jeśli suma cyfr wynosi 4 lub więcej, pojawia się przeniesienie.

Przykład dodawania:

```text
   3213₄
 + 0212₄
 -------
  10031₄
```

Sprawdzenie:

- `3213₄ = 231₁₀`
- `0212₄ = 38₁₀`
- `231 + 38 = 269`
- `10031₄ = 269₁₀`

Wynik `10031₄` jest prawidłowy.

## 6. Python – program dodający liczby w różnych systemach

Poniższy program dodaje liczby zapisane w systemach o podstawach od 2 do 36. Wykorzystuje cyfry `0–9` oraz litery `A–Z`, gdzie `A` oznacza 10, `B` oznacza 11 itd.

```python
CYFRY = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"


def dodaj(a, b, podstawa):
    a = a.upper()
    b = b.upper()

    # Wyrównanie długości zapisów
    dlugosc = max(len(a), len(b))
    a = a.zfill(dlugosc)
    b = b.zfill(dlugosc)

    wynik = ""
    przeniesienie = 0

    # Dodawanie od prawej strony
    for i in range(dlugosc - 1, -1, -1):
        x = CYFRY.index(a[i])
        y = CYFRY.index(b[i])

        suma = x + y + przeniesienie

        cyfra = suma % podstawa
        przeniesienie = suma // podstawa

        wynik = CYFRY[cyfra] + wynik

    # Pozostałe przeniesienie
    while przeniesienie > 0:
        cyfra = przeniesienie % podstawa
        wynik = CYFRY[cyfra] + wynik
        przeniesienie //= podstawa

    return wynik.lstrip("0") or "0"


def poprawna_liczba(liczba, podstawa):
    return (
        bool(liczba)
        and all(
            znak in CYFRY[:podstawa]
            for znak in liczba.upper()
        )
    )


podstawa = int(input("Podaj podstawę systemu (2-36): "))

if not 2 <= podstawa <= 36:
    print("Błąd: podstawa musi być z zakresu 2-36.")
else:
    a = input("Podaj pierwszą liczbę: ")
    b = input("Podaj drugą liczbę: ")

    if (poprawna_liczba(a, podstawa)
            and poprawna_liczba(b, podstawa)):
        print("Suma:", dodaj(a, b, podstawa))
    else:
        print("Błąd: liczby zawierają niedozwolone cyfry.")
```

### Przykładowe wyniki

System dwójkowy (podstawa 2):

```text
Pierwsza liczba: 1011
Druga liczba:   110
Suma:           10001
```

System czwórkowy (podstawa 4):

```text
Pierwsza liczba: 3213
Druga liczba:   212
Suma:           10031
```

System szesnastkowy (podstawa 16):

```text
Pierwsza liczba: 2A
Druga liczba:   15
Suma:           3F
```

### Jak działa wersja ogólna?

1. Funkcja `dodaj()` odczytuje cyfry obu liczb.
2. `CYFRY.index()` zamienia każdą cyfrę na jej wartość liczbową.
3. Obliczana jest suma cyfr wraz z przeniesieniem.
4. Operator `%` wyznacza cyfrę wyniku.
5. Operator `//` oblicza przeniesienie.
6. Funkcja zwraca wynik zapisany w tym samym systemie.

**Ważne:** program wykonuje dodawanie bez zamiany całych liczb na system dziesiętny. Obsługuje również przeniesienie większe niż 1, co jest istotne w systemach o podstawach większych niż 2.

# Odejmowanie przyklad

```python
def PrzeciwnaU2(b):
    """Funkcja wyznaczająca liczbę przeciwną w kodzie U2:

    1. Zamienia wszystkie bity na przeciwne (0 na 1, 1 na 0).
    2. Dodaje 1 do wyniku (zgodnie z algorytmem bitowym).
    """
    # Krok 1: Negacja bitów
    negowane = "".join("1" if bit == "0" else "0" for bit in b)

    # Krok 2: Dodanie 1 (symulacja dodawania od prawej strony)
    lista_bitow = list(negowane)
    przeniesienie = 1

    for i in range(len(lista_bitow) - 1, -1, -1):
        if lista_bitow[i] == "1" and przeniesienie == 1:
            lista_bitow[i] = "0"
            przeniesienie = 1
        elif lista_bitow[i] == "0" and przeniesienie == 1:
            lista_bitow[i] = "1"
            przeniesienie = 0
            break
        else:
            break

    return "".join(lista_bitow)


def U2NaDziesietny(b):
    """Pomocnicza funkcja zamieniająca ciąg bitów U2 na wartość dziesiętną."""
    if b[0] == "1":
        # Liczba ujemna: najpierw znajdujemy liczbę przeciwną, konwertujemy na int i dajemy minus
        przeciwna = PrzeciwnaU2(b)
        return -int(przeciwna, 2)
    else:
        return int(b, 2)


# --- Rozwiązanie Ćwiczenia 4 ---
if __name__ == "__main__":
    # Dane z ćwiczenia
    wejscie = "11110100"

    wynik_przeciwna = PrzeciwnaU2(wejscie)
    wartosc_poczatkowa = U2NaDziesietny(wejscie)
    wartosc_przeciwna = U2NaDziesietny(wynik_przeciwna)

    print(f"Liczba początkowa U2: {wejscie} = {wartosc_poczatkowa}")
    print(f"Liczba przeciwna U2:   {wynik_przeciwna} = {wartosc_przeciwna}")

```
# Mnożenie przyklad
``` python
def Dodaj(a, b, podstawa=2):
  """Funkcja pomocnicza dodająca dwie liczby w systemie dwójkowym."""
  wynik = ""
  przeniesienie = 0
  i = len(a) - 1
  j = len(b) - 1

  while i >= 0 or j >= 0 or przeniesienie > 0:
    cyfra_a = int(a[i]) if i >= 0 else 0
    cyfra_b = int(b[j]) if j >= 0 else 0
    suma = cyfra_a + cyfra_b + przeniesienie
    przeniesienie = suma // podstawa
    wynik = str(suma % podstawa) + wynik
    i -= 1
    j -= 1
  return wynik


def MnozBinary(a, b):
  """Rozwiązanie Ćwiczenia 6: Mnożenie dwóch liczb binarnych zapisanych jako napisy."""
  c = "0"
  d = len(b) - 1

  for i in range(d, -1, -1):
    if b[i] == "1":
      # Dodanie a z odpowiednią liczbą zer na końcu (przesunięcie w lewo)
      zera = "0" * (d - i)
      pom = a + zera if a != "0" else "0"
      c = Dodaj(c, pom, 2)

  return c.lstrip("0") if c.lstrip("0") else "0"


# Przykład użycia:
if __name__ == "__main__":
  liczba1 = "1110"  # odpowiednik 14
  liczba2 = "1101"  # odpowiednik 13
  wynik = MnozBinary(liczba1, liczba2)
  print(f"Iloczyn binarny {liczba1} * {liczba2} = {wynik}")

```
Mnożenie różnych systemów pozycyjnych


``` python
def DodajSystem(a, b, podstawa):
  """Dodaje dwie liczby w systemie o zadanej podstawie."""
  wynik = ""
  przeniesienie = 0
  i = len(a) - 1
  j = len(b) - 1

  while i >= 0 or j >= 0 or przeniesienie > 0:
    cyfra_a = int(a[i]) if i >= 0 else 0
    cyfra_b = int(b[j]) if j >= 0 else 0
    suma = cyfra_a + cyfra_b + przeniesienie
    przeniesienie = suma // podstawa
    wynik = str(suma % podstawa) + wynik
    i -= 1
    j -= 1
  return wynik


def MnozPrzezCfr(cfr, liczba, podstawa):
  """Mnoży wielocyfrową liczbę przez pojedynczą cyfrę w systemie o zadanej podstawie."""
  przn = 0
  wynik = ""

  for i in range(len(liczba) - 1, -1, -1):
    iloczyn = cfr * int(liczba[i]) + przn
    przn = iloczyn // podstawa
    wynik = str(iloczyn % podstawa) + wynik

  if przn > 0:
    wynik = str(przn) + wynik

  return wynik if wynik else "0"


def MnozSystem(a, b, podstawa):
  """Funkcja główna mnożąca dwie liczby w systemie pozycyjnym."""
  c = "0"
  d = len(b) - 1

  for i in range(d, -1, -1):
    cfr = int(b[i])
    pom = MnozPrzezCfr(cfr, a, podstawa)

    # Dopisywanie zer (odpowiednik pętli for z podręcznika C++)
    zera = "0" * (d - i)
    if pom != "0":
      pom = pom + zera
    else:
      pom = "0"

    c = DodajSystem(c, pom, podstawa)

  return c.lstrip("0") if c.lstrip("0") else "0"


# Przykład użycia dla systemu o podstawie 3 lub 10:
if __name__ == "__main__":
  podstawa = 10
  a = "123"
  b = "45"
  wynik = MnozSystem(a, b, podstawa)
  print(f"Iloczyn w systemie {podstawa}: {a} * {b} = {wynik}")

```
## 7. Ćwiczenia dla uczniów

### Zadanie 1. Dodawanie binarne

Wykonaj dodawanie pisemne:

`10101₂ + 1110₂`

Następnie sprawdź wynik w systemie dziesiętnym.

### Zadanie 2. Uzupełnianie kodu

Uzupełnij program binarny tak, aby wyświetlał również wartość przeniesienia po każdej pozycji.

### Zadanie 3. System czwórkowy

Oblicz pisemnie:

`132₄ + 213₄`

Sprawdź wynik przez konwersję na system dziesiętny.

### Zadanie 4. Walidacja danych

Rozbuduj program, aby ponownie pytał użytkownika o liczbę, jeśli zawiera ona cyfrę niedozwoloną w wybranym systemie.

### Zadanie 5. Rozbudowa programu

Dodaj możliwość wykonywania kilku działań bez ponownego uruchamiania programu. Użytkownik powinien móc zakończyć pracę, wpisując `KONIEC`.

### Zadanie 6. Analiza algorytmu

Wyjaśnij rolę operatorów `%` i `//` na przykładzie dodawania cyfr `1` i `1` w systemie binarnym oraz cyfr `3` i `3` w systemie czwórkowym.

## 8. Podsumowanie lekcji

Najważniejsze wnioski:

- Dodawanie w różnych systemach pozycyjnych przebiega według tej samej zasady.
- Podstawa systemu określa moment powstania przeniesienia.
- Operator `%` wyznacza cyfrę wyniku, a `//` – przeniesienie.
- Liczby można dodawać bezpośrednio jako napisy, przetwarzając ich cyfry od prawej do lewej.
- Poprawny program powinien sprawdzać zarówno podstawę systemu, jak i dopuszczalne cyfry.

Materiał obejmuje algorytm z podręcznika, jego implementację w Pythonie oraz rozszerzenie na różne systemy pozycyjne.
