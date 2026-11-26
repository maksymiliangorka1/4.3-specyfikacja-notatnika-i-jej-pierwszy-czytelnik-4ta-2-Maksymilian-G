# Specyfikacja funkcjonalna: Notatnik (wersja 001)

## 1. Cel

Celem programu jest umożliwienie pojedynczemu użytkownikowi szybkiego, wygodnego i niezawodnego tworzenia, trwałego zapisywania oraz przeglądania osobistych notatek tekstowych na lokalnym komputerze, w pełnej prywatności i bez konieczności połączenia z jakąkolwiek siecią komputerową.

Aplikacja gwarantuje, że notatki są przechowywane w sposób otwarty, trwały i czytelny dla człowieka, z pełnym wsparciem dla języka polskiego i znaków diakrytycznych.

---

## 2. Dla kogo

Program jest przeznaczony dla:
- Pojedynczego użytkownika pracującego na własnym komputerze osobistym.
- Osób ceniących pełną suwerenność swoich danych, prywatność oraz brak uzależnienia od usług chmurowych i kont internetowych.
- Użytkowników posługujących się językiem polskim, wymagających bezbłędnej obsługi polskich znaków diakrytycznych w interfejsie oraz w zapisywanych tekstach.
- Osób, które chcą mieć dostęp do swoich zapisków także poza dedykowaną aplikacją, przy użyciu dowolnego zwykłego edytora tekstu.

---

## 3. Co robi

1. **Tworzenie nowej notatki**:
   - Udostępnia formularz z osobnym, jednowierszowym polem na tytuł notatki oraz wielowierszowym polem na jej treść.
   - Umożliwia swobodne wprowadzanie tekstu w języku polskim ze wszystkimi znakami diakrytycznymi (`ą`, `ć`, `ę`, `ł`, `ń`, `ó`, `ś`, `ź`, `ż` oraz ich wielkimi odpowiednikami).
   - Dopuszcza zapis notatki z pustą treścią, o ile pole tytułu zostało poprawnie wypełnione (krótka notatka-przypomnienie).

2. **Zapisywanie notatek i kontrakt formatu danych**:
   - Wszystkie notatki są zapisywane do jednego, lokalnego pliku tekstowego w czytelnym formacie Markdown z kodowaniem UTF-8 (zgodnie z decyzją architektoniczną ADR 0001).
   - Każdy wpis w pliku posiada ustandaryzowaną strukturę: nagłówek Markdown drugiego stopnia z datą i tytułem (`## YYYY-MM-DD HH:mm — Tytuł`), po którym następuje pusta linia i treść notatki.
   - Zapewnia bezpieczny zapis danych (zapis atomowy chroniący przed uszkodzeniem lub wyzerowaniem dotychczasowej bazy notatek w przypadku błędu dyskowego lub awarii zasilania).
   - Bezpośrednio po udanym zapisie formularz dodawania notatki zostaje wyczyszczony (pola tytułu i treści stają się puste), kursor automatycznie powraca do pola tytułu, a przycisk zapisu wraca do stanu nieaktywnego.

3. **Prezentacja listy notatek**:
   - Wyświetla listę wszystkich zapisanych notatek użytkownika w lewym panelu okna głównego programu.
   - Prezentuje notatki w kolejności chronologicznej malejącej: najnowsza notatka znajduje się na samej górze listy.
   - Każda pozycja na liście prezentuje tytuł notatki oraz sformatowaną datę i godzinę jej utworzenia.
   - Lista jest natychmiast aktualizowana po zapisaniu nowej notatki, bez konieczności przeładowywania programu.

4. **Przeglądanie i podgląd wybranej notatki**:
   - Umożliwia wskazanie (kliknięcie) notatki na liście w celu wyświetlenia jej pełnego tytułu, daty oraz treści w dedykowanym panelu podglądu.
   - Kliknięcie notatki na liście nie czyści ani nie niszczy tekstu aktualnie wprowadzanego przez użytkownika w formularzu nowej notatki (niezależne komponenty widoku).
   - W stanie początkowym (przed wybraniem jakiejkolwiek notatki lub przy pustej liście) panel podglądu wyświetla komunikat informacyjny: *„Wybierz notatkę z listy, aby wyświetlić jej treść”*.

