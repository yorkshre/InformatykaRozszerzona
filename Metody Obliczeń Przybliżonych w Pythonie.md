# Metody Obliczeń Przybliżonych w Pythonie

Materiały oparte na zagadnieniach z algorytmów numerycznych: metody bisekcji (połowienia) oraz metody Newtona-Raphsona.

---

## 1. Wprowadzenie: Dlaczego obliczenia przybliżone?
Wielu problemów matematycznych nie da się rozwiązać analitycznie lub dokładny wynik jest trudny do uzyskania ze względu na ograniczenia reprezentacji liczb w pamięci komputera. W takich sytuacjach stosujemy **metody numeryczne**, które dają wyniki przybliżone z określoną dokładnością ($\text{EPS}$).

---

## 2. Znajdowanie miejsc zerowych funkcji – Metoda Bisekcji (Połowienia)

Metoda bisekcji pozwala znaleźć miejsce zerowe funkcji $f(x) = 0$ w przedziale $[a, b]$, działając na zasadzie podobnej do przeszukiwania binarnego.

### Warunki konieczne stosowania metody:
1. Funkcja $f$ musi być ciągła w przedziale $[a, b]$.
2. Wartości funkcji na końcach przedziału muszą mieć **różne znaki**, co zapisujemy jako:
   $$f(a) \cdot f(b) < 0$$
   Oznacza to, że wewnątrz przedziału znajduje się co najmniej jedno miejsce zerowe.

### Algorytm (Pseudokod)
1. Sprawdź, czy $f(a) \cdot f(b) < 0$. Jeśli nie, przerwij lub wybierz inny przedział.
2. Oblicz środek przedziału: $x_0 = \frac{a + b}{2}$.
3. Jeśli $f(x_0) = 0$ lub długość przedziału jest mniejsza od żądanej dokładności ($\text{EPS}$), kończymy – $x_0$ jest miejscem zerowym.
4. Jeśli $f(a) \cdot f(x_0) < 0$, to miejsce zerowe leży w lewej połowie, więc przypisz $b \leftarrow x_0$.
5. W przeciwnym razie miejsce zerowe leży w prawej połowie, więc przypisz $a \leftarrow x_0$.
6. Powtarzaj kroki 2–5 do osiągnięcia żądanej dokładności.

---

### Implementacja w Pythonie

```python
def metoda_bisekcji(f, a: float, b: float, eps: float = 0.00001, max_iter: int = 1000) -> float:
    """
    Znajduje miejsce zerowe funkcji f w przedziale [a, b] metodą bisekcji.
    
    :param f: funkcja, której miejsce zerowe wyznaczamy
    :param a: lewy koniec przedziału
    :param b: prawy koniec przedziału
    :param eps: dokładność obliczeń (EPS)
    :param max_iter: zabezpieczenie przed nieskończoną pętlą
    :return: przybliżone miejsce zerowe
    """
    # Krok 1: Sprawdzenie warunku istnienia miejsca zerowego o różnych znakach
    fa = f(a)
    fb = f(b)
    
    if fa * fb >= 0:
        raise ValueError("Funkcja musi mieć różne znaki na końcach przedziału [a, b] (f(a) * f(b) < 0).")
    
    x0 = a
    for _ in range(max_iter):
        # Krok 2: Obliczenie środka przedziału
        x0 = (a + b) / 2.0
        fx0 = f(x0)
        
        # Krok 3: Sprawdzenie warunku stopu
        if abs(fx0) < 1e-12 or (b - a) / 2.0 < eps:
            break
            
        # Krok 4 i 5: Zawężanie przedziału
        if fa * fx0 < 0:
            b = x0
            fb = fx0
        else:
            a = x0
            fa = fx0
            
    return x0

# --- Przykład użycia ---
# Funkcja z wykresu 8.2: f(x) = (1/6)*x^3 - x^2 + (1/2)*x + (5/3)
def funkcja_f(x):
    return (1.0 / 6.0) * x**3 - x**2 + 0.5 * x + (5.0 / 3.0)

# Szukamy miejsca zerowego w przedziale [3, 10] (zgodnie z rys. 8.2)
try:
    miejsce_zerowe = metoda_bisekcji(funkcja_f, a=3.0, b=10.0, eps=0.00001)
    print(f"Wyznaczone miejsce zerowe: {miejsce_zerowe:.5f}")
except ValueError as e:
    print(e)
```

---

## 3. Zastosowanie: Obliczanie pierwiastka kwadratowego metodą bisekcji

Aby obliczyć pierwiastek kwadratowy z liczby $c > 1$ bez użycia funkcji `math.sqrt()`, możemy poszukiwać miejsca zerowego funkcji:
$$f(x) = x^2 - c$$
w przedziale $[1, c]$.

```python
def pierwiastek_bisekcja(c: float, eps: float = 0.00001) -> float:
    """Oblicza pierwiastek kwadratowy z liczby c > 1 metodą bisekcji."""
    if c < 1:
        raise ValueError("Liczba c musi być większa od 1.")
        
    a = 1.0
    b = c
    
    while (b - a) / 2.0 > eps:
        pkw = (a + b) / 2.0
        if pkw * pkw < c:
            a = pkw
        else:
            b = pkw
            
    return (a + b) / 2.0

# Przykład: pierwiastek z 2
wynik_p = pierwiastek_bisekcja(2.0, eps=0.00001)
print(f"Pierwiastek kwadratowy z 2 (metoda bisekcji): {wynik_p:.5f}")
```

---

## 4. Obliczanie pierwiastka kwadratowego metodą Newtona-Raphsona

Metoda Newtona-Raphsona (stycznych) pozwala na znacznie szybsze zbieganie do wyniku. Poszukiwanie $\sqrt{c}$ odpowiada długości boku kwadratu o polu $c$. 

### Algorytm:
1. Rozpoczynamy od dowolnego prostokąta o bokach $a$ i $b$, takiego że $a \cdot b = c$ (np. $a = c$, $b = 1$).
2. W każdym kroku ulepszamy bok $a$, zastępując go średnią arytmetyczną dotychczasowych boków: $a_{\text{nowy}} = \frac{a + b}{2}$.
3. Wyznaczamy nowy bok $b$, dzieląc pole przez $a$: $b_{\text{nowy}} = \frac{c}{a_{\text{nowy}}}$.
4. Powtarzamy, aż różnica między długościami boków $a$ i $b$ będzie mniejsza od założonej dokładności.

```python
def pierwiastek_newtona(c: float, eps: float = 0.00001) -> float:
    """Oblicza pierwiastek kwadratowy z liczby c metodą Newtona-Raphsona."""
    if c < 0:
        raise ValueError("Nie można liczyć pierwiastka z liczby ujemnej.")
    if c == 0:
        return 0.0
        
    a = c
    b = 1.0
    
    # Dopóki różnica między bokami jest większa od dokładności
    while abs(a - b) > eps:
        a = (a + b) / 2.0
        b = c / a
        
    return a

# Przykład: pierwiastek z 2
wynik_n = pierwiastek_newtona(2.0, eps=0.00001)
print(f"Pierwiastek kwadratowy z 2 (metoda Newtona): {wynik_n:.5f}")
```