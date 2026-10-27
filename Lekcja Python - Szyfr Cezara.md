# Materiał do Lekcji: Szyfr Cezara i Inne Szyfry Podstawieniowe w Pythonie

Na podstawie podręcznika (*Rozdział 8. Szyfr Cezara i inne szyfry podstawieniowe*).

---

## 1. Wprowadzenie do Kryptografii

* **Kryptografia** – dziedzina wiedzy o utajnianiu informacji.
* **Tekst jawny** – wiadomość w pierwotnej, czytelnej postaci.
* **Szyfrogram (kryptogram)** – wiadomość w postaci zaszyfrowanej.
* **Klucz** – informacja pozwalająca na zaszyfrowanie lub odszyfrowanie wiadomości (np. liczba pozycji przesunięcia w alfabecie).
* **Szyfr podstawieniowy** – szyfr, w którym każdy znak tekstu jawnego zamieniany jest na inny znak lub grupę znaków.

---

## 2. Szyfr Cezara – Zasada Działania

**Szyfr Cezara** to jeden z najstarszych szyfrów podstawieniowych. Polega na zastąpieniu każdej litery tekstu jawnego literą oddaloną od niej o stałą liczbę pozycji (klucz $k$) w alfabecie. Przesunięcie odbywa się cyklicznie (po `Z` następuje `A`).

Standardowy klucz stosowany przez Juliusza Cezara to $k = 3$:
* $A \to D$
* $B \to E$
* $Z \to C$

---

## 3. Ćwiczenie 1 (Rozwiązanie)

a) **Zaszyfruj wyraz `KRYPTOLOGIA` szyfrem Cezara z kluczem $k = 3$:**
* $K \to N$, $R \to U$, $Y \to B$, $P \to S$, $T \to W$, $O \to R$, $L \to O$, $O \to R$, $G \to J$, $I \to L$, $A \to D$
* **Szyfrogram:** `NUBSWOROJLD`

b) **Odszyfruj kryptogram `QVBMZVMB` zaszyfrowany z kluczem $k = 8$:**
* Aby odszyfrować, przesuwamy litery w lewo o $8$ pozycji (lub w prawo o $26 - 8 = 18$):
* $Q \to I$, $V \to N$, $B \to T$, $M \to E$, $Z \to R$, $V \to N$, $M \to E$, $B \to T$
* **Tekst jawny:** `INTERNET`

---

## 4. Szyfrowanie Wielkich Liter w Pythonie

### Sposób 1: Z wykorzystaniem instrukcji warunkowej (`if`)

Poniższa wersja sprawdza, czy po przesunięciu kod ASCII litery przekroczył wartość odpowiadającą literze `Z` (kod 90):

```python
def szyfruj_warunkowo(tekst_jawny: str, klucz: int) -> str:
    szyfrogram = ""
    for znak in tekst_jawny:
        kod = ord(znak) + klucz
        if kod > ord('Z'):
            kod -= 26
        szyfrogram += chr(kod)
    return szyfrogram
```

---

### Sposób 2: Z wykorzystaniem operacji modulo `% 26` (Wzór matematyczny)

Aby obliczyć przesunięcie cykliczne bez użycia `if`, sprowadzamy litery do zakresu $0 \dots 25$, wykonujemy dodanie klucza z dzieleniem resztowym `% 26`, a następnie wracamy do kodów ASCII:

$$\text{kod} = ((\text{kod\_ASCII}(\text{litera}) - \text{kod\_ASCII}('A') + \text{klucz}) \bmod 26) + \text{kod\_ASCII}('A')$$

```python
# Ćwiczenie 2: Implementacja wzoru matematycznego z modulo
def szyfruj_modulo(tekst_jawny: str, klucz: int) -> str:
    szyfrogram = ""
    for znak in tekst_jawny:
        kod = (ord(znak) - ord('A') + klucz) % 26 + ord('A')
        szyfrogram += chr(kod)
    return szyfrogram
```

---

## 5. Szyfrowanie Tekstu ze Spacjami i Małymi Literami (Ćwiczenie 3)

Gdy tekst zawiera wielkie i małe litery oraz inne znaki (spacje, znaki interpunkcyjne), szyfrujemy wyłącznie litery, pozostawiając pozostałe znaki bez zmian.

