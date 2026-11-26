# Plan implementacji: Notatnik (wersja 001)

## 1. Dokumenty wejściowe

Plan został opracowany na podstawie następujących dokumentów projektowych:
1. `konstytucja.md` – nadrzędne, nienegocjowalne reguły architektoniczne projektu Notatnik-V2.
2. `decyzje/0001-format-pliku-notatek.md` – decyzja architektoniczna (ADR) ustanawiająca format Markdown (`.md`) z kodowaniem UTF-8 i strukturą nagłówków.
3. `specs/001-notatnik/spec.md` – specyfikacja funkcjonalna definiująca zakres modułu, interakcje użytkownika oraz kryteria akceptacji (zaktualizowana po przeglądzie Pierwszego Czytelnika).

---

## 2. Sprawdzenie zgodności z Konstytucją projektu

Przed rozpoczęciem prac weryfikujemy projektowane rozwiązania techniczne względem każdego z 7 artykułów Konstytucji:

| Artykuł Konstytucji | Wymóg konstytucyjny | Zgodność w planie technicznym | Status |
| :--- | :--- | :--- | :---: |
| **Art. 1: Architektura jednoosobowa i lokalna** | Wyłącznie jedna osoba na jednym komputerze, brak multi-tenant, logowania, ról i synchronizacji. | Brak modułów uwierzytelniania, brak bazy użytkowników, brak komunikacji międzyprocesowej czy sieciowej. Całość działa w pojedynczym procesie desktopowym. | **Zgodny** |
| **Art. 2: Pełna suwerenność i lokalność danych** | Dane nie opuszczają dysku lokalnego użytkownika. | Zapis danych do pojedynczego pliku tekstowego na dysku lokalnym; brak połączeń zewnętrznych. | **Zgodny** |
| **Art. 3: Całkowity brak łączności sieciowej** | Aplikacja offline-first, brak telemetrii, analityki, bibliotek HTTP i usług chmurowych. | Projekt nie zawiera referencji do bibliotek sieciowych ani pakietów analitycznych. Całkowity brak wywołań sieciowych w kodzie. | **Zgodny** |
| **Art. 4: Otwarty i czytelny format zapisu** | Czysty tekst czytelny dla człowieka, otwieralny w dowolnym edytorze bez bazy binarnej. | Plik `notatki.md` w formacie Markdown z czytelnymi nagłówkami `## YYYY-MM-DD HH:mm — Tytuł` i czystym tekstem. Brak baz SQLite/plików binarnych. | **Zgodny** |
| **Art. 5: Język polski i obsługa polskich znaków** | Natywne wsparcie dla polskich znaków w UTF-8, interfejs i komunikaty w języku polskim. | Wszystkie strumienie I/O operują na `System.Text.Encoding.UTF8`. Wszystkie teksty w plikach XAML oraz komunikaty wyjątków/błędów są zdefiniowane po polsku. | **Zgodny** |
| **Art. 6: Oficjalny stos technologiczny** | Avalonia UI, .NET, C#, widoki XAML (.axaml). | Implementacja oparta na środowisku .NET 8 / 9, frameworku Avalonia UI w języku C# z deklaracją widoków w plikach `.axaml`. | **Zgodny** |
| **Art. 7: Nadrzędność konstytucji** | Brak zaprzeczeń regułom konstytucji w specyfikacji, planie i kodzie. | Plan w całości respektuje wszystkie powyższe założenia, nie wprowadzając żadnych kompromisów naruszających konstytucję. | **Zgodny** |

---

## 3. Wymagane pliki i struktura projektu

Projekt zostanie zorganizowany w oparciu o architekturę MVVM (Model-View-ViewModel) ze ścisłą separacją warstwy prezentacji, logiki biznesowej i dostępu do danych:

```text
src/
└── NotatnikV2/
    ├── NotatnikV2.csproj                # Plik projektu .NET / Avalonia UI
    ├── Program.cs                       # Punkt wejścia aplikacji (inicjalizacja Avalonia)
    ├── App.axaml                        # Deklaracja stylów i zasobów aplikacji XAML
    ├── App.axaml.cs                     # Inicjalizacja kontekstu aplikacji i okna głównego
    ├── Models/
    │   └── Note.cs                      # Model pojedynczej notatki (Title, Content, CreatedAt)
    ├── Services/
    │   ├── INoteStorageService.cs       # Interfejs serwisu zapisu i odczytu notatek
    │   └── MarkdownNoteStorageService.cs# Implementacja I/O pliku Markdown w UTF-8 z bezpiecznym zapisem atomowym
    ├── ViewModels/
    │   ├── ViewModelBase.cs             # Klasa bazowa INotifyPropertyChanged
    │   └── MainViewModel.cs             # Główny ViewModel: kolekcja notatek, walidacja, cykl formularza
    └── Views/
        ├── MainWindow.axaml             # Deklaracja widoku UI: lewy panel listy, formularz, podgląd ze stanem pustym
        └── MainWindow.axaml.cs          # Code-behind okna głównego
```

