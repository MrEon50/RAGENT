# RAGENT: Information Competition RAG
RAGENT to zaawansowany moduł (Information Competition RAG Pipeline) w Pythonie realizujący koncepcje rygorystycznej 
selekcji informacji przed pokazaniem ich do LLM (np GPT, Claude).

Zamiast standardowego wektorowego wyszukiwania implementuje on metody typu Reciprocal Rank Fusion,
Diversity Filtering (MMR), Time-Decay freshness oraz Cross-Encoder Reranking by wymusić
"Walkę w klatce" między kandydatami i zmniejszyć szum (hallucinations) modelu.

## Instalacja

Możesz mieć to w swoim venv bardzo prosto:
```bash
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
pip install -e .
```

## Mechanika i Funkcje:
Zainplementowane i zrealizowane ulepszenia wokół "Information Competition":

1. **Reciprocal Rank Fusion (RRF)** - zderza słowa (BM25) ze znaczeniem (Vector) dając sprawiedliwy znormalizowany wynik.
2. **MMR Diversity** - dywersyfikacja by ukrócić redundancję podobnych do siebie wyników.
3. **Time-Decay** - dokument z wczoraj będzie miał automatycznie 2x większe znaczenie score niż ten sprzed roku.
4. **Source Authority** - pewne systemy są zawsze pewniejsze (np dokumenty ofcjalne) nad forum postami.
5. **Adaptive Threshold** - system nie rzuca na oślep 5 kawałkami z bazy bo tak ma w konfigu. Rzuci 3 bo tylko one dociągnęły do progu punktacji.
6. **Cross Encoder Re-ranking** - powolny, ciężki, dokładny sędzia ringu - `bge-reranker-v2-m3` wymusza twardą ocenę dokumentów z fuzji względem aktualnego zapytania.
7. **Context Token Manager** - kontrola zapytań by nigdy nie rozbić sufitu LLM limitu zapytania.

## Interfejs Web & Dashboard (Nowość!)

RAGENT posiada teraz nowoczesny interfejs webowy, który pozwala na interaktywną pracę z modelami Ollama oraz zarządzanie bazą wiedzy w czasie rzeczywistym.

### Kluczowe funkcje UI:
1. **Menedżer Zakładek (Scheduler)** – Planuj zadania automatyczne! Możesz ustawić cykliczne lub jednorazowe pobieranie danych z sieci (Internet), adresów URL lub plików lokalnych o konkretnej godzinie. Model przeanalizuje dane i doda wynik bezpośrednio do bazy RAG.
2. **Licznik Tokenów** – Precyzyjna informacja pod każdą odpowiedzią o zużyciu tokenów (Prompt → Completion).
3. **Timer Sesji** – Monitoruj czas spędzony na pracy z systemem bezpośrednio w panelu statystyk.
4. **Wsparcie dla Modeli Myślących** – Pełna obsługa modeli typu DeepSeek; proces myślenia modelu (thinking process) jest wyświetlany w dedykowanym, rozwijanym oknie.
5. **Monitor Systemowy** – Podgląd obciążenia CPU, RAM oraz transferu sieciowego w czasie rzeczywistym.

---

## Użycie 

Najprostszym sposobem na uruchomienie pełnego systemu z interfejsem graficznym jest użycie gotowych skryptów:

1. Upewnij się, że **Ollama** jest uruchomiona w tle.
2. Uruchom plik **`run_web.bat`** znajdujący się w głównym katalogu.
3. Po uruchomieniu serwera, kliknij z klawiszem **Ctrl** w poniższy adres w konsoli (lub wpisz go w przeglądarce):
   > **[http://localhost:8765](http://localhost:8765)**

Dla celów testowych w samej konsoli (bez UI), możesz nadal użyć:
```bash
python examples/demo.py
```

