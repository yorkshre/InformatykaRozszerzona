# Obliczanie wartości wielomianu w Pythonie

Pojęcie wielomianu zna z lekcji matematyki. Wielomianów używa się m.in. do opisu zależności między wielkościami fizycznymi w modelowaniu różnych zjawisk, a także mają one zastosowanie w informatyce (np. do wyznaczania przybliżonej wartości funkcji, gdy dokładna wartość jest trudna do znalezienia).

---

## 1. Sformułowanie problemu

**Wielomian** stopnia $n$ zmiennej rzeczywistej $x$ to funkcja postaci:
$$W(x) = a_n x^n + a_{n-1} x^{n-1} + ... + a_2 x^2 + a_1 x + a_0$$

gdzie:
* $a_n \neq 0$, $n$ – liczba naturalna dodatnia,
* $a_n, a_{n-1}, ..., a_2, a_1, a_0$ – współczynniki wielomianu będące liczbami rzeczywistymi,
* $a_0$ – wyraz wolny.

### Specyfikacja problemu:
* **Dane:** 
  * $n$ – stopień wielomianu (liczba całkowita dodatnia),
  * $A[0..n]$ – tablica współczynników wielomianu, gdzie element $A[0]$ odpowiada współczynnikowi $a_0$, $A[1]$ – współczynnikowi $a_1$ itd.,
  * $x$ – liczba rzeczywista (argument).
* **Wynik:** 
  * $y$ – wartość wielomianu dla argumentu $x$.

---

## 2. Algorytm naiwny

Wartość wielomianu można policzyć, podstawiając argument bezpośrednio do wzoru. Sumowanie wyrazów wielomianu warto rozpocząć od wyrazu wolnego i wykorzystywać policzoną aktualnie potęgę $x$ do wyliczenia kolejnej ($x^n = x^{n-1} \cdot x$).

### Implementacja w Pythonie:

```python
def horner_naiwny(A, n, x):
    """
    Oblicza wartość wielomianu algorytmem naiwnym.
    A - tablica współczynników [a_0, a_1, ..., a_n]
    n - stopień wielomianu
    x - argument
    """
    y = A[0]  # Wartość początkowa to wyraz wolny a_0 (x^0 = 1)
    z = 1     # Zmienna pomocnicza przechowująca kolejne potęgi x
    
    for i in range(1, n + 1):
        z = z * x
        y = y + A[i] * z
        
    return y

# Przykład użycia:
# W(x) = 2x^3 + 3x^2 + x + 5 dla x = 2
# Współczynniki: A = [5, 1, 3, 2] (od a_0 do a_n)
wspolczynniki = [5, 1, 3, 2]
stopien = 3
argument = 2

wynik_naiwny = horner_naiwny(wspolczynniki, stopien, argument)
print(f"Wynik algorytmem naiwnym: {wynik_naiwny}")
```

---

## 3. Schemat Hornera (Algorytm optymalny)

Schemat Hornera polega na wielokrotnym wyłączaniu argumentu przed nawias we wzorze ogólnym funkcji wielomianowej, aż do najbardziej wewnętrznych nawiasów. 

Dla wielomianu stopnia $n$ schemat ten wykonuje $n$ operacji mnożenia i $n$ operacji dodawania – jest to algorytm optymalny.

Wzór przekształcony:
$$W(x) = (...((a_n \cdot x + a_{n-1}) \cdot x + a_{n-2}) \cdot x + ...) \cdot x + a_0$$

### Implementacja w Pythonie:

```python
def schemat_hornera(A, n, x):
    """
    Oblicza wartość wielomianu przy użyciu schematu Hornera.
    A - tablica współczynników [a_0, a_1, ..., a_n] lub odwrócona,
        w zależności od przyjętej konwencji. 
        Tutaj zakładamy standard: A[0] = a_0, ..., A[n] = a_n.
    """
    y = A[n]  # Zaczynamy od najwyższego współczynnika a_n
    
    for i in range(n - 1, -1, -1):
        y = y * x + A[i]
        
    return y

# Przykład użycia dla tych samych danych:
wynik_horner = schemat_hornera(wspolczynniki, stopien, argument)
print(f"Wynik schematem Hornera: {wynik_horner}")
```

---

## 4. Schemat Hornera dla systemów pozycyjnych

Schemat Hornera wykorzystuje się również podczas zamiany liczby z systemu dwójkowego (lub dowolnego innego o podstawie $p$) na liczbę dziesiętną. Wtedy cyfry liczby pełnią rolę współczynników wielomianu, a podstawą jest $p$.

Przykład zamiany liczby binarnej reprezentowanej przez ciąg cyfr na wartość dziesiętną:

```python
def binarny_na_dziesietny(bin_str):
    """
    Zamienia liczbę zapisaną w systemie binarnym na dziesiętną 
    przy użyciu schematu Hornera.
    """
    y = 0
    for cyfra in bin_str:
        y = y * 2 + int(cyfra)
    return y

# Przykład dla liczby binarnej "1101" (co daje 13 w systemie dziesiętnym)
binarna = "1101"
dziesietna = binarny_na_dziesietny(binarna)
print(f"Liczba binarna {binarna} w systemie dziesiętnym to: {dziesietna}")
```

---

## 5. Szybkie potęgowanie (Algorytm iteracyjny)

Wersja iteracyjna szybkiego podnoszenia do potęgi wykorzystuje binarny zapis wykładnika potęgi. Przeglądając cyfry binarne od prawej do lewej, sprawdzamy, czy w danej pozycji występuje jedynka – jeśli tak, mnożymy wynik przez podstawę potęgi, a w każdym kroku podnosimy podstawę do kwadratu.

### Implementacja w Pythonie:

```python
def szybkie_potegowanie(podstawa, wykladnik):
    """
    Oblicza potęgę (podstawa ^ wykladnik) przy użyciu 
    algorytmu szybkiego potęgowania iteracyjnego.
    """
    wynik = 1
    aktualna_podstawa = podstawa
    
    while wykladnik > 0:
        if wykladnik % 2 == 1:  # Jeśli najmłodszy bit to 1
            wynik = wynik * aktualna_podstawa
        aktualna_podstawa = aktualna_podstawa * aktualna_podstawa
        wykladnik = wykladnik // 2  # Przesunięcie bitowe w prawo / dzielenie integer
        
    return wynik

# Przykład użycia: 2^13
podst = 2
wykl = 13
wynik_potegi = szybkie_potegowanie(podst, wykl)
print(f"{podst}^{wykl} = {wynik_potegi}")