---

## 4. Kolejność implementacji

Prace programistyczne zostaną przeprowadzone w pięciu kolejnych fazach:

### Faza 1: Inicjalizacja szkieletu projektu i konfiguracja środowiska
1. Utworzenie projektu aplikacji Avalonia UI z platformą docelową .NET.
2. Konfiguracja zależności w pliku `NotatnikV2.csproj`.
3. Przygotowanie podstawowych plików startowych `Program.cs`, `App.axaml`, `App.axaml.cs`.

### Faza 2: Model danych i serwis bezpiecznego zapisu (I/O)
1. Utworzenie klasy modelu `Note.cs` (właściwości: `Title`, `Content`, `CreatedAt`).
2. Implementacja parsera Markdown w `MarkdownNoteStorageService.cs`:
   - Zapewnienie kodowania `Encoding.UTF8`.
   - Odczyt i parsowanie ustandaryzowanego nagłówka `## YYYY-MM-DD HH:mm — Tytuł`.
   - Ochrona parsera przed rozbijaniem wpisów w przypadku wystąpienia znaczników Markdown w treści notatki.
3. Implementacja bezpiecznego zapisu atomowego:
   - Zapis nowej notatki do pliku tymczasowego (`notatki.md.tmp`).
   - Atomowa podmiana lub bezpieczne dopisanie z zachowaniem integralności pliku bazy w razie przerwania zasilania lub błędu dyskowego.

### Faza 3: Warstwa ViewModel i logika biznesowa (MVVM)
1. Implementacja `ViewModelBase.cs` ze wsparciem dla powiadomień o zmianie właściwości (`INotifyPropertyChanged`).
2. Implementacja `MainViewModel.cs`:
   - Kolekcja notatek typu `ObservableCollection<Note>` posortowana malejąco po dacie utworzenia.
   - Właściwości wejściowe dla formularza: `NewNoteTitle`, `NewNoteContent`.
   - Logika walidacji pustego tytułu (`CanSave` – aktywny tylko wtedy, gdy `!string.IsNullOrWhiteSpace(NewNoteTitle)`).
   - Komenda zapisu notatki (`SaveNoteCommand`) z natychmiastowym dodaniem do kolekcji na pozycję 0, wyczyszczeniem pól formularza i zresetowaniem `CanSave`.
   - Właściwość wybranej notatki (`SelectedNote`) aktualizująca panel podglądu z obsługą stanu pustego (gdy brak selekcji).

### Faza 4: Interfejs użytkownika w XAML (Avalonia UI)
1. Przygotowanie responsywnego układu okna w `MainWindow.axaml`:
   - Lewy panel: Lista notatek (`ListBox`) z szablonem prezentującym tytuł pogrubioną czcionką oraz datę utworzenia.
   - Prawy panel górny: Niezależny formularz tworzenia nowej notatki (pole tytułu `TextBox`, pole treści `TextBox` z zawijaniem wierszy, przycisk `Button` powiązany z komendą zapisu i walidacją).
   - Prawy panel dolny: Panel podglądu wybranej notatki (`SelectedNote`) z komunikatem zachęty, gdy żadna notatka nie jest zaznaczona.
2. Ustawienie wszystkich etykiet, nagłówków i tekstów interfejsu w języku polskim.

### Faza 5: Integracja, obsługa błędów i weryfikacja kryteriów
1. Obsługa wyjątków I/O (`IOException`, `UnauthorizedAccessException`) z prezentacją czytelnych komunikatów w języku polskim w dedykowanym pasku informacyjnym okna.
2. Sprawdzenie czasu uruchomienia aplikacji (optymalizacja ładowania wstępnego, cel: < 2 sekundy).
3. Wykonanie manualnych testów weryfikacyjnych dla 5 obowiązkowych kryteriów akceptacji.

---

## 5. Elementy pozostające poza zakresem (Out of Scope)

W wersji 001 następujące elementy i technologie pozostają bezwzględnie poza zakresem prac:
1. **Wyszukiwarka i zaawansowane filtrowanie**: Świadomie odłożone do kolejnej wersji w celu terminowej realizacji zadania (zgodnie z ADR 0001).
2. **Edycja i usuwanie wpisów z poziomu GUI**: W wersji 001 modyfikacja i usuwanie notatek odbywa się bezpośrednio w pliku `.md` w zewnętrznym edytorze tekstu.
3. **Silniki baz danych (SQLite, LiteDB, SQL Server)**: Odrzucone ze względu na niezgodność z Art. 4 Konstytucji (wymóg czystego tekstu).
4. **Moduły sieciowe i chmurowe**: Zabronione na mocy Art. 2 i 3 Konstytucji.
5. **Renderowanie skomplikowanego HTML/WYSIWYG**: Podgląd bazuje na czytelnym tekście Markdown bez zewnętrznych silników przeglądarkowych.