5. **Walidacja danych wejściowych**:
   - Blokuje możliwość zapisu notatki, jeśli pole tytułu jest puste lub zawiera wyłącznie białe znaki (spacje, tabulatory). W takim stanie przycisk zapisu jest wizualnie wyszarzony i nie reaguje na próby kliknięcia.

6. **Polska warstwa językowa i obsługa błędów**:
   - Wszystkie elementy interfejsu (etykiety pól, przyciski, nagłówki kolumn, teksty podpowiedzi) są wyświetlane w języku polskim.
   - W przypadku wystąpienia błędu odczytu lub zapisu (np. brak uprawnień do pliku) program nie przerywa nagle swojego działania, lecz prezentuje czytelny komunikat informujący o naturze problemu w języku polskim w dedykowanym pasku informacyjnym okna.

7. **Trwałość i szybki start**:
   - Po ponownym uruchomieniu programu aplikacja otwiera się w czasie poniżej 2 sekund i automatycznie ładuje wszystkie wcześniej zapisane notatki z lokalnego pliku.

---

## 4. Czego nie robi

1. **Brak łączności sieciowej i chmury**:
   - Program nie łączy się z internetem, nie synchronizuje notatek z serwerami zdalnymi, nie wysyła telemetrii ani statystyk użytkowania.
2. **Brak wielu użytkowników i logowania**:
   - Program nie posiada profili użytkowników, kont, haseł, ekranu logowania ani podziału na role.
3. **Brak zamkniętych i binarnych formatów**:
   - Program nie zapisuje notatek w bazach danych ani plikach binarnych niedających się otworzyć w zwykłym edytorze tekstu.
4. **Brak wyszukiwania i filtrowania (w wersji 001)**:
   - Program w bieżącej wersji nie zawiera paska wyszukiwania ani zaawansowanego filtrowania notatek (zakres świadomie odłożony w decyzjach projektowych).
5. **Brak edycji i usuwania istniejących notatek z poziomu UI (w wersji 001)**:
   - Program w wersji 001 koncentruje się na niezawodnym dodawaniu i odczycie notatek; modyfikacje i usuwanie wcześniejszych wpisów użytkownik wykonuje bezpośrednio w pliku tekstowym.
6. **Brak załączników i multimediów**:
   - Program nie obsługuje dodawania obrazów, plików dźwiękowych ani załączników binarnych do notatek.

---

## 5. Kryteria akceptacji

### Kryterium 1: Obsługa polskich znaków
- **Sytuacja początkowa**: Program jest uruchomiony na komputerze użytkownika, otwarty jest formularz tworzenia nowej notatki.
- **Działanie użytkownika**: Użytkownik wprowadza w polu tytułu tekst zawierający wszystkie polskie znaki diakrytyczne: `Zażółć gęślą jaźń ZAŻÓŁĆ GĘŚLĄ JAŹŃ`, a w polu treści zdanie: `Pchnąć w tę łódź jeża lub ośm skrzyń fig`, po czym klika aktywny przycisk zapisu notatki.
- **Oczekiwany rezultat**: Kiedy użytkownik zapisze notatkę z polskimi znakami, to na liście notatek, w panelu podglądu oraz w docelowym pliku tekstowym na dysku wszystkie znaki diakrytyczne są zachowane w kodowaniu UTF-8 i prezentowane bez zniekształceń, zamiany na encje, znaków zapytania czy błędnych symboli.
- **Sprawdzenie**: 
  1. Sprawdzić wzrokowo w oknie aplikacji, czy na liście notatek i w podglądzie tytuł i treść wyświetlają się poprawnie z polskimi literami.
  2. Otworzyć plik danych w zewnętrznym edytorze tekstu (np. Notatnik Windows) z wybranym kodowaniem UTF-8 i upewnić się, że zapisany tekst jest w 100% czytelny i identyczny z wprowadzonym.

