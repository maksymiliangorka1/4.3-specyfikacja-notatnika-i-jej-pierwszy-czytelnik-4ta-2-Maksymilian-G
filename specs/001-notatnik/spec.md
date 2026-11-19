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
   - Udostępnia formularz z osobnym polem na tytuł notatki oraz osobnym polem na jej treść.
   - Umożliwia swobodne wprowadzanie tekstu w języku polskim ze wszystkimi znakami diakrytycznymi (`ą`, `ć`, `ę`, `ł`, `ń`, `ó`, `ś`, `ź`, `ż` oraz ich wielkimi odpowiednikami).

2. **Zapisywanie notatek**:
   - Zapisuje notatkę do lokalnego pliku tekstowego w czytelnym formacie Markdown z zachowaniem kodowania UTF-8.
   - Do każdej zapisywanej notatki automatycznie przypisuje aktualną datę i godzinę jej utworzenia w formacie `YYYY-MM-DD HH:mm`.
   - Zapewnia bezpieczny zapis danych chroniący przed uszkodzeniem lub wyzerowaniem dotychczasowej bazy notatek.

3. **Prezentacja listy notatek**:
   - Wyświetla listę wszystkich zapisanych notatek użytkownika w oknie głównym programu.
   - Prezentuje notatki w kolejności chronologicznej malejącej: najnowsza notatka znajduje się na samej górze listy.
   - Każda pozycja na liście prezentuje co najmniej tytuł notatki oraz datę jej utworzenia.
   - Lista jest natychmiast aktualizowana po zapisaniu nowej notatki, bez konieczności przeładowywania programu.

4. **Przeglądanie i podgląd wybranej notatki**:
   - Umożliwia wskazanie (kliknięcie) notatki na liście w celu wyświetlenia jej pełnego tytułu, daty oraz treści w dedykowanym panelu podglądu.

5. **Walidacja danych wejściowych**:
   - Blokuje możliwość zapisu notatki, jeśli pole tytułu jest puste lub zawiera wyłącznie białe znaki (spacje, tabulatory). W takim stanie przycisk zapisu jest nieaktywny.

6. **Polska warstwa językowa i obsługa błędów**:
   - Wszystkie elementy interfejsu (etykiety pól, przyciski, nagłówki kolumn) oraz komunikaty o błędach i ostrzeżenia są wyświetlane w języku polskim.

7. **Trwałość i szybki start**:
   - Po ponownym uruchomieniu programu aplikacja otwiera się w czasie poniżej 2 sekund i automatycznie ładuje wszystkie wcześniej zapisane notatki.

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
- **Sytuacja początkowa**: Program jest uruchomiony na komputerze użytkownika, otwarty jest pusty formularz tworzenia nowej notatki.
- **Działanie użytkownika**: Użytkownik wprowadza w polu tytułu tekst zawierający wszystkie polskie znaki diakrytyczne: `Zażółć gęślą jaźń ZAŻÓŁĆ GĘŚLĄ JAŹŃ`, a w polu treści zdanie: `Pchnąć w tę łódź jeża lub ośm skrzyń fig`, po czym klika aktywny przycisk zapisu notatki.
- **Oczekiwany rezultat**: Kiedy użytkownik zapisze notatkę z polskimi znakami, to na liście notatek, w panelu podglądu oraz w docelowym pliku tekstowym na dysku wszystkie znaki diakrytyczne są zachowane w kodowaniu UTF-8 i prezentowane bez zniekształceń, znaków zapytania czy błędnych symboli.
- **Sprawdzenie**: 
  1. Sprawdzić wzrokowo w oknie aplikacji, czy na liście notatek i w podglądzie tytuł i treść wyświetlają się poprawnie z polskimi literami.
  2. Otworzyć plik danych w zewnętrznym edytorze tekstu (np. Notatnik Windows) z wybranym kodowaniem UTF-8 i upewnić się, że zapisany tekst jest w 100% czytelny i identyczny z wprowadzonym.

