# 5. Czy ta liczba jest pierwsza?

## 5.1. Liczby pierwsze i złożone
* **Liczba pierwsza** to liczba całkowita dodatnia, która ma dokładnie dwa różne dzielniki: liczbę 1 i samą siebie. Liczba 1 nie jest liczbą pierwszą.
* **Liczba złożona** to liczba większa od 1, która nie jest liczbą pierwszą.
* Wśród liczb mniejszych od 100 znajduje się dwadzieścia pięć liczb pierwszych: $2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97$.

---

## 5.2. Sprawdzanie, czy liczba jest pierwsza (Algorytmy)

### Algorytm 1 (Podstawowy zoptymalizowany do $\sqrt{n}$)
Dzielników liczby $n$ poszukujemy w zakresie od $2$ do $\sqrt{n}$ (co zapisujemy jako $d \cdot d \le n$). Jeśli znajdziemy jakikolwiek dzielnik, liczba jest złożona.

```python
def czy_pierwsza_1(n):
    if n <= 1:
        return False
    d = 2
    while d * d <= n:
        if n % d == 0:
            return False
        d += 1
    return True
```

### Algorytm 2 (Uwzględnienie liczb nieparzystych – Ćwiczenie 1)
Jedyną parzystą liczbą pierwszą jest 2. Po sprawdzeniu dwójki możemy pomijać liczby parzyste i sprawdzać dzielniki tylko w zbiorze liczb nieparzystych ($3, 5, 7, \dots$).

```python
def czy_pierwsza_2(n):
    if n <= 1:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False
    
    d = 3
    while d * d <= n:
        if n % d == 0:
            return False
        d += 2  # sprawdzamy tylko liczby nieparzyste
    return True
```

### Algorytm 3 (Postać $6i \pm 1$ – Ćwiczenie 2)
Każda liczba pierwsza większa od 3 daje się przedstawić w postaci $6i - 1$ lub $6i + 1$, gdzie $i$ jest liczbą całkowitą dodatnią. Pozwala to na jednoczesne sprawdzanie dwóch dzielników w jednej iteracji ($6i-1$ oraz $6i+1$).

```python
def czy_pierwsza_3(n):
    if n <= 1:
        return False
    if n <= 3:
        return True
    if n % 2 == 0 or n % 3 == 0:
        return False
        
    d = 5
    while d * d <= n:
        if n % d == 0 or n % (d + 2) == 0:
            return False
        d += 6
    return True
```

---

## 5.3. Rozkładamy liczbę na czynniki pierwsze (Faktoryzacja)
Każdą liczbę złożoną można przedstawić w postaci iloczynu liczb pierwszych (np. $70 = 2 \cdot 5 \cdot 7$). 

**Algorytm:** Badamy resztę z dzielenia liczby przez kolejne liczby naturalne zaczynając od 2. Jeśli dzielenie jest bez reszty, wypisujemy czynnik i dzielimy przez niego liczbę. Postępujemy tak dopóki liczba nie osiągnie wartości 1.

```python
def rozklad_na_czynniki(n):
    print(f"Rozkład liczby {n}: ", end="")
    d = 2
    czynniki = []
    
    while d * d <= n:
        while n % d == 0:
            czynniki.append(str(d))
            n //= d
        d += 1
        
    if n > 1:
        czynniki.append(str(n))
        
    print(" * ".join(czynniki))

# Przykład z podręcznika: n = 456
rozklad_na_czynniki(456)  # Wynik: 456 = 2 * 2 * 2 * 3 * 19
```

---

## 📝 Zadania z rozwiązaniami

### Zadanie A
Napisz program w Pythonie, który pobierze od użytkownika liczbę całkowitą i sprawdzi za pomocą schematu zoptymalizowanego ($\sqrt{n}$), czy jest ona liczbą pierwszą, wypisując odpowiedni komunikat (`"TAK"` lub `"NIE"`).

#### Rozwiązanie:
```python
def zadanie_sprawdz_pierwsza():
    n = int(input("Podaj liczbę n: "))
    if czy_pierwsza_1(n):
        print("TAK")
    else:
        print("NIE")

# zadanie_sprawdz_pierwsza()
```

### Zadanie B
Napisz program, który znajdzie i wypisze wszystkie liczby pierwsze z przedziału od $1$ do $100$.

#### Rozwiązanie:
```python
def znajdz_pierwsze_w_zakresie(limit):
    pierwsze = [i for i in range(2, limit + 1) if czy_pierwsza_1(i)]
    print(f"Liczby pierwsze od 1 do {limit}:")
    print(pierwsze)

znajdz_pierwsze_w_zakresie(100)