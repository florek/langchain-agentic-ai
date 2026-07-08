# Agentic AI — wprowadzenie

## Czym jest Agentic AI

Agentic AI to podejście do budowania aplikacji opartych o modele językowe (LLM), w którym model nie tylko generuje tekst, lecz **podejmuje decyzje i wykonuje działania** w pętli: obserwacja → rozumowanie → akcja. Agent może korzystać z narzędzi zewnętrznych (wyszukiwarka, baza danych, API), pamiętać kontekst rozmowy i iteracyjnie dążyć do celu użytkownika.

## Różnica: LLM vs RAG vs Agent

| Podejście | Opis | Kiedy stosować |
|-----------|------|----------------|
| **Klasyczny LLM** | Model odpowiada wyłącznie na podstawie wiedzy z treningu i promptu | Proste Q&A, generowanie tekstu bez aktualnych danych |
| **RAG** (Retrieval-Augmented Generation) | Model otrzymuje fragmenty dokumentów pobrane z bazy wektorowej przed wygenerowaniem odpowiedzi | Odpowiedzi oparte o własną dokumentację, aktualne dane |
| **Agent** | Model sam decyduje, które narzędzia wywołać i w jakiej kolejności, by osiągnąć cel | Złożone zadania wymagające wielu kroków, integracji z systemami |

## Kluczowe pojęcia kursu

- **LangChain** — framework do budowania aplikacji LLM: łańcuchy, agenci, integracje z modelami i narzędziami.
- **LangGraph** — biblioteka do definiowania przepływów agentowych jako grafów stanów z warunkowymi przejściami.
- **Tool Calling** — mechanizm, w którym LLM zwraca strukturalne żądanie wywołania funkcji zamiast samego tekstu.
- **ReAct** (Reasoning + Acting) — wzorzec promptowania, w którym agent na przemian rozumuje (Thought) i działa (Action/Observation).
- **Prompt Engineering** — projektowanie instrukcji systemowych i szablonów promptów dla lepszych odpowiedzi.
- **Context Engineering** — świadome zarządzanie tym, co trafia do okna kontekstowego modelu (historia, dokumenty, wyniki narzędzi).
- **LangSmith** — platforma do śledzenia, debugowania i ewaluacji aplikacji LangChain.
- **MCP** (Model Context Protocol) — standardowy protokół łączenia modeli z zewnętrznymi źródłami kontekstu i narzędziami.
- **Deep Agents** — zaawansowane architektury agentowe z wielopoziomowym planowaniem i delegowaniem zadań.

## Architektury agentowe

### Pojedynczy agent
Jeden model z dostępem do zestawu narzędzi. Prosty w implementacji, ale ograniczony przy bardzo złożonych zadaniach.

### System multi-agentowy
Wiele wyspecjalizowanych agentów współpracuje: jeden planuje, inny wyszukuje, trzeci pisze raport. LangGraph ułatwia modelowanie takich przepływów jako graf z węzłami i krawędziami.

## Bazy wektorowe w ekosystemie RAG

W kursie pojawiają się trzy popularne rozwiązania:

- **FAISS** — biblioteka do szybkiego wyszukiwania podobieństwa, działa lokalnie w pamięci lub na dysku.
- **Chroma** — lekka baza wektorowa z prostym API, dobra na start i prototypy.
- **Pinecone** — zarządzana usługa chmurowa, skalowalna produkcyjnie.

Wspólny wzorzec RAG: dokumenty → podział na fragmenty (chunking) → embeddingi → indeks wektorowy → przy zapytaniu wyszukanie podobnych fragmentów → dołączenie do promptu → odpowiedź LLM.

## Pamięć w systemach agentowych

- **Pamięć krótkoterminowa** — historia bieżącej konwersacji w oknie kontekstowym.
- **Pamięć długoterminowa** — trwałe przechowywanie faktów o użytkowniku lub sesji (np. w bazie danych lub wektorowej).
- Ograniczenie: każdy model ma limit tokenów kontekstu — dlatego context engineering i podsumowywanie historii są kluczowe.

## Few-shot prompting

Technika polegająca na dołączeniu do promptu kilku przykładów par pytanie–odpowiedź, aby model lepiej zrozumiał oczekiwany format i styl odpowiedzi. Szczególnie skuteczna przy zadaniach strukturalnych i klasyfikacji.

## Cel repozytorium szkoleniowego

Repozytorium służy do praktycznego utrwalania wiedzy z kursu *Agentic AI Engineering with LangChain and LangGraph*. Nie jest gotowym produktem — jest miejscem eksperymentów, notatek i ćwiczeń rozwijanych lekcja po lekcji.
