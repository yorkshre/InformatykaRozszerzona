## Wyszukiwanie w tekście i obsługa plików ##
Do pracy z plikami w Pythonie używamy funkcji `open`, która działa wraz z konstrukcją `with`, która zamyka plik po zakończeniu pracy:

```python
with open('dane.txt', 'r', encoding='utf-8') as plik:
    zawartosc = plik.read()
    print(zawartosc)
```
Tryb `'r'` oznacza odczyt. Parametr `encoding='utf-8'` jest konieczny, jeśli pracujesz z polskimi znakami lub plikami tekstowymi o niestandardowym kodowaniu.
Tryby otwierania plików w Pythonie (`open()`)

## 1. Podstawowe tryby tekstowe

| Tryb | Nazwa | Opis działania | Tworzy plik? | Czyszczenie pliku? |
| :--- | :--- | :--- | :---: | :---: |
| `'r'` | **Odczyt** *(Read)* | Otwiera plik tylko do odczytu. Plik musi istnieć (w przeciwnym razie zgłosi błąd `FileNotFoundError`). | Nie | Nie |
| `'w'` | **Zapis** *(Write)* | Otwiera plik tylko do zapisu. Jeśli plik istnieje, jego **zawartość zostanie usunięta**. Jeśli nie istnieje – tworzy nowy. | **Tak** | **Tak** |
| `'a'` | **Dopisywanie** *(Append)* | Otwiera plik do dopisywania na końcu. Jeśli plik nie istnieje – tworzy go. Nie usuwa starej zawartości. | **Tak** | Nie |
| `'x'` | **Wyłączny zapis** *(Exclusive creation)* | Tworzy nowy plik i otwiera go do zapisu. Zgłasza błąd `FileExistsError`, jeśli plik już istnieje. | **Tak** | Nie |
| `'r+'` | **Odczyt i zapis** | Otwiera istniejący plik do odczytu i zapisu. Kursor ustawiany jest na początku, nadpisuje istniejące znaki. | Nie | Nie |
| `'w+'` | **Zapis i odczyt** | Otwiera plik do zapisu i odczytu. **Czyści zawartość pliku** przy otwarciu! Tworzy plik, jeśli nie istnieje. | **Tak** | **Tak** |
| `'a+'` | **Dopisywanie i odczyt** | Otwiera plik do dopisywania i odczytu. Zapis zawsze trafia na koniec pliku. Tworzy plik, jeśli nie istnieje. | **Tak** | Nie |

---

## 2. Tryby binarne (dla zdjęć, PDF-ów, wideo itp.)

Dopisanie litery **`b`** przełącza operacje na bajty zamiast tekstu:

| Tryb | Nazwa | Zastosowanie / Opis |
| :--- | :--- | :--- |
| `'rb'` | **Odczyt binarny** | Odczyt plików binarnych (obrazy, dokumenty PDF, archiwum ZIP). |
| `'wb'` | **Zapis binarny** | Zapis danych binarnych (np. pobrany obrazek z sieci). Nadpisuje plik! |
| `'ab'` | **Dopisywanie binarne** | Dopisywanie bajtów na końcu pliku binarnego. |
| `'rb+'` | **Odczyt i zapis binarny** | Zaawansowana edycja i odczyt struktury binarnej bez czyszczenia pliku. |

---

## 3. Modyfikatory trybów

* **`+`** – Dodaje możliwość odwrotną (np. do odczytu dodaje zapis, do zapisu dodaje odczyt).
* **`b`** – Włącza tryb **binarny** *(Binary)* zamiast domyślnego tekstowego.
* **`t`** – Włącza tryb **tekstowy** (jest to wartość domyślna, np. `'r'` to to samo co `'rt'`).


---
### Zadanie 1 ###
W pliku slowa.txt zapisano 1000 wierszy. Każdy z nich zawiera dwa niepuste słowa oddzielone spacją. Słowa składają się wyłącznie z wielkich liter
alfabetu angielskiego*

**Przykład**:

```text
AAIWQX EZSLCL
ACTOACTAOER OACTA
ACUO KORN

```
Podaj, ile słów w pliku slowa.txt kończy się na literę A.

### Zadanie 2 ###
Podaj liczbę wierszy z pliku slowa.txt zawierających pary słów, w których pierwsze słowo zawiera się w drugim słowie.
*Przykład:*
Słowo ADC zawiera się w słowie ASWADCF, jak też w słowie ADC. Słowo ADC nie zawiera się w słowie ASWADFC

### Zadanie 3 ###

Anagram to słowo powstałe z przestawienia liter danego słowa, wykorzystujące wszystkie jego litery.
*Przykład:*
Anagramami słowa ```txt SLOMA``` są na przykład: ```txtMASLO, SLMAO, SOLMA,``` …
Podaj liczbę wierszy w pliku slowa.txt, w których występują pary słów
takich, że pierwsze słowo jest anagramem drugiego. Wypisz te pary.
Są to zadania maturalne z różnych lat.
Aby pokazać, jak diametralnie różny jest poziom trudności zadań. 
W zadaniu 1 wystarczy sprawdzić ostatnią literę każdego ze słów. W Pythonie zrobimy to, umieszczając na końcu sprawdzanego słowa.

W zadaniu 2 wystarczy sprawdzić, czy jedno słowo mieści się w drugim
— rozwiążemy to operatorem in.
W zadaniu 3 natomiast należy zauważyć, że aby jedno słowo było anagramem drugiego, musi się składać z tych samych liter. Wystarczy więc posortować oba wyrazy i sprawdzić, czy są identyczne. 
Można rozwiązanie wykonać w listach składanych:
``` python
with open("slowa.txt") as plik:
slowa = [i.split() for i in plik]
literaA = [i[0] for i in slowa if i[0][-1] == "A"]
literaA += [i[1] for i in slowa if i[1][-1] == "A"]
zawiera = [i[0] for i in slowa if i[0] in i[1]]
anagram = [i[0] for i in slowa if sorted(i[0]) == sorted(i[1])]
print(len(literaA), len(zawiera), len(anagram))
```

Jak widać, całość zajmuje mniej wierszy niż niejedno pojedyncze zadanie z innych matur. Wiersz zawierający metodę .split() jednocześnie wczytuje
dane z pliku i rozdziela wyrazy z jednego wiersza na dwa oddzielne elementy.


*Źródło:  Ronald Zimek - "Python na maturze"*