### Kryterium 2: Aktualność listy notatek
- **Sytuacja początkowa**: Program jest uruchomiony, na liście notatek wyświetlane są dotychczasowe wpisy (lub lista jest pusta przy pierwszym uruchomieniu).
- **Działanie użytkownika**: Użytkownik wpisuje w formularzu unikalny tytuł `Notatka z godziny 12:00` oraz dowolną treść, a następnie klika przycisk zapisu.
- **Oczekiwany rezultat**: Kiedy użytkownik zatwierdzi zapis notatki, to nowa notatka pojawia się natychmiast na samej górze listy notatek jako pierwsza pozycja, z właściwym tytułem i bieżącą datą, formularz dodawania zostaje wyczyszczony, kursor powraca do pola tytułu, a aktualizacja następuje bez konieczności ponownego uruchamiania programu ani manualnego odświeżania widoku.
- **Sprawdzenie**: 
  1. Zaobserwować stan listy bezpośrednio po kliknięciu przycisku zapisu – liczba elementów na liście musi wzrosnąć dokładnie o jeden.
  2. Sprawdzić, czy pierwszy element od góry listy posiada tytuł `Notatka z godziny 12:00`.
  3. Sprawdzić, czy pola tytułu i treści w formularzu stały się puste.

### Kryterium 3: Bezpieczeństwo zapisu
- **Sytuacja początkowa**: Plik notatek na dysku zawiera już wcześniej zapisane notatki (np. 3 wpisy). Użytkownik przygotował nową, czwartą notatkę w formularzu.
- **Działanie użytkownika**: Użytkownik klika przycisk zapisu notatki.
- **Oczekiwany rezultat**: Kiedy następuje operacja zapisu, to program wykonuje ją w sposób chroniący integralność bazy danych – dotychczasowe 3 notatki pozostają w pliku nienaruszone, a nowy wpis zostaje dodany zgodnie z przyjętym formatem. W przypadku wystąpienia błędu dyskowego (np. brak praw do zapisu) program nie ulega nagłemu zamknięciu, wyświetla czytelny komunikat po polsku w oknie, a dotychczasowy plik nie zostaje wyczyszczony ani uszkodzony.
- **Sprawdzenie**: 
  1. Przed zapisem wykonać kopię pliku z 3 notatkami.
  2. Zapisać czwartą notatkę i sprawdzić, czy plik zawiera komplet 4 notatek.
  3. Przetestować sytuację awaryjną (plik oznaczony atrybutem „Tylko do odczytu”): sprawdzić, czy aplikacja wyświetla komunikat o błędzie po polsku i nie pozostawia pustego pliku bez zawartości.

### Kryterium 4: Blokada zapisu pustej notatki
- **Sytuacja początkowa**: Program jest uruchomiony, pole tytułu w formularzu nowej notatki jest puste (lub zawiera wyłącznie spacje/tabulatory), a pole treści zawiera dowolny tekst lub jest puste.
- **Działanie użytkownika**: Użytkownik spogląda na przycisk zapisu notatki lub próbuje go kliknąć.
- **Oczekiwany rezultat**: Kiedy pole tytułu jest puste lub zawiera wyłącznie białe znaki, to przycisk zapisu jest wizualnie wyszarzony i nieaktywny, kliknięcie w niego nie wywołuje żadnej akcji, plik na dysku nie ulega modyfikacji, a na liście notatek nie pojawia się żaden nowy element.
- **Sprawdzenie**: 
  1. Pozostawić pole tytułu puste, wpisać treść `Przykładowy tekst` i sprawdzić stan przycisku zapisu – zweryfikować brak reakcji przycisku na kliknięcie oraz jego wyszarzony wygląd.
  2. Wpisać do pola tytułu ciąg spacji `   ` – zweryfikować, czy przycisk nadal pozostaje nieaktywny.
  3. Wpisać co najmniej jeden widoczny znak w tytule (np. literę `A`) – upewnić się, że przycisk natychmiast staje się aktywny.

