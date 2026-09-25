# Zmiana modelu LLM w LangChain — OpenAI, Ollama i Gemma 3

## 1. Cel lekcji

W tej części wykładu chodzi o pokazanie jednej z największych zalet **LangChain**:

> Możemy stosunkowo łatwo zmieniać model językowy, z którego korzysta nasza aplikacja.

Na przykład możemy przejść z:

```text
OpenAI GPT-5
```

na:

```text
Gemma 3
```

uruchamianą **lokalnie na własnym komputerze** za pomocą **Ollama**.

Najważniejsza idea jest taka:

> Reszta aplikacji może pozostać prawie bez zmian — zmieniamy głównie obiekt modelu.

---

## 2. Dlaczego to jest ważne?

LangChain oddziela kod aplikacji od konkretnego dostawcy LLM.

Możemy więc używać różnych modeli:

- OpenAI,
- Google Gemini,
- Anthropic Claude,
- modeli open-weight,
- lokalnych modeli przez Ollama,
- modeli dostępnych przez zewnętrzne API.

Dzięki temu aplikacja nie musi być na stałe związana z jednym modelem.

Można powiedzieć obrazowo:

> W LangChain modele można zmieniać prawie jak „skarpetki”.

Oczywiście każdy dostawca wymaga odpowiedniego klienta i konfiguracji, ale sam interfejs pracy z modelem pozostaje bardzo podobny.

---

## 3. Co oznacza model open-weight?

Model **open-weight** to model, którego wagi są dostępne do pobrania.

Dzięki temu możemy uruchomić go:

- lokalnie,
- na własnym serwerze,
- w chmurze,
- bez konieczności wysyłania każdego zapytania do zewnętrznego API.

Przykładem jest **Gemma 3** firmy Google.

---

## 4. Po co uruchamiać model lokalnie?

Lokalny model ma kilka zalet:

- nie wymaga zewnętrznego API przy każdym zapytaniu,
- dane mogą pozostać na naszym komputerze,
- nie płacimy za każde wywołanie API,
- możemy pracować offline,
- możemy eksperymentować z różnymi modelami.

Są też wady:

- potrzebujemy odpowiedniego sprzętu,
- słabsze modele lokalne mogą mieć gorszą jakość odpowiedzi,
- większe modele wymagają dużo RAM-u lub VRAM-u,
- lokalne generowanie może być wolniejsze.

---

## 5. Ollama

Do uruchamiania modeli lokalnie wykorzystujemy **Ollama**.

Ollama upraszcza:

- pobieranie modeli,
- przechowywanie modeli,
- uruchamianie modeli,
- komunikację z nimi przez terminal,
- używanie ich z poziomu kodu.

Po instalacji możemy korzystać z komendy:

```bash
ollama
```

---

## 6. Instalacja Ollama

Po instalacji warto sprawdzić, czy wszystko działa:

```bash
ollama --help
```

Możemy również zobaczyć modele dostępne lokalnie:

```bash
ollama list
```

Na początku lista może być pusta.

---

## 7. Pobieranie modelu Gemma 3

Na potrzeby demonstracji można użyć małego modelu, np.:

```text
Gemma 3 270M
```

`270M` oznacza około:

```text
270 milionów parametrów
```

Mały model:

- pobiera się szybciej,
- zużywa mniej pamięci,
- działa szybciej,
- ale zwykle daje słabsze odpowiedzi niż większe modele.

Model pobieramy komendą:

```bash
ollama pull gemma3:270m
```

Po pobraniu możemy sprawdzić:

```bash
ollama list
```

---

## 8. Uruchomienie modelu w terminalu

Model możemy uruchomić bez pisania kodu:

```bash
ollama run gemma3:270m
```

Otrzymujemy wtedy prosty interfejs CLI.

Możemy wpisać:

```text
Hello
```

a model wygeneruje odpowiedź.

Dzięki temu łatwo sprawdzić, czy:

- Ollama działa,
- model został poprawnie pobrany,
- lokalna inferencja działa poprawnie.

---

## 9. Ollama jako lokalny backend dla aplikacji

Schemat wygląda tak:

```text
Aplikacja Python
       ↓
LangChain
       ↓
ChatOllama
       ↓
Ollama
       ↓
Gemma 3
```

Model działa lokalnie na naszym komputerze.

---

## 10. Integracja LangChain z Ollama

Do integracji używamy odpowiedniego pakietu LangChain.

Przykładowy import:

```python
from langchain_ollama import ChatOllama
```

Następnie tworzymy obiekt modelu:

```python
llm = ChatOllama(
    model="gemma3:270m",
    temperature=0
)
```

---

## 11. Parametry modelu

### `model`

Określa model, którego chcemy używać:

```python
model="gemma3:270m"
```

Nazwa musi odpowiadać modelowi dostępnemu w Ollama.

Możemy to sprawdzić:

```bash
ollama list
```

### `temperature`

Parametr:

```python
temperature=0
```

oznacza, że model ma generować bardziej przewidywalne i stabilne odpowiedzi.

W uproszczeniu:

```text
temperature = 0
→ bardziej przewidywalnie

temperature = 0.7
→ bardziej kreatywnie
```

---

## 12. Największa zaleta LangChain

Załóżmy, że wcześniej mieliśmy:

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="gpt-5"
)
```

A teraz chcemy użyć lokalnego modelu:

```python
from langchain_ollama import ChatOllama

llm = ChatOllama(
    model="gemma3:270m",
    temperature=0
)
```

Pozostała część kodu może wyglądać bardzo podobnie.

To właśnie jest jedna z największych zalet abstrakcji LangChain.

---

## 13. Ten sam interfejs, inny model

Możemy później wywołać model np.:

```python
response = llm.invoke(
    "Wyjaśnij czym jest Random Forest."
)

