# Obliczanie wartości wielomianu

## Sformułowanie problemu

Pojęcie wielomianu znasz z lekcji matematyki. Wielomianem stopnia $n$ zmiennej rzeczywistej $x$ nazywamy funkcję postaci:

$$W(x) = a_n x^n + a_{n-1} x^{n-1} + \dots + a_2 x^2 + a_1 x + a_0$$

gdzie $a_n \neq 0$, $n$ jest liczbą naturalną dodatnią, a współczynniki $a_n, a_{n-1}, \dots, a_0$ są liczbami rzeczywistymi. Wyraz $a_0$ nazywamy wyrazem wolnym.

### Specyfikacja problemu
* **Dane:** 
  * $n$ – liczba całkowita dodatnia oznaczająca stopień wielomianu.
  * $A[0 \dots n]$ – tablica liczb rzeczywistych będących współczynnikami wielomianu, gdzie $A[n] \neq 0$, element $A[0]$ odpowiada współczynnikowi $a_0$, $A[1]$ współczynnikowi $a_1$ itd.
  * $x$ – liczba rzeczywista (argument).
* **Wynik:** $y$ – wartość wielomianu o współczynnikach z tablicy $A$ dla argumentu $x$.

---

##  Obliczanie wartości wielomianu algorytmem naiwnym

Wartość wielomianu dla danego argumentu można policzyć, podstawiając ten argument bezpośrednio do wzoru wielomianu. Sumowanie wyrazów wielomianu warto rozpocząć od wyrazu wolnego i wykorzystywać policzoną aktualnie potęgę $x$ do wyliczenia kolejnej ($x^n = x^{n-1} \cdot x$ dla $n > 1$).

### Ćwiczenie 1
> Napisz program, który obliczy wartość wielomianu algorytmem naiwnym zgodnie ze specyfikacją podaną w podręczniku. Wykorzystaj w temacie funkcje `Czytaj` i `W`.

#### Rozwiązanie w Pythonie:
```python
def czytaj_wspolczynniki():
    n = int(input("Podaj stopień wielomianu: "))
    # Tworzymy tablicę na n+1 współczynników
    A = [0.0] * (n + 1)
    for i in range(n, -1, -1):
        A[i] = float(input(f"Podaj a{i} = "))
    return n, A

def W_naiwny(n, A, x):
    y = A[0]
    z = 1
    for i in range(1, n + 1):
        z = z * x
        y = y + A[i] * z
    return y

# Program główny
if __name__ == "__main__":
    n, A = czytaj_wspolczynniki()
    x = float(input("Podaj argument x: "))
    wynik = W_naiwny(n, A, x)
    print(f"Wartość wielomianu (algorytm naiwny): {wynik}")
```

---

## Obliczanie wartości wielomianu za pomocą schematu Hornera

Liczbę operacji arytmetycznych podczas wyznaczania wartości wielomianu możemy zmniejszyć, jeśli wykorzystamy **schemat Hornera**. Jego działanie polega na wielokrotnym wyłączaniu $x$ przed nawias:
$$W(x) = x \cdot (x \cdot (\dots (a_n \cdot x + a_{n-1}) + \dots + a_1) + a_0$$

Algorytm ten wykonuje dokładnie $n$ operacji mnożenia i $n$ operacji dodawania, co czyni go algorytmem optymalnym.

### Ćwiczenie 2
> Napisz program, który obliczy wartość wielomianu z wykorzystaniem schematu Hornera, zgodnie ze specyfikacją.

#### Rozwiązanie w Pythonie:
```python
def W_horner(n, A, x):
    y = A[n]  # wartość początkowa to współczynnik przy najwyższej potędze
    for i in range(n - 1, -1, -1):
        y = y * x + A[i]
    return y

# Program główny
if __name__ == "__main__":
    n, A = czytaj_wspolczynniki()
    x = float(input("Podaj argument x: "))
    wynik = W_horner(n, A, x)
    print(f"Wartość wielomianu (schemat Hornera): {wynik}")
```

---

## Wybrane zadania z podręcznika z rozwiązaniami

### Zadanie 2
> Napisz program, który z pliku tekstowego przekazanego ci przez nauczyciela (`wielomian.txt`) wczyta stopień i współczynniki wielomianu, a następnie z klawiatury obliczy wartość wielomianu, korzystając ze schematu Hornera. Stopień wielomianu znajduje się w pierwszym wierszu pliku tekstowego, w kolejnych wierszach podane są współczynniki wielomianu (od $a_n$ do $a_0$).

#### Rozwiązanie w Pythonie:
```python
def horner_z_pliku(nazwa_pliku):
    with open(nazwa_pliku, "r", encoding="utf-8") as f:
        n = int(f.readline().strip())
        # Wczytujemy współczynniki od a_n do a_0
        A = [float(f.readline().strip()) for _ in range(n + 1)]
        # Odwracamy tablicę, aby A[0] odpowiadało wyrazowi wolnemu a_0
        A.reverse()

    x = float(input("Podaj argument x: "))
    
    # Schemat Hornera
    y = A[n]
    for i in range(n - 1, -1, -1):
        y = y * x + A[i]
        
    print(f"Wynik obliczony z pliku dla x = {x} wynosi: {y}")

# Przykład wywołania (wymaga utworzenia pliku wielomian.txt):
# horner_z_pliku("wielomian.txt")
```
##Zadania do samodzielnego wykonania

### Zadanie 1

* **Treść zadania:** 
  > Napisz program, który wczyta z klawiatury dwa wielomiany, a następnie wyznaczy wielomian będący ich sumą i go wypisze.

* **Omówienie i wskazówki algorytmiczne:**
  Zadanie polega na dodaniu do siebie dwóch wielomianów reprezentowanych np. za pomocą tablic współczynników, gdzie indeks tablicy odpowiada potędze zmiennej $x$.
  1. **Wczytywanie danych:** Program musi najpierw pobrać stopień oraz współczynniki pierwszego wielomianu, a następnie stopień i współczynniki drugiego wielomianu.
  2. **Dodawanie:** Suma wielomianów polega na dodaniu do siebie współczynników stojących przy tych samych potęgach zmiennej $x$.
  3. **Różne stopnie:** Jeżeli wielomiany mają różne stopnie, należy pamiętać, że współczynniki przy wyższych potęgach brakujących w krótszym wielomianie wynoszą $0$.

---

### Zadanie 2

* **Treść zadania:** 
  > Napisz program, który wczyta z klawiatury podstawę systemu pozycyjnego z zakresu od 2 do 9, liczbę cyfr liczby oraz cyfry tej liczby i obliczy jej wartość dziesiętną, korzystając ze schematu Hornera.

* **Omówienie i wskazówki algorytmiczne:**
  Zadanie dotyczy konwersji liczby z systemu o podstawie $p$ ($2 \le p \le 9$) na system dziesiętny.
  1. **Wczytywanie danych:** Użytkownik podaje podstawę systemu $p$, liczbę cyfr $n$ oraz kolejne cyfry tworzące tę liczbę (najczęściej od najbardziej znaczącej do najmniej znaczącej).
  2. **Schemat Hornera:** Do obliczenia wartości dziesiętnej wykorzystuje się schemat Hornera, który w tym kontekście sprowadza się do efektywnego obliczania wartości wielomianu. Algorytm zaczyna od zera (lub pierwszej cyfry), a w każdym kroku mnoży dotychczasowy wynik przez podstawę systemu $p$ i dodaje kolejną cyfrę:
     $$\text{wynik} = \text{wynik} \cdot p + \text{cyfry}[i]$$
