# Zadania implementacyjne: Notatnik (wersja 001)

Dokument zawiera precyzyjną, dekomponowaną listę zadań do wykonania w procesie implementacji Notatnika-V2 w wersji 001. Każde zadanie jest zdefiniowane w sposób atomowy (jedno zadanie = jeden logiczny commit) i posiada bezwzględnie weryfikowalny warunek ukończenia.

---

### T01: Inicjalizacja projektu bazowego Avalonia UI w .NET
- **Opis**: Utworzenie struktury projektu desktopowego w środowisku .NET przy użyciu szablonu Avalonia UI, z konfiguracją pliku projektu oraz podstawowych plików startowych.
- **Gotowe, gdy**: W katalogu `src/NotatnikV2/` znajdują się pliki `NotatnikV2.csproj`, `Program.cs`, `App.axaml` oraz `App.axaml.cs`, a polecenie `dotnet build` wykonane w tym katalogu kończy się kodem wyjścia 0 z zerem błędów kompilacji.
- **Sugerowany opis commitu**: `feat(init): inicjalizacja projektu Avalonia UI dla Notatnika-V2`

---

### T02: Implementacja modelu danych notatki (Note)
- **Opis**: Utworzenie klasy `Note` reprezentującej pojedynczy wpis notatki z właściwościami `Title` (string), `Content` (string) oraz `CreatedAt` (DateTime), wraz z metodą formatującą nagłówek zgodnie ze standardem `## YYYY-MM-DD HH:mm — Tytuł`.
- **Gotowe, gdy**: Istnieje plik `src/NotatnikV2/Models/Note.cs` zawierający klasę `Note` z polami `Title`, `Content`, `CreatedAt` oraz funkcją formatowania nagłówka; kod kompiluje się bez ostrzeżeń.
- **Sugerowany opis commitu**: `feat(model): implementacja klasy modelu Note`

---

### T03: Implementacja serwisu I/O z kodowaniem UTF-8 i parsowaniem Markdown
- **Opis**: Stworzenie interfejsu `INoteStorageService` oraz implementacji `MarkdownNoteStorageService` odczytującej i parsującej plik `notatki.md` przy wymuszonym kodowaniu UTF-8 bez użycia zewnętrznych baz danych.
- **Gotowe, gdy**: Klasa `MarkdownNoteStorageService` implementuje metodę `ReadAllNotesAsync()`, która poprawnie wczytuje notatki z pliku UTF-8 z polskimi znakami i poprawnie rozdziela nagłówki od treści notatki, a test odczytu pliku z polskimi znakami zwraca wierny ciąg znaków.
- **Sugerowany opis commitu**: `feat(storage): implementacja odczytu notatek w formacie Markdown UTF-8`

---

### T04: Implementacja mechanizmu bezpiecznego zapisu atomowego do pliku
- **Opis**: Rozbudowa `MarkdownNoteStorageService` o metodę `SaveNoteAsync(Note note)` realizującą bezpieczny zapis (zapis do pliku tymczasowego `.tmp` i bezpieczna podmiana/dopisanie), chroniący przed uszkodzeniem pliku bazy w przypadku błędu dyskowego.
- **Gotowe, gdy**: Próba zapisu notatki przy symulowanym braku uprawnień lub przerwaniu I/O nie powoduje usunięcia ani wyczyszczenia istniejącego pliku `notatki.md`, a poprawny zapis powiększa bazę o nową notatkę.
- **Sugerowany opis commitu**: `feat(storage): implementacja bezpiecznego zapisu atomowego notatek`

---

### T05: Implementacja MainViewModel z sortowaniem, walidacją i cyklem formularza
- **Opis**: Utworzenie klasy `MainViewModel` dziedziczącej po `ViewModelBase`, zarządzającej kolekcją `ObservableCollection<Note>` posortowaną od najnowszej, właściwościami formularza, logiką blokady zapisu przy pustym tytule oraz czyszczeniem formularza po zapisie.
- **Gotowe, gdy**: Właściwość `CanSave` zwraca `false`, gdy pole tytułu jest puste lub zawiera same spacje, zwraca `true`, gdy tytuł zawiera co najmniej jeden niebiały znak; wywołanie zapisu wstawia nowy obiekt `Note` na indeks 0 kolekcji, czyści pola `NewNoteTitle` i `NewNoteContent` oraz resetuje `CanSave` do `false`.
- **Sugerowany opis commitu**: `feat(viewmodel): implementacja MainViewModel z walidacją tytułu, sortowaniem i czyszczeniem pól`

---

### T06: Budowa deklaratywnego widoku głównego w XAML (MainWindow.axaml)
- **Opis**: Utworzenie widoku okna głównego w technologii Avalonia XAML zawierającego lewy panel z listą notatek (`ListBox`), prawy panel z formularzem dodawania (pola tytułu i treści, przycisk zapisu spięty z `CanSave`) oraz dolny obszar podglądu wybranej notatki z obsługą komunikatu zachęty (stan pusty).
- **Gotowe, gdy**: Plik `MainWindow.axaml` kompiluje się, zawiera polskie etykiety kontrolek (`Tytuł`, `Treść`, `Zapisz notatkę`, `Lista notatek`, `Podgląd`, tekst stanu pustego), poprawnie binduje kontrolki do właściwości `MainViewModel`, a wybór pozycji na liście nie wpływa na tekst w polach formularza.
- **Sugerowany opis commitu**: `feat(ui): deklaracja interfejsu graficznego MainWindow w XAML ze stanami pustymi`

---

### T07: Obsługa wyjątków operacji wejścia/wyjścia z polskimi komunikatami
- **Opis**: Dodanie w ViewModelu i widoku obsługi błędów operacji na plikach (np. `UnauthorizedAccessException`, `DirectoryNotFoundException`) wyświetlającej czytelne powiadomienie o błędzie w języku polskim w dedykowanym pasku informacyjnym zamiast nagłego zamknięcia programu.
- **Gotowe, gdy**: W sytuacji błędu zapisu aplikacja nie rzuca nieobsłużonego wyjątku (nie zamyka się), lecz wyświetla w interfejsie zrozumiały dla użytkownika komunikat o treści wskazującej na problem z dostępem do dysku w języku polskim.
- **Sugerowany opis commitu**: `feat(error-handling): polska obsługa błędów I/O w interfejsie użytkownika`

---

### T08: Weryfikacja akceptacyjna i optymalizacja czasu startu programu
- **Opis**: Przeprowadzenie pełnej procedury testowej sprawdzającej wszystkie kryteria akceptacji ze specyfikacji oraz pomiar czasu uruchomienia programu.
- **Gotowe, gdy**: Czas od wywołania aplikacji do wyświetlenia okna wynosi poniżej 2,0 sekund, a wszystkie scenariusze akceptacyjne (polskie znaki, aktualność listy, bezpieczeństwo zapisu, blokada pustej notatki, trwałość po restarcie, podgląd z zachowaniem szkicu) zakończyły się wynikiem pozytywnym.
- **Sugerowany opis commitu**: `test: pełna weryfikacja kryteriów akceptacji specyfikacji v001`
