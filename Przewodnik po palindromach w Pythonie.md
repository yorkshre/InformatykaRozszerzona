# Przewodnik po palindromach i algorytmach tekstowych w Pythonie

W tym przewodniku rozwiniemy zagadnienie **palindromów** oraz algorytmów operowania na ciągach znaków (stringach), tłumacząc koncepcje z podręcznika informatyki na język Python.

---

## 1. Czym jest palindrom i jak działa algorytm?

**Palindrom** to słowo, zdanie lub liczba, która czytana od lewej do prawej i od prawej do lewej brzmi dokładnie tak samo. Przykłady:
* Słowa: *kajak*, *radar*, *potop*, *anna*
* Zdania (po usunięciu spacji): *„Kobyła ma mały bok”*

### Jak działa algorytm porównywania par liter?
Algorytm iteracyjny polega na użyciu dwóch wskaźników (indeksów):
1. **$i$** – wskazuje początek słowa (zaczyna od $0$).
2. **$j$** – wskazuje koniec słowa (zaczyna od `długość - 1`).

W pętli porównujemy znak `wyraz[i]` ze znakiem `wyraz[j]`. Jeśli są takie same, zbliżamy się do środka ($i$ rośnie o 1, $j$ maleje o 1). Jeśli natrafimy na różnicę, przerywamy pętlę – wyraz nie jest palindromem.

---

## 2. Implementacja w Pythonie: Od pojedynczego słowa do całego zdania

Poniższy skrypt zawiera pełne, skomentowane implementacje obu algorytmów (sprawdzanie wyrazu oraz wyszukiwanie palindromów w zdaniu).

```python
def czy_palindrom(wyraz):
    """
    Sprawdza, czy podany wyraz jest palindromem.
    Ignoruje wielkość liter (odpowiednik funkcji toupper z C++).
    """
    i = 0
    j = len(wyraz) - 1
    p = True  # Zmienna logiczna (flaga)
    
    while p and (i < j):
        # Porównujemy wielkie litery, aby kod był odporny na np. "Kajak"
        if wyraz[i].upper() == wyraz[j].upper():
            i += 1
            j -= 1
        else:
            p = False
            
    return p

def znajdz_palindromy_w_zdaniu(zdanie):
    """
    Wyszukuje słowa będące palindromami w podanym zdaniu,
    wykorzystując spację jako separator (wartownik).
    """
    # Dodajemy spację na końcu, aby algorytm poprawnie obsłużył ostatnie słowo
    zdanie_z_wartownikiem = zdanie + " "
    wyniki = []
    
    while len(zdanie_z_wartownikiem) > 0:
        # Szukamy pierwszego wystąpienia spacji (odpowiednik find w C++)
        i = zdanie_z_wartownikiem.find(' ')
        
        if i > 0:
            # Wycinamy słowo od początku do pierwszej spacji (odpowiednik substr)
            wyraz = zdanie_z_wartownikiem[:i]
            
            # Sprawdzamy czy wyraz jest palindromem
            if czy_palindrom(wyraz):
                wyniki.append(wyraz)
                
            # Usuwamy sprawdzone słowo wraz ze spacją ze zdania (odpowiednik erase)
            zdanie_z_wartownikiem = zdanie_z_wartownikiem[i + 1:]
        else:
            break
            
    return wyniki

# --- Przykłady użycia ---
if __name__ == "__main__":
    # Test 1: Pojedyncze słowa
    slowo_testowe = "Kajak"
    if czy_palindrom(slowo_testowe):
        print(f"Słowo '{slowo_testowe}' to palindrom: TAK")
    else:
        print(f"Słowo '{slowo_testowe}' to palindrom: NIE")

    # Test 2: Wyszukiwanie w zdaniu
    zdanie_testowe = "potop i kajak to super ród"
    znalezione = znajdz_palindromy_w_zdaniu(zdanie_testowe)
    print(f"Znalezione palindromy w zdaniu '{zdanie_testowe}':", znalezione)
```

---

## 3. Pythonowy "Pythonic Way" (Sztuczki i skróty)

Python posiada potężne mechanizmy operowania na napisach (**slicing**), dzięki którym algorytmy sprawdzania palindromów można zapisać w jednej linijce kodu bez pisania jawnych pętli `while`:

```python
def czy_palindrom_szybki(wyraz):
    # Wyczyszczenie z wielkości liter
    w = wyraz.upper()
    # [::-1] oznacza odwrócenie napisu od końca do początku
    return w == w[::-1]

# Przykłady:
print(czy_palindrom_szybki("radar"))  # Zwraca: True
print(czy_palindrom_szybki("Python")) # Zwraca: False
```

### Porównanie C++ i Pythona:
* **Wielkość liter:** W C++ musieliśmy ręcznie wywoływać funkcję `toupper()` na pojedynczych znakach z biblioteki `<cctype>`. W Pythonie wystarczy przekształcić cały napis metodą `.upper()`.
* **Zarządzanie pamięcią:** W C++ musieliśmy ręcznie operować na indeksach i metodach typu `substr` / `erase`. W Pythonie napisy są niemutowalne, a operacje cięcia (`[start:stop]`) są realizowane automatycznie i bezpiecznie przez interpreter.