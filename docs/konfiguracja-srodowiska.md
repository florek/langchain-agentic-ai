# Konfiguracja środowiska projektu

## Wymagania wstępne

Kurs zakłada znajomość Pythona, programowania obiektowego, pracy z terminalem, Gita, środowisk wirtualnych i zmiennych środowiskowych. Wcześniejsza znajomość Machine Learningu nie jest wymagana.

## Python 3.12+

Projekt wymaga Pythona w wersji co najmniej 3.12. Wersja jest przypięta w pliku konfiguracyjnym środowiska, aby wszyscy uczestnicy kursu pracowali na tej samej wersji interpretera.

## Menedżer pakietów uv

Projekt korzysta z **uv** — nowoczesnego menedżera pakietów i środowisk wirtualnych dla Pythona. Zalety w kontekście kursu:

- szybka instalacja zależności,
- deterministyczny plik lock z dokładnymi wersjami,
- automatyczne tworzenie i aktywacja wirtualnego środowiska,
- kompatybilność z formatem `pyproject.toml`.

Typowy przepływ pracy:

1. Sklonowanie repozytorium.
2. Uruchomienie `uv sync` — tworzy `.venv` i instaluje zależności z lockfile.
3. Uruchomienie skryptów przez `uv run python ...` lub po aktywacji środowiska.

## Struktura zależności

Główne pakiety zainstalowane w projekcie:

| Pakiet | Rola |
|--------|------|
| `langchain` | Rdzeń frameworka LangChain (łańcuchy, agenci, abstrakcje LLM) |
| `langchain-openai` | Integracja z modelami OpenAI (GPT-4, GPT-4o itd.) |
| `langchain-ollama` | Integracja z lokalnymi modelami przez serwer Ollama |
| `python-dotenv` | Ładowanie zmiennych środowiskowych z pliku `.env` |
| `black` | Automatyczne formatowanie kodu |
| `isort` | Sortowanie importów |

## Narzędzia developerskie

**Black** i **isort** zapewniają spójny styl kodu w całym repozytorium. Są instalowane jako zależności projektu, więc każdy uczestnik ma te same wersje bez dodatkowej konfiguracji globalnej.

## Plik .gitignore

Repozytorium ignoruje:

- katalogi środowisk wirtualnych (`venv/`, `.venv/`),
- plik `.env` z sekretami (klucze API),
- artefakty Pythona (`__pycache__/`).

**Kluczowa zasada bezpieczeństwa:** nigdy nie commituj pliku `.env` ani kluczy API do repozytorium. Plik `.env` istnieje lokalnie i jest ładowany w runtime przez `python-dotenv`.

## Uruchamianie punktu wejścia

Aplikacja startowa ładuje zmienne środowiskowe przy imporcie modułu (`load_dotenv()` wywoływane na poziomie modułu, przed definicją funkcji `main`). Dzięki temu każde wywołanie `os.getenv(...)` w programie widzi wartości z pliku `.env`.

Wzorzec:

```python
from dotenv import load_dotenv

load_dotenv()

def main():
    # logika aplikacji
```

Wywołanie `load_dotenv()` szuka pliku `.env` w bieżącym katalogu roboczym i ustawia zmienne środowiskowe procesu. Nie nadpisuje zmiennych już ustawionych w systemie operacyjnym (domyślne zachowanie).

## Zmienne środowiskowe API

Klucze do zewnętrznych usług LLM (np. OpenAI) przechowuje się w zmiennych środowiskowych, nie w kodzie źródłowym. Typowa nazwa to `OPEN_AI_KEY` lub `OPENAI_API_KEY` — w projekcie używana jest forma zgodna z konfiguracją lokalnego pliku `.env`.

Odczyt w kodzie:

```python
import os
api_key = os.getenv("OPEN_AI_KEY")
```

Jeśli zmienna nie istnieje, `getenv` zwraca `None` — aplikacja powinna obsłużyć ten przypadek (komunikat błędu lub fallback), zanim spróbuje wywołać API.

## Ollama jako lokalna alternatywa

**Ollama** uruchamia modele open-source lokalnie (Llama, Mistral, Gemma itd.) bez kosztów API. Pakiet `langchain-ollama` udostępnia klasy `ChatOllama` i `OllamaEmbeddings` kompatybilne z resztą ekosystemu LangChain. Wymaga działającego serwera Ollama na maszynie deweloperskiej (domyślnie port 11434).

## Pułapki konfiguracyjne

- Uruchamianie skryptu spoza katalogu projektu — `load_dotenv()` może nie znaleźć `.env`.
- Brak aktywnego środowiska wirtualnego — import `langchain` kończy się błędem `ModuleNotFoundError`.
- Commitowanie `.env` do Gita — wyciek klucza API; plik musi pozostać w `.gitignore`.
- Mylenie nazw zmiennych (`OPEN_AI_KEY` vs `OPENAI_API_KEY`) — integracja LangChain OpenAI oczekuje standardowej nazwy z dokumentacji pakietu.
