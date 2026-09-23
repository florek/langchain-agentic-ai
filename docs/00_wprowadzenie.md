# Wprowadzenie do kursu Agentic AI

Ten materiał utrwala pojęcia ramowe kursu **Agentic AI Engineering with LangChain and LangGraph**: czym różnią się aplikacje LLM, RAG i agenci oraz jakie elementy ekosystemu GenAI pojawiają się w dalszej nauce.

---

## 1. Trzy poziomy aplikacji LLM

W praktyce GenAI warto rozróżniać trzy podejścia:

| Podejście | Istota | Typowe zastosowanie |
|-----------|--------|---------------------|
| Klasyczny LLM | Jednorazowe wywołanie modelu z promptem | streszczenie, klasyfikacja, generowanie tekstu |
| RAG | Model + wyszukiwanie w bazie wiedzy (często wektorowej) | odpowiedzi oparte o dokumenty / dokumentację |
| Agenci | Model planuje kroki, może używać narzędzi i pamięci | zadania wymagające działań wieloetapowych |

**Agentic AI** oznacza systemy, w których model nie tylko generuje tekst, ale **decyduje o kolejnych krokach**, korzysta z narzędzi (tool calling) i utrzymuje kontekst (memory).

---

## 2. LangChain a LangGraph

- **LangChain** — biblioteka i ekosystem do składania aplikacji LLM: prompty, modele chat, łańcuchy Runnable, integracje z narzędziami i pamięcią.
- **LangGraph** — warstwa do budowania **przepływów agentowych** (grafy stanów, pętle, rozgałęzienia) — szczególnie przy multi-agent i złożonej kontroli przebiegu.

Kurs korzysta z LangChain w wersji **1.x+** oraz aktualnego ekosystemu LangGraph.

---

## 3. Prompt engineering i context engineering

**Prompt engineering** — projektowanie instrukcji i formatu wejścia tak, by model realizował zadanie przewidywalnie (np. struktura odpowiedzi: podsumowanie + fakty).

**Context engineering** — zarządzanie tym, *co* trafia do kontekstu modelu: które dokumenty, historia rozmowy, wyniki narzędzi, limity okna kontekstu.

Powiązane wzorce:

- **few-shot prompting** — przykłady w prompcie pokazujące oczekiwany format lub styl,
- **ReAct prompting** — wzorzec „rozumowanie + działanie” (thought / action / observation) stosowany przy agentach z narzędziami.

---

## 4. Budulce systemów agentowych

W dalszej części kursu pojawiają się m.in.:

- **tool calling** — model wybiera i wywołuje zewnętrzne funkcje / API,
- **RAG + vector databases** (np. FAISS, Pinecone, Chroma) — wyszukiwanie semantyki w dokumentach,
- **memory systems** — pamięć krótkoterminowa (historia) i dłuższa (trwałe fakty),
- **LangSmith** — tracing i obserwowalność przebiegów LLM,
- **Model Context Protocol (MCP)** — standard udostępniania narzędzi i kontekstu modelom,
- **deep agents** — agenci o głębszym planowaniu i wieloetapowym rozwiązywaniu zadań,
- systemy **multi-agentowe** — współpraca kilku agentów o różnych rolach.

---

## 5. Wymagania wstępne (nie ML)

Do przerabiania materiału wystarczy solidna podstawa: Python, OOP, terminal, Git, venv, zmienne środowiskowe, debugowanie. **Wcześniejsza znajomość Machine Learningu nie jest wymagana** — nacisk leży na inżynierii aplikacji LLM i agentów.

---

## 6. Pułapki koncepcyjne

- Mylenie RAG z agentem: RAG dostarcza wiedzę z dokumentów; agent dodatkowo **steruje przebiegiem** i narzędziami.
- Traktowanie każdego wywołania LLM jako „agenta” — bez narzędzi, pętli decyzji i pamięci to zwykle klasyczny pipeline prompt → model.
- Ignorowanie context engineeringu: nawet dobry prompt zawodzi, gdy do modelu trafia zbędny lub zbyt długi kontekst.