```python
def cezar(tekst: str, klucz: int) -> str:
    wynik = ""
    for znak in tekst:
        if 'A' <= znak <= 'Z':
            kod = (ord(znak) - ord('A') + klucz) % 26 + ord('A')
            wynik += chr(kod)
        elif 'a' <= znak <= 'z':
            kod = (ord(znak) - ord('a') + klucz) % 26 + ord('a')
            wynik += chr(kod)
        else:
            wynik += znak  # Znaki inne niż litery pozostają bez zmian
    return wynik
```

---

## 6. Obsługa Plików Tekstowych (Sekcja 8.2 & Ćwiczenie 4)

W języku Python odczyt i zapis do plików tekstowych wykonujemy najwygodniej za pomocą bloku `with open(...)`:

```python
def szyfruj_plik(nazwa_wejscia: str, nazwa_wyjscia: str, klucz: int):
    try:
        with open(nazwa_wejscia, 'r', encoding='utf-8') as f_in:
            tekst = f_in.read()
        
        zaszyfrowany_tekst = cezar(tekst, klucz)
        
        with open(nazwa_wyjscia, 'w', encoding='utf-8') as f_out:
            f_out.write(zaszyfrowany_tekst)
            
        print(f"Pomyślnie zaszyfrowano plik '{nazwa_wejscia}' i zapisano do '{nazwa_wyjscia}'.")
    except FileNotFoundError:
        print(f"Błąd: Nie znaleziono pliku '{nazwa_wejscia}'.")
```

---

## 7. Pełny Kod Programu Testującego

Poniżej znajduje się kompletna aplikacja łącząca wszystkie omówione zagadnienia:

```python
# --- FUNKCJE SZYFRUJĄCE ---

def cezar(tekst: str, klucz: int) -> str:
    """Szyfruje tekst z uwzględnieniem wielkich/małych liter oraz innych znaków."""
    wynik = ""
    klucz = klucz % 26  # Obsługa kluczy większych niż 26
    for znak in tekst:
        if 'A' <= znak <= 'Z':
            kod = (ord(znak) - ord('A') + klucz) % 26 + ord('A')
            wynik += chr(kod)
        elif 'a' <= znak <= 'z':
            kod = (ord(znak) - ord('a') + klucz) % 26 + ord('a')
            wynik += chr(kod)
        else:
            wynik += znak
    return wynik

def odszyfruj_cezar(tekst: str, klucz: int) -> str:
    """Odszyfrowanie polega na użyciu odwrotnego klucza (26 - klucz)."""
    return cezar(tekst, 26 - (klucz % 26))


# --- TESTOWANIE W PROGRAMIE GŁÓWNYM ---

if __name__ == "__main__":
    print("=== TEST 1: SZYFROWANIE POJEDYNCZYCH SŁÓW (Ćwiczenie 1) ===")
    slowo = "KRYPTOLOGIA"
    k = 3
    zaszyfrowane = cezar(slowo, k)
    print(f"Tekst: {slowo} | Klucz: {k} | Szyfrogram: {zaszyfrowane}")

    kryptogram = "QVBMZVMB"
    k2 = 8
    odszyfrowane = odszyfruj_cezar(kryptogram, k2)
    print(f"Kryptogram: {kryptogram} | Klucz: {k2} | Odszyfrowany tekst: {odszyfrowane}\n")

    print("=== TEST 2: TEKST ZE SPACJAMI I MAŁYMI LITERAMI (Ćwiczenie 3) ===")
    tekst_jawny = "Informatyka to przyszlosc! Klucz = 5."
    klucz_test = 5
    wynik_szyfrowania = cezar(tekst_jawny, klucz_test)
    print(f"Tekst jawny: {tekst_jawny}")
    print(f"Zaszyfrowany: {wynik_szyfrowania}\n")

    print("=== TEST 3: OPERACJE NA PLIKACH (Ćwiczenie 4) ===")
    # Utworzenie przykładowego pliku wejściowego
    with open("tekst_jawny.txt", "w", encoding="utf-8") as f:
        f.write("To jest przykladowy tekst w pliku.\nSzyfr Cezara dziala poprawnie!")

    # Szyfrowanie zawartości pliku
    klucz_plik = 7
    with open("tekst_jawny.txt", "r", encoding="utf-8") as f_in:
        zawartosc = f_in.read()

    zaszyfrowana_zawartosc = cezar(zawartosc, klucz_plik)

    with open("szyfrogram.txt", "w", encoding="utf-8") as f_out:
        f_out.write(zaszyfrowana_zawartosc)

    print(f"Plik 'tekst_jawny.txt' zostal zaszyfrowany z kluczem {klucz_plik} i zapisany w 'szyfrogram.txt'.")
```