print(response.content)
```

Niezależnie od tego, czy pod spodem działa:

- OpenAI,
- Gemini,
- Gemma,
- Llama,
- inny model,

kod aplikacji może pozostać bardzo podobny.

---

## 14. Model lokalny a model chmurowy

Wykład pokazuje dwa główne podejścia.

### Model lokalny

Przykład:

```text
Gemma 3
+
Ollama
```

Schemat:

```text
Python
 ↓
LangChain
 ↓
Ollama
 ↓
lokalny model
```

### Model przez zewnętrzne API

W takim przypadku zwykle potrzebujemy klucza API.

Schemat:

```text
Python
 ↓
LangChain
 ↓
klient dostawcy
 ↓
API
 ↓
model w chmurze
```

---

## 15. API key

Przy dostawcach chmurowych zwykle:

1. zakładamy konto,
2. tworzymy klucz API,
3. zapisujemy go jako zmienną środowiskową,
4. konfigurujemy odpowiedniego klienta LangChain.

Przykładowo:

```bash
export SOME_API_KEY="..."
```

---

## 16. Dlaczego jakość modeli może się różnić?

Mały model lokalny może działać szybko, ale jego jakość odpowiedzi może być niższa.

Przykładowo:

```text
Gemma 3 270M
```

jest bardzo małym modelem.

Dlatego może:

- gorzej rozumieć skomplikowane polecenia,
- częściej popełniać błędy,
- generować mniej precyzyjne odpowiedzi.

Większy model zwykle zapewnia lepszą jakość, ale wymaga więcej zasobów.

---

## 17. Typowy kompromis

Przy lokalnych LLM-ach często wybieramy między:

```text
szybkością
        ↕
jakością
        ↕
zużyciem pamięci
```

Mały model:

```text
+ szybki
+ małe wymagania
- słabsza jakość
```

Duży model:

```text
+ lepsza jakość
- większe wymagania
- wolniejsze działanie
```

---

## 18. Open-weight nie oznacza automatycznie „lepszy”

Modele open-weight są bardzo użyteczne, ale nie oznacza to, że zawsze będą lepsze od modeli dostępnych przez API.

Lokalny model może być świetny, gdy zależy nam na:

- prywatności,
- niskich kosztach,
- pracy offline,
- pełnej kontroli.

Model chmurowy może być lepszy, gdy najważniejsza jest:

- wysoka jakość,
- duża zdolność rozumowania,
- łatwa skalowalność,
- brak konieczności utrzymywania sprzętu.

---

## 19. Dlaczego LangChain jest tutaj przydatny?

Bez dodatkowej warstwy abstrakcji zmiana dostawcy mogłaby wymagać przebudowy dużej części aplikacji.

LangChain pozwala ujednolicić sposób pracy.

```text
             ┌─ OpenAI
             ├─ Gemini
Aplikacja ───┼─ Ollama
             ├─ Anthropic
             └─ inne modele
```

A kod korzystający z `llm` pozostaje podobny.

---

## 20. Najważniejsza koncepcja z lekcji

Najważniejszy fragment można streścić tak:

```python
llm = ...
```

To właśnie ten obiekt określa, z jakiego modelu korzysta aplikacja.

Jeżeli reszta systemu jest dobrze zaprojektowana, zmiana modelu może sprowadzać się praktycznie do zmiany kilku linii.

---

## 21. Przykład: OpenAI → Ollama

### Wersja OpenAI

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="gpt-5"
)
```

### Wersja lokalna

```python
from langchain_ollama import ChatOllama

llm = ChatOllama(
    model="gemma3:270m",
    temperature=0
)
```

Dalszy kod:

```python
response = llm.invoke(
    "What is machine learning?"
)

print(response.content)
```

może pozostać praktycznie taki sam.

---

## 22. Podstawowe komendy Ollama

### Pomoc

```bash
ollama --help
```

### Lista lokalnych modeli

```bash
ollama list
```

### Pobieranie modelu

```bash
ollama pull gemma3:270m
```

### Uruchomienie modelu

```bash
ollama run gemma3:270m
```

---

## 23. Cały proces w skrócie

```text
Zainstaluj Ollama
       ↓
Wybierz model
       ↓
ollama pull
       ↓
ollama list
       ↓
ollama run
       ↓
sprawdź model w terminalu
       ↓
zainstaluj integrację LangChain
       ↓
ChatOllama(...)
       ↓
używaj modelu w aplikacji
```

---

## 24. Co warto zapamiętać?

1. **LangChain pozwala łatwo zmieniać modele LLM.**
2. **Ollama umożliwia uruchamianie modeli lokalnie.**
3. **Gemma 3 jest przykładem modelu open-weight.**
4. **Małe modele są szybsze i lżejsze, ale zwykle mają niższą jakość.**
5. **Zmiana dostawcy LLM nie musi oznaczać przebudowy całej aplikacji.**
6. **Najczęściej zmieniamy głównie klienta i nazwę modelu.**
7. **Lokalny model daje większą kontrolę i prywatność.**
8. **Model chmurowy często oferuje lepszą jakość i łatwiejszą skalowalność.**

---

## 25. Najważniejsza intuicja

> **LangChain oddziela logikę aplikacji od konkretnego modelu LLM.**

Dzięki temu możemy stosunkowo łatwo przełączać się między:

```text
GPT
Gemini
Gemma
Llama
innymi modelami
```

bez przepisywania całej aplikacji.

---

## 26. Jednozdaniowe podsumowanie

> **Ollama pozwala uruchomić model open-weight lokalnie, a LangChain sprawia, że podmiana takiego modelu zamiast OpenAI może sprowadzać się do zmiany kilku linii kodu.**
