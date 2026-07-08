# Prompt Engineering i Context Engineering

## Prompt Engineering

Prompt engineering to sztuka i nauka projektowania instrukcji dla modelu językowego tak, aby generował pożądane, spójne i użyteczne odpowiedzi.

### Elementy skutecznego promptu

1. **Rola systemowa** — kim model ma być (ekspert, asystent, recenzent).
2. **Kontekst zadania** — co model ma zrobić i w jakim formacie.
3. **Ograniczenia** — czego model nie powinien robić (halucynacje, wymyślanie faktów).
4. **Przykłady (few-shot)** — wzorcowe pary wejście–wyjście pokazujące oczekiwany format.
5. **Dane wejściowe użytkownika** — konkretne pytanie lub zadanie.

### Few-shot prompting

Technika polega na dołączeniu kilku przykładów rozwiązanych zadań przed właściwym pytaniem. Model uczy się wzorca z kontekstu bez aktualizacji wag.

Kiedy stosować:
- klasyfikacja tekstu,
- ekstrakcja strukturalna (JSON, tabele),
- tłumaczenie do określonego formatu,
- zadania z jasnym schematem odpowiedzi.

Pułapka: zbyt wiele przykładów zużywa tokeny kontekstu i może zepchnąć właściwe pytanie poza okno modelu.

### ReAct w kontekście promptów

ReAct (Reasoning + Acting) wymaga od agenta jawnego rozumowania przed akcją. W promptach systemowych definiuje się format:

```
Thought: [rozumowanie]
Action: [nazwa narzędzia]
Action Input: [argumenty]
Observation: [wynik — wstawiany przez runtime]
... (powtarzane)
Final Answer: [odpowiedź dla użytkownika]
```

W nowoczesnych implementacjach z natywnym tool calling model nie generuje tego formatu tekstowego — zamiast tego zwraca strukturalne wywołanie narzędzia. Koncepcja Thought/Action/Observation pozostaje jednak użyteczna przy debugowaniu i projektowaniu przepływu agenta.

## Context Engineering

Context engineering to świadome zarządzanie tym, **co i w jakiej kolejności** trafia do okna kontekstowego modelu. W systemach agentowych i RAG to często ważniejsze niż sam prompt systemowy.

### Co konkuruje o tokeny kontekstu

- instrukcja systemowa,
- historia konwersacji,
- fragmenty dokumentów z RAG,
- opisy dostępnych narzędzi,
- wyniki poprzednich wywołań narzędzi (observations),
- few-shot examples.

### Strategie zarządzania kontekstem

**Podsumowywanie historii** — po N wiadomościach starsza część rozmowy jest kompresowana przez LLM do krótkiego streszczenia, zamiast przekazywania pełnej historii.

**Selekcja dokumentów RAG** — zwracanie tylko top-k najtrafniejszych chunków, z filtrowaniem po metadanych (data, źródło, kategoria).

**Pruning tool results** — obcinanie zbyt długich wyników narzędzi przed dołączeniem do kontekstu (np. tylko pierwsze 500 znaków odpowiedzi API).

**Hierarchia priorytetów** — instrukcja systemowa i bieżące pytanie użytkownika mają pierwszeństwo; historia i dokumenty są skracane w razie potrzeby.

### Pułapki context engineeringu

- **Lost in the middle** — modele gorzej wykorzystują informacje ze środka długiego kontekstu; kluczowe dane umieszczaj na początku lub końcu.
- **Zanieczyszczenie kontekstu** — nieistotne fragmenty RAG lub błędne wyniki narzędzi prowadzą model na manowce.
- **Nadmierne few-shot** — przykłady wypierają miejsce na dokumenty lub historię.
- **Brak limitu iteracji agenta** — każda iteracja dokłada Thought/Action/Observation do kontekstu, szybko wyczerpując okno.

## Pamięć a context engineering

Systemy pamięci w LangChain dzielą się na:

- **ConversationBufferMemory** — pełna historia (proste, ale rośnie bez ograniczeń).
- **ConversationSummaryMemory** — historia kompresowana do podsumowania.
- **VectorStoreRetrieverMemory** — relewantne wspomnienia pobierane semantycznie przy każdym zapytaniu.

Wybór typu pamięci to decyzja context engineeringowa: co agent „pamięta" między turami i ile tego mieści się w kontekście.

## Praktyczne wskazówki z kursu

- Zaczynaj od prostego promptu systemowego i iteruj na podstawie błędów modelu.
- Testuj z realnymi danymi użytkowników, nie tylko z „szczęśliwymi ścieżkami".
- Przy RAG mierz jakość retrievera osobno od jakości generacji — słaby retriever nie naprawi go nawet najlepszy prompt.
- Ustal maksymalną liczbę tokenów na wynik narzędzia i egzekwuj ją w kodzie.
- Dokumentuj wersje promptów — zmiana jednego zdania może drastycznie zmienić zachowanie agenta.
