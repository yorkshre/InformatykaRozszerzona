# Materiał do Lekcji: Reprezentacja i Operacje na Liczbach Wymiernych w Pythonie

Na podstawie podręcznika (*Rozdział 7. Algorytm Euklidesa i działania na ułamkach* / *Rozdział 1. Algorytmy na liczbach całkowitych i tekstach*).

---

## 1. Wprowadzenie i Reprezentacja Liczb Wymiernych

**Liczba wymierna** to liczba, którą można przedstawić w postaci ilorazu dwóch liczb całkowitych: $\frac{l}{m}$, gdzie:
* $l$ – licznik (liczba całkowita)
* $m$ – mianownik (liczba całkowita dodatnia, $m > 0$)

W pamięci komputera liczby wymierne najczęściej reprezentujemy jako **pary liczb całkowitych**. 
W językach takich jak C++ używa się do tego struktur (`struct`), natomiast w języku **Python** najczytelniejszym rozwiązaniem jest utworzenie własnej **klasy**.

### Definicja typu danych dla liczby wymiernej w Pythonie

```python
class Wymierna:
    def __init__(self, licz=0, mian=1):
        if mian == 0:
            raise ValueError("Mianownik nie może być równy 0!")
        
        # Zapewniamy, że mianownik jest zawsze dodatni, a ewentualny minus trafia do licznika
        if mian < 0:
            licz = -licz
            mian = -mian
            
        self.licz = licz
        self.mian = mian
```

---

## 2. Podstawowe Algorytmy Pomocnicze

Do wykonywania skracania ułamków oraz sprowadzania ich do wspólnego mianownika wykorzystujemy **Algorytm Euklidesa** do wyznaczania **Największego Wspólnego Dzielnika (NWD)** oraz **Najmniejszą Wspólną Wielokrotność (NWW)**.

### Algorytm Euklidesa (NWD)
```python
def nwd(a, b):
    while b != 0:
        a, b = b, a % b
    return a
```

### Ćwiczenie 5: Wyznaczanie NWW
Zależność między NWD i NWW dla liczb całkowitych dodatnich $a$ i $b$:
$$\text{NWW}(a, b) = \frac{a \cdot b}{\text{NWD}(a, b)}$$

```python
def nww(a, b):
    if a == 0 or b == 0:
        return 0
    return abs(a * b) // nwd(a, b)
```

---

## 3. Skracanie Ułamków (Ćwiczenie 4)

**Specyfikacja problemu:**
* **Dane:** Liczba wymierna w postaci $\frac{l}{m}$, gdzie $l, m \in \mathbb{C}, m > 0$.
* **Wynik:** Skrócony ułamek $\frac{p}{q}$, gdzie $p$ i $q$ są liczbami względnie pierwszymi ($\text{NWD}(p, q) = 1$).

Aby skrócić ułamek, dzielimy zarówno licznik, jak i mianownik przez ich $\text{NWD}$.

```python
def skroc(a: Wymierna) -> Wymierna:
    dzielnik = nwd(abs(a.licz), a.mian)
    a.licz //= dzielnik
    a.mian //= dzielnik
    return a
```

---

## 4. Dodawanie Liczb Wymiernych (Strona 112)

**Specyfikacja problemu:**
* **Dane:** Liczby wymierne $a = \frac{l_1}{m_1}$, $b = \frac{l_2}{m_2}$.
* **Wynik:** Liczba wymierna $c = a + b$.

### Zoptymalizowany algorytm dodawania bez skracania na końcu:
Zamiast mnożyć mianowniki bezpośrednio ($m_1 \cdot m_2$), co może prowadzić do operowania na bardzo dużych liczbach, stosujemy wzór optymalny:

1. $nwd_1 = \text{NWD}(m_1, m_2)$
2. $l = l_1 \cdot \frac{m_2}{nwd_1} + l_2 \cdot \frac{m_1}{nwd_1}$
3. $nwd_2 = \text{NWD}(|l|, nwd_1)$
4. Ostateczny wynik $\frac{p}{q}$:
   $$p = \frac{l}{nwd_2}$$
   $$q = \frac{m_1}{nwd_1} \cdot \frac{m_2}{nwd_2}$$

### Implementacja funkcji dodającej:
```python
def dodaj(a: Wymierna, b: Wymierna) -> Wymierna:
    c = Wymierna()
    
    nwd1 = nwd(a.mian, b.mian)
    c.licz = a.licz * (b.mian // nwd1) + b.licz * (a.mian // nwd1)
    
    nwd2 = nwd(abs(c.licz), nwd1)
    c.licz //= nwd2
    c.mian = (a.mian // nwd1) * (b.mian // nwd2)
    
    return c
```

---

## 5. Mnożenie Liczb Wymiernych (Ćwiczenie 7 / Strona 114)

Aby uniknąć przekroczenia zakresu liczb i zachować proste wartości, wykonuje się skracanie ułamków **"na krzyż"** przed wykonaniem właściwego mnożenia:

