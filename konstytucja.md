# Konstytucja projektu Notatnik-V2

Niniejszy dokument określa fundamentalne, nadrzędne i niezmienne zasady projektu Notatnik-V2. Wszystkie decyzje techniczne, specyfikacje funkcjonalne oraz przyszłe implementacje muszą pozostawać w pełnej zgodzie z poniższymi regułami.

---

### 1. Architektura jednoosobowa i lokalna
Notatnik jest programem przeznaczonym wyłącznie dla jednej osoby na jednym komputerze. Aplikacja nie posiada architektury wielodostępnej (multi-tenant), kont użytkowników, logowania, ról ani mechanizmów synchronizacji rozproszonej pomiędzy różnymi maszynami.

### 2. Pełna suwerenność i lokalność danych
Dane użytkownika nigdy nie opuszczają jego komputera. Wszystkie tworzone notatki, metadane oraz pliki konfiguracyjne są zapisywane i przechowywane bezpośrednio oraz wyłącznie na lokalnym dysku twardym użytkownika.

### 3. Całkowity brak łączności sieciowej (Offline-first)
Program nie wysyła żadnych danych do internetu ani nie wymaga aktywnego połączenia sieciowego do żadnej ze swoich operacji. Aplikacja nie zawiera modułów telemetrii, narzędzi śledzących, analityki użytkowania, powiadomień zewnętrznych ani integracji z usługami chmurowymi.

### 4. Otwarty i czytelny dla człowieka format zapisu
Notatki są przechowywane w formacie czystego tekstu, czytelnym dla człowieka i możliwym do otwarcia, odczytania oraz edycji w dowolnym zwykłym edytorze tekstu (np. Notatnik Windows, nano, VS Code) bez pośrednictwa aplikacji Notatnik-V2. Format danych nigdy nie może zostać zmieniony na binarny, zaszyfrowany w sposób uniemożliwiający odczyt poza aplikacją ani zamknięty w formacie własnościowym.

### 5. Język polski i pełna obsługa polskich znaków
Interfejs aplikacji jest tworzony w języku polskim i natywnie obsługuje polskie znaki diakrytyczne (standard kodowania UTF-8) w polach wprowadzania tekstu, widokach list, oknach dialogowych oraz komunikatach systemowych.

### 6. Oficjalny stos technologiczny
Projekt przyszłej aplikacji wykorzystuje framework Avalonia UI oraz platformę .NET z językiem C# i warstwą widoku deklarowaną w XAML (.axaml). Wybór ten gwarantuje spójne, nowoczesne środowisko desktopowe zachowujące pełną autonomię kodu.

### 7. Nadrzędność konstytucji i procedura zmian
Specyfikacja żadnej funkcji, plan zadań ani bieżący kod programu nie może zaprzeczać niniejszej konstytucji. Zmiana któregokolwiek z punktów konstytucji jest traktowana jako operacja najwyższej wagi i wymaga osobnej, sformalizowanej i zapisanej decyzji architektonicznej (ADR) wraz ze szczegółowym uzasadnieniem.
