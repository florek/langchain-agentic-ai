# PromptTemplate, modele chat i łańcuch LCEL

Ten materiał utrwala pierwszy praktyczny wzorzec aplikacji LLM w LangChain: **szablon promptu → model chat → wywołanie łańcucha** oraz przełączanie między modelem w chmurze a lokalnym.

---

## 1. Zmienne środowiskowe (`python-dotenv`)

Przed utworzeniem klienta modelu ładuje się zmienne z pliku `.env` przez `load_dotenv()`. Dzięki temu klucz API (np. OpenAI) nie trafia do kodu źródłowego, tylko do środowiska procesu.

**Pułapka:** bez załadowanych zmiennych wywołanie `ChatOpenAI` kończy się błędem braku klucza lub nieautoryzowanym dostępem.

---

## 2. `PromptTemplate`

`PromptTemplate` z `langchain_core.prompts` buduje tekst promptu z:

- **`template`** — string z placeholderami w stylu `{information}`,
- **`input_variables`** — lista nazw zmiennych, które muszą zostać podane przy wywołaniu.

Przykład koncepcyjny: prompt „na podstawie `{information}` przygotuj krótkie podsumowanie i dwa ciekawe fakty” — model dostaje ustrukturyzowane zadanie zamiast surowego bloku tekstu bez instrukcji.

**Dlaczego szablon, a nie zwykły f-string?** Szablon jest elementem Runnable: da się go złożyć w łańcuch z modelem i wielokrotnie wywoływać z różnymi danymi wejściowymi.

---

## 3. Modele chat: `ChatOpenAI` i `ChatOllama`

| Integracja | Pakiet | Skąd bierze model |
|------------|--------|-------------------|
| `ChatOpenAI` | `langchain-openai` | API OpenAI w chmurze |
| `ChatOllama` | `langchain-ollama` | lokalny serwer Ollama |

Wspólne parametry praktyczne:

- **`model`** — nazwa modelu (np. model OpenAI lub tag modelu Ollama),
- **`temperature`** — losowość odpowiedzi; **`temperature=0`** dąży do bardziej deterministycznych, powtarzalnych wyników (przydatne przy streszczeniach i faktach).

**Dobra praktyka:** ten sam prompt i łańcuch można podpiąć pod inny backend LLM — zmienia się obiekt modelu, nie logika zadania.

---

## 4. LCEL: operator `|` i łańcuch Runnable

LangChain Expression Language (LCEL) łączy komponenty operatorem pipe:

```text
łańcuch = prompt_template | llm
```

Powstaje obiekt **Runnable**: najpierw wypełnia szablon, potem przekazuje wynik do modelu. To podstawowy wzorzec „prompt → model” bez ręcznego sklejania stringów i osobnych wywołań API.

---

## 5. `invoke` i treść odpowiedzi

Wywołanie synchroniczne:

```text
response = chain.invoke({"information": ...})
```

- argumentem jest **słownik** dopasowany do `input_variables` szablonu,
- odpowiedź modelu chat ma atrybut **`.content`** z wygenerowanym tekstem.

**Pułapki:**

- niezgodność kluczy w słowniku z `input_variables` → błąd walidacji wejścia,
- drukowanie całego obiektu odpowiedzi zamiast `.content` — widać metadane, nie sam tekst użytkownika,
- wysoka `temperature` przy zadaniach wymagających stabilnego formatu (np. stała struktura „summary + facts”).

---

## 6. Minimalny przepływ end-to-end

1. Załaduj środowisko (`load_dotenv`).
2. Zdefiniuj `PromptTemplate` z instrukcją i placeholderami.
3. Utwórz LLM (`ChatOpenAI` lub `ChatOllama`) z wybranym `model` i `temperature`.
4. Złóż łańcuch: `prompt | llm`.
5. Wywołaj `invoke` ze słownikiem wejściowym i odczytaj `response.content`.

Ten wzorzec jest bazą pod kolejne lekcje: narzędzia, pamięć, RAG i grafy agentowe buduje się na tych samych ideach Runnable i jasnego kontraktu wejścia/wyjścia.
