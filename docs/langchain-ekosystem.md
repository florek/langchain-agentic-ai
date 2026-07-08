# LangChain — ekosystem i podstawy

## Czym jest LangChain

LangChain to framework open-source do budowania aplikacji opartych o modele językowe. W wersji 1.x+ oferuje ujednolicony interfejs do pracy z różnymi dostawcami LLM, narzędziami, pamięcią i łańcuchami przetwarzania. Kurs korzysta z aktualnej wersji 1.x+.

## Warstwy abstrakcji

### Modele (LLMs / Chat Models)

LangChain rozróżnia:

- **LLM** — modele tekstowe przyjmujące i zwracające surowy string.
- **Chat Models** — modele konwersacyjne operujące na listach wiadomości (`SystemMessage`, `HumanMessage`, `AIMessage`).

W praktyce nowoczesne API (OpenAI, Ollama) używa się głównie przez Chat Models.

### Prompty (Prompt Templates)

Szablony promptów pozwalają parametryzować instrukcje — zamiast konkatenacji stringów definiuje się szablon z placeholderami (np. `{question}`, `{context}`), który jest wypełniany danymi w runtime.

### Łańcuchy (Chains)

Łańcuch to sekwencja kroków: prompt → model → parser wyjścia. W LangChain 1.x koncepcja łańcuchów ewoluowała w kierunku **LangChain Expression Language (LCEL)** — deklaratywnego składania komponentów operatorem `|`.

Przykład idei LCEL:

```
chain = prompt | model | output_parser
result = chain.invoke({"question": "..."})
```

### Agenci (Agents)

Agent to pętla, w której model:

1. Otrzymuje zadanie i listę dostępnych narzędzi.
2. Decyduje, czy odpowiedzieć bezpośrednio, czy wywołać narzędzie.
3. Otrzymuje wynik narzędzia (observation) i kontynuuje do osiągnięcia celu.

Agent kończy pracę, gdy model zwróci odpowiedź końcową lub osiągnięty zostanie limit iteracji.

## LangGraph — grafy stanów

LangGraph rozszerza LangChain o modelowanie przepływów jako **grafów stanów** (state machines):

- **Węzeł (node)** — funkcja przetwarzająca stan (np. wywołanie LLM, narzędzia).
- **Krawędź (edge)** — przejście między węzłami, może być warunkowa.
- **Stan (state)** — współdzielony słownik/obiekt przekazywany między węzłami (historia wiadomości, wyniki pośrednie).

Zalety LangGraph nad prostą pętlą agenta:

- jawna kontrola przepływu (rozgałęzienia, pętle, checkpointy),
- wsparcie dla systemów multi-agentowych,
- możliwość wznawiania od checkpointu (persystencja stanu),
- łatwiejsze debugowanie dzięki przejrzystemu grafowi.

## Tool Calling

Nowoczesne modele (GPT-4, Claude, Llama 3+) obsługują **structured tool calling** — model zwraca JSON z nazwą funkcji i argumentami zamiast tekstu opisującego akcję.

W LangChain narzędzia definiuje się dekoratorem `@tool` lub klasą dziedziczącą po `BaseTool`. Agent otrzymuje opisy narzędzi w formacie zrozumiałym dla modelu i mapuje odpowiedź modelu na wywołanie Pythona.

Typowy cykl:

1. Użytkownik: „Jaka jest pogoda w Warszawie?"
2. Model: `tool_call(name="get_weather", args={"city": "Warsaw"})`
3. Runtime wykonuje funkcję i zwraca wynik.
4. Model generuje odpowiedź naturalną na podstawie wyniku.

## ReAct prompting

ReAct to wzorzec, w którym agent explicite przeplata:

- **Thought** — wewnętrzne rozumowanie,
- **Action** — wywołanie narzędzia,
- **Observation** — wynik narzędzia.

LangChain implementuje ReAct zarówno przez klasyczne prompty tekstowe, jak i przez natywne tool calling wspierane przez model. Natywne tool calling jest preferowane tam, gdzie model je obsługuje — mniej halucynacji formatu i lepsza niezawodność.

## RAG w LangChain

Typowy pipeline RAG w ekosystemie LangChain:

1. **Document Loaders** — wczytanie plików PDF, HTML, CSV itd.
2. **Text Splitters** — podział na chunki o kontrolowanym rozmiarze z nakładaniem (overlap).
3. **Embeddings** — wektoryzacja chunków modelem embeddingowym.
4. **Vector Store** — indeksowanie i wyszukiwanie podobieństwa (FAISS, Chroma, Pinecone).
5. **Retriever** — komponent zwracający najtrafniejsze dokumenty dla zapytania.
6. **Chain** — prompt z kontekstem + model generujący odpowiedź.

Kluczowa decyzja projektowa: rozmiar chunków i strategia podziału wpływają na jakość odpowiedzi bardziej niż sam wybór modelu.

## LangSmith

LangSmith to platforma do:

- **Tracing** — śledzenie każdego wywołania łańcucha/agenta (input, output, czas, tokeny),
- **Debugging** — analiza, dlaczego agent podjął daną decyzję,
- **Evaluation** — automatyczne testy jakości odpowiedzi na zestawie przykładów,
- **Monitoring** — obserwacja aplikacji produkcyjnej.

Integracja wymaga ustawienia zmiennych `LANGCHAIN_TRACING_V2=true` i `LANGCHAIN_API_KEY`.

## Model Context Protocol (MCP)

MCP to otwarty standard łączenia LLM z zewnętrznymi źródłami danych i narzędziami przez ujednolicony protokół. Zamiast pisać integrację dla każdego API osobno, serwer MCP udostępnia zasoby i narzędzia w standardowym formacie, który klient (np. agent LangChain) może odkrywać dynamicznie.

## Deep Agents

Deep Agents to koncepcja wielopoziomowych agentów z:

- planowaniem zadań na wysokim poziomie,
- delegowaniem podzadań wyspecjalizowanym agentom,
- refleksją i korektą planu na podstawie wyników pośrednich.

LangGraph jest naturalnym narzędziem do implementacji takich architektur dzięki grafom z wieloma węzłami i warunkowymi przejściami.

## Najlepsze praktyki

- Zawsze ustawiaj `temperature` świadomie: niska (0–0.3) dla zadań faktycznych, wyższa dla kreatywnych.
- Ograniczaj liczbę iteracji agenta (`max_iterations`), aby uniknąć nieskończonych pętli.
- Loguj i śledź wywołania (LangSmith) od początku projektu, nie dopiero w produkcji.
- Testuj prompty na reprezentatywnych przykładach przed wdrożeniem.
- Separuj konfigurację (klucze API, nazwy modeli) od logiki aplikacji.
