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
    fa = f(a)
    fb = f(b)
    
    if fa * fb >= 0:
        raise ValueError("Funkcja musi mieć różne znaki na końcach przedziału [a, b] (f(a) * f(b) < 0).")
    
    x0 = a
    for _ in range(max_iter):
        x0 = (a + b) / 2.0
        fx0 = f(x0)
        
        if abs(fx0) < 1e-12 or (b - a) / 2.0 < eps:
            break
            
        if fa * fx0 < 0:
            b = x0
            fb = fx0
        else:
            a = x0
            fa = fx0
            
    return x0

# Przykład użycia (funkcja z wykresu 8.2)
def funkcja_f(x):
    return (1.0 / 6.0) * x**3 - x**2 + 0.5 * x + (5.0 / 3.0)

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
```

---

## 4. Obliczanie pierwiastka kwadratowego metodą Newtona-Raphsona

Metoda Newtona-Raphsona (stycznych) pozwala na szybkie wyznaczanie wartości $\sqrt{c}$ poprzez iteracyjne przekształcanie prostokąta o polu $c$ w kwadrat.

### Przykład krok po kroku: Wyznaczanie $\sqrt{3}$
Załóżmy pole $P = c = 3$ oraz żądaną dokładność $\text{EPS} = 0,00001$. Startujemy od prostokąta początkowego o bokach $a = 1$ oraz $b = 3$ (ponieważ $1 \cdot 3 = 3$).

* **Krok 1:**
  $$a = \frac{1 + 3}{2} = 2, \quad b = \frac{3}{2} = 1,5$$
  Sprawdzamy warunek stopu: $|a - b| = |2 - 1,5| = 0,5 > 0,00001$ (kontynuujemy).

* **Krok 2:**
  $$a = \frac{2 + 1,5}{2} = 1,75, \quad b = \frac{3}{1,75} \approx 1,71429$$
  Warunek stopu: $|1,75 - 1,71429| \approx 0,03571 > 0,00001$ (szukamy dalej).

* **Krok 3:**
  $$a \approx \frac{1,75 + 1,71429}{2} \approx 1,73215, \quad b \approx \frac{3}{1,73215} \approx 1,73195$$
  Warunek stopu: $|1,73215 - 1,73195| \approx 0,0002 > 0,00001$ (kontynuujemy).

* **Krok 4:**
  $$a \approx \frac{1,73215 + 1,73195}{2} \approx 1,73205, \quad b \approx \frac{3}{1,73205} \approx 1,73205$$
  Warunek stopu: $|a - b| < 0,00001$. Algorytm kończy działanie.

Wynik przybliżony: $\sqrt{3} \approx 1,73205$.

### Implementacja w Pythonie

```python
def pierwiastek_newtona(c: float, eps: float = 0.00001) -> float:
    """Oblicza pierwiastek kwadratowy z liczby c metodą Newtona-Raphsona."""
    if c < 0:
        raise ValueError("Nie można liczyć pierwiastka z liczby ujemnej.")
    if c == 0:
        return 0.0
        
    a = c
    b = 1.0
    
    while abs(a - b) > eps:
        a = (a + b) / 2.0
        b = c / a
        
    return a

# Przykład użycia dla pierwiastka z 3
wynik_3 = pierwiastek_newtona(3.0, eps=0.00001)
print(f"Pierwiastek z 3 (metoda Newtona): {wynik_3:.5f}")
```
