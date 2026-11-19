# 0001. Format pliku notatek w Notatniku-V2

- **Status**: Zaakceptowana
- **Data**: 2026-11-05
- **Autor**: Zespół projektowy Notatnik-V2

---

## Kontekst

W poprzedniej wersji aplikacji (Budowa 2, zadanie 3.2 – projekt `notatnik`) notatki były zapisywane w pojedynczym pliku `notatki.md` (początkowo w rundzie 1–2 był to plik `notatki.txt` z separatorami `---`). W pliku `notatki.md` każdy wpis miał postać nagłówka Markdown drugiego stopnia z datą i tytułem: `## YYYY-MM-DD HH:mm — Tytuł`, a pod nim znajdowała się treść notatki oddzielona pustą linią. Zapis nowej notatki polegał na wczytaniu całej dotychczasowej zawartości pliku do pamięci (`File.ReadAllText`), doklejeniu jej poniżej nowego wpisu i ponownym nadpisaniu całego pliku (`File.WriteAllText`).

Dotychczasowe rozwiązanie rodziło konkretne problemy techniczne i użytkowe:
1. **Podatność parsowania na błędy (brak odporności)**: Parsowanie pliku w C# opierało się na wyrażeniu regularnym dzielącym tekst po nagłówkach: `Regex.Split(content, @"(?:\r?\n\r?\n)+(?=## )")`. Jeżeli użytkownik w treści samej notatki użył nagłówka Markdown drugiego stopnia (`## `), parser błędnie dzielił jedną notatkę na dwa niezależne elementy na liście.
2. **Konieczność każdorazowego przepisywania całego pliku**: Wstawianie najnowszych notatek na początek pliku wymagało odczytu i ponownego zapisu całego pliku na dysku, co przy dużej liczbie notatek obniżało wydajność i rodziło ryzyko uszkodzenia lub utraty całej bazy danych w razie awarii zasilania lub błędu I/O w trakcie operacji zapisu.
3. **Trudność modyfikacji i usuwania pojedynczych wpisów**: Struktura jednolitego bloku tekstowego uniemożliwiała prostą, izolowaną edycję lub skasowanie wybranej notatki bez ponownego parsowania i rekonstruowania całego pliku.

Dla Notatnika-V2 konieczne jest ustalenie formatu zapisu, który rozwiąże te bolączki, zachowując bezwzględną zgodność z Konstytucją projektu.

---

## Decyzja

Dla aplikacji Notatnik-V2 wybieramy format **Markdown (`.md`)** z ustandaryzowaną strukturą nagłówków oraz jednoznacznym kodowaniem **UTF-8**.

Wybór ten wynika z następujących przesłanek:
- **Pełna czytelność dla człowieka**: Plik Markdown jest czystym tekstem, w pełni zrozumiałym bez żadnego dedykowanego oprogramowania. Użytkownik widzi strukturę dokumentu, daty, tytuły i treść w naturalnej formie.
- **Możliwość edycji w zwykłym edytorze**: Plik można bezpośrednio i bezpiecznie edytować w dowolnym prostym programie tekstowym (Notatnik Windows, VS Code, Notepad++, Obsidian, nano) bez ryzyka naruszenia binarnej struktury.
- **Zgodność z Konstytucją**: Rozwiązanie w 100% realizuje punkt 4 Konstytucji (format czytelny dla człowieka i otwieralny w zwykłym edytorze tekstu), punkt 2 (dane na lokalnym dysku) oraz punkt 5 (natywna obsługa polskich znaków dzięki UTF-8).
- **Przyszła implementacja w C# i Avalonia UI**: Środowisko .NET oferuje bogate wsparcie do pracy ze strumieniami tekstu oraz dojrzałe, wydajne biblioteki do obsługi i parsowania Markdown (np. `Markdig`). Pozwoli to w przyszłości na bezproblemowe renderowanie sformatowanego podglądu (listy, pogrubienia, kursywy) bezpośrednio w kontrolkach Avalonia UI.