### Kryterium 2: Aktualność listy notatek
- **Sytuacja początkowa**: Program jest uruchomiony, na liście notatek wyświetlane są dotychczasowe wpisy (lub lista jest pusta przy pierwszym uruchomieniu).
- **Działanie użytkownika**: Użytkownik wpisuje w formularzu unikalny tytuł `Notatka z godziny 12:00` oraz dowolną treść, a następnie klika przycisk zapisu.
- **Oczekiwany rezultat**: Kiedy użytkownik zatwierdzi zapis notatki, to nowa notatka pojawia się natychmiast na samej górze listy notatek jako pierwsza pozycja, z właściwym tytułem i bieżącą datą, bez konieczności ponownego uruchamiania programu ani ręcznego odświeżania widoku.
- **Sprawdzenie**: 
  1. Zaobserwować stan listy bezpośrednio po kliknięciu przycisku zapisu – liczba elementów na liście musi wzrosnąć dokładnie o jeden.
  2. Sprawdzić, czy pierwszy element od góry listy posiada tytuł `Notatka z godziny 12:00`.

### Kryterium 3: Bezpieczeństwo zapisu
- **Sytuacja początkowa**: Plik notatek na dysku zawiera już wcześniej zapisane notatki (np. 3 wpisy). Użytkownik przygotował nową, czwartą notatkę w formularzu.
- **Działanie użytkownika**: Użytkownik klika przycisk zapisu notatki.
- **Oczekiwany rezultat**: Kiedy następuje operacja zapisu, to program wykonuje ją w sposób chroniący integralność bazy danych – dotychczasowe 3 notatki pozostają w pliku nienaruszone, a nowy wpis zostaje dopisany na początku lub końcu struktury zgodnie z przyjętym formatem. W przypadku wystąpienia błędu dyskowego (np. brak praw do zapisu) program wyświetla czytelny komunikat po polsku, a dotychczasowy plik nie zostaje wyczyszczony ani uszkodzony.
- **Sprawdzenie**: 
  1. Przed zapisem wykonać kopię pliku z 3 notatkami.
  2. Zapisać czwartą notatkę i sprawdzić, czy plik zawiera komplet 4 notatek.
  3. Przetestować sytuację awaryjną (plik oznaczony atrybutem tylko do odczytu): sprawdzić, czy aplikacja wyświetla komunikat o błędzie po polsku i nie tworzy pustego pliku 0 bajtów.

### Kryterium 4: Blokada zapisu pustej notatki
- **Sytuacja początkowa**: Program jest uruchomiony, pole tytułu w formularzu nowej notatki jest puste (lub zawiera wyłącznie spacje/tabulatory), a pole treści zawiera dowolny tekst lub jest puste.
- **Działanie użytkownika**: Użytkownik spogląda na przycisk zapisu notatki lub próbuje go kliknąć / wywołać zapis.
- **Oczekiwany rezultat**: Kiedy pole tytułu jest puste lub zawiera wyłącznie białe znaki, to przycisk zapisu jest wizualnie i funkcjonalnie nieaktywny (zablokowany), kliknięcie w niego nie wywołuje żadnej akcji, plik na dysku nie ulega modyfikacji, a na liście notatek nie pojawia się żaden nowy element.
- **Sprawdzenie**: 
  1. Pozostawić pole tytułu puste, wpisać treść `Przykładowy tekst` i sprawdzić stan przycisku zapisu – zweryfikować właściwość zablokowania przycisku.
  2. Wpisać do pola tytułu ciąg spacji `   ` – zweryfikować, czy przycisk nadal pozostaje nieaktywny.
  3. Wpisać co najmniej jeden znak niebędący białym znakiem (np. `A`) – upewnić się, że przycisk natychmiast staje się aktywny.

