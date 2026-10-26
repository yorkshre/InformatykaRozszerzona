# 📚 Temat lekcji: Algorytmy badające własności geometryczne – Położenie punktów względem prostej

---

## 🎯 Cele lekcji
Po przeprowadzeniu tej lekcji będziesz potrafić:
1. Zrozumieć matematyczną zasadę badania położenia punktu względem prostej opisanej równaniem ogólnym $Ax + By + C = 0$.
2. Wyznaczyć, po której stronie prostej znajduje się dany punkt na podstawie znaku wartości wyrażenia $Ax_P + By_P + C$.
3. Sprawdzić, czy dwa punkty leżą po tej samej stronie prostej.
4. Zaimplementować strukturę/klasę reprezentującą punkt 2D w języku Python.
5. Napisać algorytm sprawdzający położenie punktów w Pythonie z wykorzystaniem podziału na funkcje.

---

## 💡 Teoria w pigułce

Równanie ogólne prostej ma postać:
$$Ax + By + C = 0$$
gdzie $A \neq 0$ lub $B \neq 0$.

Gdy podstawimy współrzędne punktu $P(x_P, y_P)$ do lewej strony równania ogólnego prostej, otrzymamy wartość wyrażenia $W = A x_P + B y_P + C$:
- **$W = 0$**: Punkt $P$ **leży na prostej**.
- **$W > 0$**: Punkt $P$ leży po jednej stronie prostej (w tzw. dodatniej półpłaszczyźnie).
- **$W < 0$**: Punkt $P$ leży po drugiej stronie prostej (w tzw. ujemnej półpłaszczyźnie).

### Czy dwa punkty $P_1(x_1, y_1)$ i $P_2(x_2, y_2)$ leżą po tej samej stronie prostej?
Obliczamy wartości dla obu punktów:
- $W_1 = A x_1 + B y_1 + C$
- $W_2 = A x_2 + B y_2 + C$

Dwa punkty leżą po **tej samej stronie prostej**, jeśli wyniki $W_1$ i $W_2$ mają **ten sam znak**.  
W programowaniu najprościej sprawdzić to badając **iloczyn**:
$$W_1 \cdot W_2 > 0$$
Jeśli $W_1 \cdot W_2 > 0$, to punkty leżą po tej samej stronie prostej (żaden z nich nie leży na prostej i oba mają ten sam znak).

---

## 💻 Implementacja w Pythonie

Poniższy kod implementuje algorytm w języku Python z użyciem klasy `Punkt` oraz czytelnego podziału na funkcje.

```python
from dataclasses import dataclass

# 1. Struktura reprezentująca punkt na płaszczyźnie 2D
@dataclass
class Punkt:
    x: float
    y: float


# 2. Funkcja pomocnicza do wczytywania współrzędnych punktu z klawiatury
def czytaj_punkt(info: str) -> Punkt:
    print(info)
    x = float(input("x = "))
    y = float(input("y = "))
    return Punkt(x, y)


# 3. Funkcja obliczająca wartość wyrażenia Ax + By + C dla danego punktu
def wartosc_prostej(A: float, B: float, C: float, p: Punkt) -> float:
    return A * p.x + B * p.y + C


# 4. Funkcja sprawdzająca, czy dwa punkty leżą po tej samej stronie prostej
def pkt_po_tej_samej_stronie(A: float, B: float, C: float, p1: Punkt, p2: Punkt) -> bool:
    w1 = wartosc_prostej(A, B, C, p1)
    w2 = wartosc_prostej(A, B, C, p2)
    
    # Punkty leżą po tej samej stronie, gdy iloczyn wartości jest większy od 0
    return (w1 * w2) > 0


# 5. Program główny (main)
def main():
    print("=== Badanie położenia punktów względem prostej Ax + By + C = 0 ===")
    
    # Pobranie współczynników równania ogólnego prostej
    A = float(input("Podaj A: "))
    B = float(input("Podaj B: "))
    C = float(input("Podaj C: "))
    
    # Wczytanie dwóch punktów
    p1 = czytaj_punkt("Podaj współrzędne pierwszego punktu:")
    p2 = czytaj_punkt("Podaj współrzędne drugiego punktu:")
    
    # Sprawdzenie warunku i wyświetlenie wyniku
    if pkt_po_tej_samej_stronie(A, B, C, p1, p2):
        print("\nPunkty leżą po TEJ SAMEJ stronie prostej.")
    else:
        print("\nPunkty NIE leżą po tej samej stronie prostej (leżą po przeciwnych stronach lub co najmniej jeden z nich leży na prostej).")

if __name__ == "__main__":
    main()
```

---

## ✍️ Zbiór Ćwiczeń i Zadań

### **Ćwiczenie 1: Analiza teoretyczna (bez komputera)**
Dana jest prosta o równaniu $2x - 3y + 6 = 0$.
Przeanalizuj i oblicz położenie następujących punktów:
1. $P_1(0, 2)$
2. $P_2(3, 0)$
3. $P_3(-2, 1)$

*Pytanie:* Czy punkty $P_2$ i $P_3$ leżą po tej samej stronie prostej? Wykonaj obliczenia iloczynu $W_2 \cdot W_3$.

---

### **Ćwiczenie 2: Określenie dokładnego położenia jednego punktu**
Napisz program (lub funkcję w Pythonie), który przyjmuje współczynniki prostej $A, B, C$ oraz jeden punkt $P(x, y)$ i wypisuje jeden z trzech komunikatów:
- `"Punkt leży na prostej"`
- `"Punkt leży po dodatniej stronie prostej"`
- `"Punkt leży po ujemnej stronie prostej"`

**Szkielet rozwiązania:**
```python
def okresl_polozenie_punktu(A: float, B: float, C: float, p: Punkt):
    w = A * p.x + B * p.y + C
    if w > 0:
        print("Punkt leży po DODATNIEJ stronie prostej.")
    elif w < 0:
        print("Punkt leży po UJEMNEJ stronie prostej.")
    else:
        print("Punkt LEŻY NA prostej.")
```

---

### **Ćwiczenie 3: Rozbudowany klasyfikator dwóch punktów**
Rozbuduj funkcję `pkt_po_tej_samej_stronie`, aby zwracała bardziej szczegółowe informacje (np. napis `str` lub kod stanu), uwzględniając przypadki:
1. Oba punkty leżą po tej samej stronie prostej.
2. Punkty leżą po przeciwnych stronach prostej.
3. Co najmniej jeden z punktów leży bezpośrednio na prostej.

---

### **Ćwiczenie 4: Sprawdzanie przynależności punktu do odcinka (Zadanie wyzwanie)**
Napisz program w Pythonie, który sprawdza, czy dany punkt $P(x, y)$ leży **na odcinku** ograniczonym punktami $A(x_A, y_A)$ oraz $B(x_B, y_B)$.

*Wskazówka:*
1. Prosta przechodząca przez punkty $A$ i $B$ ma równanie: $(y_A - y_B)x + (x_B - x_A)y + (x_A y_B - x_B y_A) = 0$.
2. Punkt $P$ leży na odcinku $AB$, jeśli leży na tej prostej oraz jego współrzędne mieszczą się w przedziałach:  
   $\min(x_A, x_B) \le x_P \le \max(x_A, x_B)$  
   oraz  
   $\min(y_A, y_B) \le y_P \le \max(y_A, y_B)$.