---

## Odrzucone

1. **JSON (`.json`)**:
   - *Powód odrzucenia*: Mimo że format JSON jest domyślnym standardem w C# (`System.Text.Json`) i ułatwia automatyczną serializację/deserializację struktur obiektowych, jest nieprzyjazny dla zwykłego użytkownika przy bezpośrednim przeglądaniu w prostym edytorze tekstu. Wymaga rygorystycznego pilnowania składni (nawiasy klamrowe, przecinki, uciekanie znaków nowej linii `\n` oraz cudzysłowów `\"`). Przypadkowy błąd użytkownika podczas ręcznej edycji (np. brakujący przecinek) prowadzi do błędu parsowania i braku możliwości odczytania całego pliku w aplikacji. Naruszałoby to ideę prostego, czytelnego notatnika.
2. **Osobny plik `.md` dla każdej notatki w dedykowanym katalogu**:
   - *Powód odrzucenia*: Zapisywanie każdej notatki jako osobnego pliku na dysku (np. w folderze `notatki/`) powoduje szybkie powstawanie bałaganu w systemie plików (dziesiątki lub setki drobnych plików). Utrudnia użytkownikowi proste tworzenie kopii zapasowej i przenoszenie notatek (zamiast jednego pliku trzeba zarządzać całą strukturą katalogów). Ponadto rodzi to problemy z unikalnością i poprawnością nazw plików w systemie operacyjnym Windows, gdzie tytuły notatek mogą zawierać znaki zabronione w ścieżkach systemowych (`\ / : * ? " < > |`).
3. **Relacyjna baza danych SQLite (`.db` / `.sqlite`)**:
   - *Powód odrzucenia*: Plik bazy SQLite jest formatem binarnym. Użytkownik nie jest w stanie podejrzeć ani edytować swoich notatek w zwykłym edytorze tekstu bez instalowania specjalistycznego oprogramowania bazodanowego. Taki format wprost łamałby punkt 4 Konstytucji projektu Notatnik-V2.

---

## Skutki

### Korzyści
- **Pewność i trwałość danych**: Użytkownik zachowuje pełną niezależność od aplikacji Notatnik-V2 – jego notatki są bezpieczne i czytelne nawet w przypadku odinstalowania programu.
- **Wygoda tworzenia kopii zapasowej**: Cała baza notatek znajduje się w jednym pliku tekstowym, co umożliwia błyskawiczny backup lub archiwizację.
- **Wsparcie dla formatowania**: Zastosowanie Markdown umożliwia użytkownikowi tworzenie nagłówków, list punktowanych, bloków kodu i wyróżnień tekstowych, które w przyszłości zostaną zwizualizowane w Avalonia UI.
- **Brak zależności od ciężkich bibliotek**: Brak konieczności dołączania silników bazodanowych do projektu.

### Ograniczenia i trudności (co decyzja utrudnia)
- **Konieczność stworzenia odpornego parsera w C#**: W odróżnieniu od formatu JSON czy bazy SQL, format Markdown wymaga zaprojektowania stabilnego mechanizmu oddzielania metadanych (tytuł, data utworzenia) od właściwej treści, tak aby znaczniki Markdown użyte przez użytkownika wewnątrz treści notatki nie psuły podziału na wpisy.
- **Skalowalność przy dużej liczbie notatek**: Przy tysiącach wpisów w jednym pliku tekstowym operacje wyszukiwania, sortowania czy filtrowania w locie mogą wymagać wczytania całego pliku do pamięci i buforowania obiektów w pamięci RAM, co wymusi optymalizację algorytmów ładowania danych w C#.
- **Ryzyko konfliktów przy równoległej edycji**: Jeśli użytkownik otworzy plik notatek w zewnętrznym edytorze tekstowym i jednocześnie dokona zapisu z poziomu Notatnika-V2, może dojść do konfliktu blokady pliku lub nadpisania zmian, co wymagać będzie obsługi wyjątków I/O w warstwie C#.