### Kryterium 5: Działanie po ponownym uruchomieniu
- **Sytuacja początkowa**: Użytkownik dodał i zapisał w programie dwie notatki testowe, po czym zamknął program.
- **Działanie użytkownika**: Użytkownik ponownie uruchamia aplikację ze skrótu lub pliku wykonywalnego.
- **Oczekiwany rezultat**: Kiedy aplikacja zostaje ponownie uruchomiona, to okno główne pojawia się w czasie poniżej 2 sekund, wszystkie wcześniej zapisane notatki są automatycznie odczytane z lokalnego pliku i wyświetlone na liście w kolejności od najnowszej, a kliknięcie na notatkę prezentuje jej pełną, nienaruszoną treść.
- **Sprawdzenie**: 
  1. Zmierzyć stoperem czas od momentu wywołania programu do pełnego załadowania interfejsu (musi być < 2,0 s).
  2. Sprawdzić, czy obie wcześniej dodane notatki są widoczne na liście z poprawnymi tytułami i datami.
  3. Wybrać każdą z notatek i upewnić się, że treść w panelu podglądu jest w 100% zgodna z zapisaną przed zamknięciem.

### Kryterium 6: Podgląd wybranej notatki i ochrona szkicu
- **Sytuacja początkowa**: Na liście notatek znajdują się co najmniej dwie różne notatki. W formularzu tworzenia notatki użytkownik wpisał roboczy tytuł `Mój szkic`.
- **Działanie użytkownika**: Użytkownik klika lewym przyciskiem myszy na wybraną pozycję na liście notatek.
- **Oczekiwany rezultat**: Kiedy użytkownik wskaże notatkę na liście, to w panelu podglądu natychmiast pojawia się pełny tytuł, data utworzenia oraz pełna treść wybranej notatki, a roboczy tekst `Mój szkic` w formularzu tworzenia nowej notatki pozostaje nienaruszony.
- **Sprawdzenie**: Wpisać tekst do formularza, kliknąć notatkę na liście i sprawdzić: (a) czy panel podglądu zaktualizował się do wybranej notatki, (b) czy formularz nadal zawiera wpisany wcześniej roboczy tekst.

---

## 6. Do ustalenia

W toku analizy wymagań oraz po niezależnym przeglądzie Pierwszego Czytelnika wyodrębniono następujące kwestie otwarte:

1. **[DO USTALENIA]: Domyślna lokalizacja pliku danych notatek**:
   - *Wariant A*: Plik danych umieszczony w bieżącym katalogu roboczym obok programu (maksymalna przenośność, łatwy dostęp).
   - *Wariant B*: Plik danych w standardowym folderze `Dokumenty` bieżącego użytkownika systemu operacyjnego (bezpieczeństwo przed skasowaniem przy aktualizacji programu).
2. **[DO USTALENIA]: Sposób prezentacji treści w panelu podglądu**:
   - *Wariant A*: Renderowanie wizualne znaczników Markdown (nagłówki, listy, pogrubienia) w postaci sformatowanej.
   - *Wariant B*: Prezentacja czystego tekstu Markdown dokładnie tak, jak został wpisany (maksymalna prostota i transparentność).
3. **[DO USTALENIA]: Zachowanie w przypadku zewnętrznej modyfikacji pliku danych**:
   - *Wariant A*: Odczyt pliku następuje wyłącznie podczas uruchamiania programu oraz po operacji zapisu z poziomu aplikacji.
   - *Wariant B*: Aplikacja w tle automatycznie wykrywa zmiany wprowadzone w pliku z zewnątrz i odświeża listę, jeśli plik został zmodyfikowany przez inny edytor tekstu.
4. **[DO USTALENIA]: Obsługa skrótów klawiszowych**:
   - Czy program ma obsługiwać globalny skrót zapisu (`Ctrl+S`) oraz przejście klawiszem Tab / Enter między polami formularza?
5. **[DO USTALENIA]: Limit długości tytułu notatki**:
   - Czy pole tytułu powinno posiadać sztywny limit znaków (np. 150 znaków), aby zapobiec zaburzeniu czytelności elementów listy?