$$\frac{l_1}{m_1} \cdot \frac{l_2}{m_2} = \frac{\frac{l_1}{\text{NWD}(l_1, m_2)}}{\frac{m_1}{\text{NWD}(l_2, m_1)}} \cdot \frac{\frac{l_2}{\text{NWD}(l_2, m_1)}}{\frac{m_2}{\text{NWD}(l_1, m_2)}}$$

### Pseudokod (wg podręcznika):
```text
funkcja Mnoz(a, b)
    pom <- a.mian
    a.mian <- b.mian
    b.mian <- pom
    Skroc(a)
    Skroc(b)
    c.licz <- a.licz * b.licz
    c.mian <- a.mian * b.mian
    zwróć c i zakończ
```

### Implementacja w Pythonie (Ćwiczenie 7):
```python
def mnoz(a: Wymierna, b: Wymierna) -> Wymierna:
    # Tworzymy kopie obiektów, by nie zmieniać oryginalnych ułamków
    a_kopia = Wymierna(a.licz, a.mian)
    b_kopia = Wymierna(b.licz, b.mian)
    
    # Zamiana mianowników na krzyż
    a_kopia.mian, b_kopia.mian = b_kopia.mian, a_kopia.mian
    
    # Skracanie "na krzyż"
    skroc(a_kopia)
    skroc(b_kopia)
    
    # Wyznaczenie wyniku
    c = Wymierna(a_kopia.licz * b_kopia.licz, a_kopia.mian * b_kopia.mian)
    return skroc(c)
```

---

## 6. Wypisywanie Ułamka (`PiszUlamek`)

Zgodnie z zasadami podanymi w podręczniku, jeśli ułamek jest ujemny, reprezentujemy go na ekranie w nawiasach okrągłych, np. `(-3/20)`.

```python
def pisz_ulamek(a: Wymierna):
    if a.licz < 0:
        print(f"({a.licz}/{a.mian})", end="")
    else:
        print(f"{a.licz}/{a.mian}", end="")
```

---

## 7. Pełny Kod Programu Testującego

Poniżej znajduje się kompletny program łączący wszystkie omówione elementy:

```python
# --- DEFINICJE KLAS I FUNKCJI ---

class Wymierna:
    def __init__(self, licz=0, mian=1):
        if mian == 0:
            raise ValueError("Mianownik nie może być zerem!")
        if mian < 0:
            licz = -licz
            mian = -mian
        self.licz = licz
        self.mian = mian

def nwd(a, b):
    while b != 0:
        a, b = b, a % b
    return a

def nww(a, b):
    if a == 0 or b == 0:
        return 0
    return abs(a * b) // nwd(a, b)

def skroc(a: Wymierna) -> Wymierna:
    dzielnik = nwd(abs(a.licz), a.mian)
    a.licz //= dzielnik
    a.mian //= dzielnik
    return a

def dodaj(a: Wymierna, b: Wymierna) -> Wymierna:
    c = Wymierna()
    nwd1 = nwd(a.mian, b.mian)
    c.licz = a.licz * (b.mian // nwd1) + b.licz * (a.mian // nwd1)
    nwd2 = nwd(abs(c.licz), nwd1)
    c.licz //= nwd2
    c.mian = (a.mian // nwd1) * (b.mian // nwd2)
    return c

def mnoz(a: Wymierna, b: Wymierna) -> Wymierna:
    a_kopia = Wymierna(a.licz, a.mian)
    b_kopia = Wymierna(b.licz, b.mian)
    
    a_kopia.mian, b_kopia.mian = b_kopia.mian, a_kopia.mian
    
    skroc(a_kopia)
    skroc(b_kopia)
    
    c = Wymierna(a_kopia.licz * b_kopia.licz, a_kopia.mian * b_kopia.mian)
    return skroc(c)

def pisz_ulamek(a: Wymierna):
    if a.licz < 0:
        print(f"({a.licz}/{a.mian})", end="")
    else:
        print(f"{a.licz}/{a.mian}", end="")


# --- TESTOWANIE PROGRAMU (MAIN) ---

if __name__ == "__main__":
    print("=== TEST DODAWANIA (Ćwiczenie 6 / przykład ze strony 113) ===")
    # Przykładowe ułamki: 5/12 oraz -3/20
    u1 = Wymierna(5, 12)
    u2 = Wymierna(-3, 20)
    
    # Skracanie wstępne ułamków (opcjonalne)
    skroc(u1)
    skroc(u2)
    
    wynik_dodawania = dodaj(u1, u2)
    
    pisz_ulamek(u1)
    print(" + ", end="")
    pisz_ulamek(u2)
    print(" = ", end="")
    pisz_ulamek(wynik_dodawania)
    print("\n")  # Wynik: 5/12 + (-3/20) = 4/15

    print("=== TEST MNOŻENIA (Ćwiczenie 7 / przykład ze strony 114) ===")
    # Przykładowe ułamki: 3/5 oraz 10/21
    u3 = Wymierna(3, 5)
    u4 = Wymierna(10, 21)
    
    wynik_mnozenia = mnoz(u3, u4)
    
    pisz_ulamek(u3)
    print(" * ", end="")
    pisz_ulamek(u4)
    print(" = ", end="")
    pisz_ulamek(wynik_mnozenia)
    print()  # Wynik: 3/5 * 10/21 = 2/7
```