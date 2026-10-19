# Temat: Algorytmy numeryczne i metody obliczeń przybliżonych w Pythonie

---

## 1. Metoda prostokątów

Metoda prostokątów polega na podziale przedziału $[a, b]$ na $n$ równych części o szerokości $dx = \frac{b - a}{n}$ i zsumowaniu pól prostokątów tworzonych pod wykresem funkcji.

### Specyfikacja problemu:
* **Dane:** 
  * $a, b$ – liczby rzeczywiste ($a < b$)
  * $f(x)$ – funkcja ciągła o nieujemnych wartościach dla argumentów z przedziału $[a, b]$
  * $n$ – liczba podprzedziałów (dodatnia liczba całkowita)
* **Wynik:** $s$ – przybliżona wartość pola obszaru zamkniętego ograniczonego prostymi $x = a$, $x = b$, osią $OX$ oraz funkcją $f(x)$.

### Kod w Pythonie (odpowiednik z C++ ze strony 139):
```python
import math

# Definiujemy funkcję f(x), np. f(x) = sin(x)
def f(x):
    return math.sin(x)

def calka_prostokaty():
    a = float(input("a = "))
    b = float(input("b = "))
    n = int(input("n = "))
    
    dx = (b - a) / n
    x = a
    s = 0.0
    
    for i in range(1, n + 1):
        x = x + dx
        s = s + dx * f(x)
        
    print(f"Pole obszaru: {s}")

if __name__ == "__main__":
    calka_prostokaty()
```

---

## 2. Metoda trapezów

Zamiast prostokątów stosujemy trapezy łączące wartości funkcji na krańcach każdego podprzedziału. Pole pojedynczego trapezu wynosi:
$$P = \frac{1}{2} \cdot (f(x_{i-1}) + f(x_i)) \cdot dx$$

### Kod w Pythonie (odpowiednik z C++ ze strony 141):
```python
import math

def f(x):
    return math.sin(x)

def calka_trapezy():
    a = float(input("a = "))
    b = float(input("b = "))
    n = int(input("n = "))
    
    dx = (b - a) / n
    x = a
    f1 = f(x)
    s = 0.0
    
    for i in range(1, n + 1):
        x = x + dx
        f2 = f(x)
        s = s + dx * (f1 + f2) / 2
        f1 = f2
        
    print(f"Pole obszaru: {s}")

if __name__ == "__main__":
    calka_trapezy()
```

---

## 3. Przybliżenie liczby $\pi$ metodą Monte Carlo

Metoda Monte Carlo wykorzystuje losowanie punktów w obrębie kwadratu o boku $a = 2$ i sprawdza, ile z nich znajduje się wewnątrz koła o promieniu $r = 1$.

Stosunek pól wynosi:
$$\frac{\pi \cdot r^2}{a^2} = \frac{\pi}{4}$$

Stąd:
$$\pi \approx 4 \cdot \frac{n_0}{n}$$

### Specyfikacja:
* **Dane:** $n$ – całkowita liczba losowań punktów $(x, y) \in [-1, 1] \times [-1, 1]$.
* **Wynik:** $pi$ – przybliżona wartość liczby $\pi$.

### Kod w Pythonie (odpowiednik z C++ ze strony 142):
```python
import random

def monte_carlo_pi():
    n = int(input("Liczba punktow: "))
    n0 = 0
    
    for _ in range(n):
        x = random.uniform(-1, 1)
        y = random.uniform(-1, 1)
        
        if x * x + y * y <= 1:
            n0 += 1
            
    pi = 4 * n0 / n
    print(f"Przyblizenie pi: {pi}")

if __name__ == "__main__":
    monte_carlo_pi()
```

---

## 4. Symulacja ruchów Browna

Symulacja losowego przemieszczania się cząstki na płaszczyźnie. W każdym kroku cząstka przesuwa się losowo o $+1$ lub $-1$ w osi $OX$ oraz $OY$.

### Specyfikacja:
* **Dane:** $n$ – liczba ruchów w symulacji.
* **Wynik:** $(x, y)$ – położenie cząstki po $n$ ruchach.

### Kod w Pythonie (odpowiednik pseudokodu ze strony 145):
```python
import random

def ruchy_browna():
    n = int(input("Liczba ruchow: "))
    x = 0
    y = 0
    
    for _ in range(n):
        dx = random.choice([-1, 1])
        dy = random.choice([-1, 1])
        x += dx
        y += dy
        
    print(f"Koncowe polozenie czastki: ({x}, {y})")

if __name__ == "__main__":
    ruchy_browna()
```

---

## 5. Ćwiczenia dla uczniów z podręcznika

1. **Ćwiczenie 7 (str. 142):** Napisz program obliczający przybliżoną wartość liczby $\pi$, wykorzystując metodę prostokątów lub trapezów dla funkcji $f(x) = \sqrt{1 - x^2}$ na przedziale $[-1, 1]$. *(Wskazówka: Pole obszaru pod tą funkcją wynosi $\frac{\pi}{2}$, więc otrzymany wynik s należy pomnożyć przez 2)*.
2. **Zadanie 3 (str. 147):** Zmodyfikuj programy dla metody prostokątów i trapezów tak, aby obliczały prawidłowo pola także dla ujemnych wartości funkcji (użyj funkcji wartości bezwzględnej `abs()`).