### Kryterium 5: Działanie po ponownym uruchomieniu
- **Sytuacja początkowa**: Użytkownik dodał i zapisał w programie dwie notatki testowe, po czym zamknął program.
- **Działanie użytkownika**: Użytkownik ponownie uruchamia aplikację ze skrótu lub pliku wykonywalnego.
- **Oczekiwany rezultat**: Kiedy aplikacja zostaje ponownie uruchomiona, to okno główne pojawia się w czasie poniżej 2 sekund, wszystkie wcześniej zapisane notatki są automatycznie odczytane z lokalnego pliku i wyświetlone na liście w kolejności od najnowszej, a kliknięcie na notatkę prezentuje jej pełną, nienaruszoną treść.
- **Sprawdzenie**: 
  1. Zmierzyć stoperem czas od momentu wywołania programu do pełnego załadowania interfejsu (musi być < 2,0 s).
  2. Sprawdzić, czy obie wcześniej dodane notatki są widoczne na liście z poprawnymi tytułami i datami.
  3. Wybrać każdą z notatek i upewnić się, że treść w panelu podglądu jest w 100% zgodna z zapisaną przed zamknięciem.

### Kryterium 6: Podgląd wybranej notatki
- **Sytuacja początkowa**: Na liście notatek znajdują się co najmniej dwie różne notatki. Panel podglądu jest pusty lub wyświetla wcześniej wybraną notatkę.
- **Działanie użytkownika**: Użytkownik klika lewym przyciskiem myszy na wybraną pozycję na liście notatek.
- **Oczekiwany rezultat**: Kiedy użytkownik wskaże notatkę na liście, to w panelu podglądu natychmiast pojawia się pełny tytuł, data utworzenia oraz pełna treść wybranej notatki.
- **Sprawdzenie**: Kliknąć pierwszą notatkę, a następnie drugą notatkę na liście i sprawdzić, czy panel podglądu natychmiast aktualizuje wyświetlaną treść do odpowiadającej klikniętemu wpisowi.

---

## 6. Do ustalenia

W toku analizy wymagań zidentyfikowano następujące kwestie otwarte, które wymagają decyzji produktowej:

1. **[DO USTALENIA]: Dopuszczalność pustej treści notatki przy wypełnionym tytule**:
   - Czy użytkownik może zapisać notatkę, która posiada tytuł, ale jej pole treści pozostało całkowicie puste? (Wersja rekomendowana: zezwolić na pustą treść, gdyż sam tytuł może pełnić rolę szybkiego przypomnienia typu „Kupić mleko”).
2. **[DO USTALENIA]: Domyślna lokalizacja i nazwa pliku z notatkami**:
   - Gdzie dokładnie ma znajdować się plik notatek na dysku użytkownika: czy w bieżącym katalogu roboczym programu jako `notatki.md`, czy w dedykowanym katalogu użytkownika (np. standardowy folder Dokumenty w systemie operacyjnym)?
3. **[DO USTALENIA]: Sposób prezentacji treści w panelu podglądu**:
   - Czy panel podglądu powinien renderować znaczniki Markdown (nagłówki, pogrubienia, listy wypunktowane) w postaci sformatowanego widoku wizualnego, czy wyświetlać czysty tekst Markdown dokładnie tak, jak został wpisany przez użytkownika?
4. **[DO USTALENIA]: Zachowanie w przypadku zewnętrznej modyfikacji pliku**:
   - Jak program powinien zachować się, jeśli użytkownik zmodyfikuje plik `notatki.md` w zewnętrznym edytorze (np. Notatnik Windows) w czasie, gdy aplikacja jest uruchomiona – czy lista powinna automatycznie wykrywać zmiany na dysku i odświeżać widok, czy odczyt następuje wyłącznie przy starcie programu?
5. **[DO USTALENIA]: Limit długości tytułu i treści notatki**:
   - Czy tytuł notatki powinien mieć sztywny limit znaków (np. maksymalnie 200 znaków) ze względu na czytelność listy, czy pozostawać bez ograniczeń długości?
