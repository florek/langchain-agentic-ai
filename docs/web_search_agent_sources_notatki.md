# Web Search jako narzędzie agenta — wyszukiwanie, źródła i aktualne dane

## 1. Cel lekcji

W tej części wykładu pokazany jest agent korzystający z:

## web search

czyli wyszukiwania informacji w internecie.

Najważniejsza idea jest taka:

> **LLM sam z siebie nie ma dostępu do aktualnych informacji z internetu, ale może dostać narzędzie, które pozwoli mu takie informacje wyszukać.**

---

## 2. Przykład w ChatGPT

W demonstracji użytkownik korzysta z ChatGPT i włącza:

```text
web search
```

Następnie zadaje pytanie, które wymaga aktualnych danych.

Przykład:

```text
Znajdź 3 oferty pracy dla AI Engineera na LinkedIn.
```

Tego typu zadanie wymaga wyszukania aktualnych informacji.

---

## 3. Kiedy potrzebny jest web search?

Jeżeli pytanie dotyczy danych, które mogą się zmieniać, model nie powinien polegać wyłącznie na wiedzy zapisanej podczas treningu.

Przykłady:

```text
aktualne oferty pracy
aktualne ceny
wyniki sportowe
nowe wiadomości
rozkład lotów
dostępność produktów
```

W takich sytuacjach agent może użyć narzędzia wyszukującego internet.

---

## 4. LLM + tool

Schemat działania wygląda tak:

```text
użytkownik
   ↓
LLM
   ↓
czy potrzebuję aktualnych danych?
   ↓
TAK
   ↓
web search tool
   ↓
wyniki wyszukiwania
   ↓
LLM
   ↓
odpowiedź
```

To typowy przykład systemu agentowego.

---

## 5. LLM decyduje o użyciu narzędzia

Najważniejszy element:

> Model może zdecydować, że samo generowanie odpowiedzi nie wystarczy.

Wtedy wybiera odpowiedni tool.

W tym przypadku:

```text
web search
```

---

## 6. Przykład: oferty pracy AI Engineer

Agent może znaleźć kilka ofert pracy.

Przy każdej z nich może pokazać np.:

- nazwę stanowiska,
- firmę,
- lokalizację,
- opis,
- wymagane technologie,
- link do źródła.

W materiale pojawiają się m.in. technologie takie jak:

```text
LangChain
LlamaIndex
RAG
Generative AI
```

---

## 7. Dlaczego źródła są tak ważne?

Bardzo istotnym elementem odpowiedzi są:

## source URLs

czyli linki do źródeł.

Bez źródeł użytkownik otrzymuje jedynie tekst wygenerowany przez model.

Z linkiem może sprawdzić:

> Czy dana informacja rzeczywiście pochodzi z istniejącego źródła?

---

## 8. Problem halucynacji

LLM-y mogą generować odpowiedzi, które:

- brzmią poprawnie,
- są logiczne,
- wyglądają wiarygodnie,

ale nie muszą być prawdziwe.

To zjawisko nazywamy:

## hallucination — halucynacja

Dlatego w systemach wyszukujących aktualne dane bardzo ważne jest pokazywanie źródeł.

---

## 9. Źródło pozwala zweryfikować odpowiedź

Jeżeli agent zwraca:

```text
informacja
+
URL źródła
```

użytkownik może:

1. otworzyć link,
2. sprawdzić oryginalną treść,
3. zweryfikować odpowiedź modelu.

To znacząco zwiększa użyteczność systemu.

---

## 10. Dlaczego jest to ważne w systemach agentowych?

Agent może podejmować decyzje na podstawie danych z internetu.

Jeżeli dane są nieprawidłowe, kolejne kroki również mogą być błędne.

Dlatego:

```text
źródła
+
możliwość weryfikacji
```

są bardzo istotne.

---

## 11. Generative UI

W demonstracji pokazano również elementy interfejsu generowane dynamicznie na podstawie odpowiedzi.

Autor określa to jako:

## Generative UI

Czyli interfejs użytkownika jest tworzony lub dostosowywany w zależności od tego, co agent aktualnie wykonuje.

Przykładowo:

```text
wyniki wyszukiwania
→ karty ofert pracy
→ logo serwisu
→ link
→ opis
```

---

## 12. Odpowiedź to nie tylko tekst

Nowoczesna aplikacja AI może zwracać:

- tekst,
- linki,
- karty,
- przyciski,
- obrazy,
- inne elementy UI.

Dlatego interakcja z agentem może być znacznie bogatsza niż zwykła odpowiedź tekstowa.

---

## 13. LLM ma wiedzę statyczną

Ważny fragment wykładu:

> Wiedza modelu jest zasadniczo statyczna względem danych, na których został wytrenowany.

Model nie ma automatycznie dostępu do internetu.

Nie wie sam z siebie, co wydarzyło się po zakończeniu zbierania danych treningowych.

---

## 14. Model nie „patrzy do internetu”

Sam LLM nie wykonuje automatycznie:

```text
Google search
LinkedIn search
API call
database query
```

Potrzebuje do tego:

## toola

Czyli zewnętrznej funkcji lub systemu.

---

## 15. Wiedza modelu vs aktualne dane

Możemy rozdzielić dwa rodzaje informacji:

### Wiedza modelu

```text
informacje nauczone podczas treningu
```

### Aktualne dane

```text
informacje pobrane z zewnętrznego źródła
```

Schemat:

```text
LLM
+
web search
=
model z dostępem do aktualnych informacji
```

---

## 16. Dlaczego web search stał się standardem?

Autor przypomina, że we wcześniejszych latach systemy takie jak ChatGPT nie miały powszechnie dostępnego wyszukiwania internetowego.

Z czasem web search stał się standardową funkcją wielu aplikacji AI.

To pokazuje zmianę:

```text
LLM generujący tylko tekst
```

w kierunku:

```text
LLM + tools + aktualne dane
```

---

## 17. Agent wyszukujący informacje

W dalszej części kursu ma zostać zbudowany:

## search agent

Czyli agent, który:

```text
1. otrzymuje pytanie
2. ocenia, czy potrzebuje internetu
3. wykonuje wyszukiwanie
4. analizuje wyniki
5. wybiera istotne informacje
6. generuje odpowiedź
7. podaje źródła
```

---

## 18. To nie jest tylko zwykłe wyszukiwanie

Klasyczna wyszukiwarka zwraca:

```text
listę linków
```

Agent może zrobić więcej:

```text
wyszukać
+
przeczytać
+
porównać
+
wybrać
+
podsumować
+
podać źródła
```

To właśnie zwiększa możliwości systemu.

---

## 19. Przykładowy workflow agenta wyszukującego

```text
USER
 ↓
"Znajdź 3 oferty AI Engineer"
 ↓
LLM
 ↓
web search
 ↓
wyniki
 ↓
LLM analizuje
 ↓
wybiera 3 oferty
 ↓
tworzy podsumowanie
 ↓
dodaje linki źródłowe
 ↓
ODPOWIEDŹ
```

---

## 20. Dlaczego agent potrzebuje narzędzia?

Bez web search model mógłby:

- zgadywać,
- podać nieaktualne informacje,
- stworzyć nieistniejące oferty.

Z narzędziem może oprzeć odpowiedź na aktualnych danych.

---

## 21. Źródła jako część odpowiedzi

Dobra odpowiedź agenta wyszukującego powinna zawierać:

```text
treść odpowiedzi
+
źródło
```

Dzięki temu użytkownik może szybko zweryfikować dane.

---

## 22. Relacja do wcześniejszego materiału o agentach

Ten przykład dobrze pokazuje definicję agenta:

> LLM decyduje, że potrzebuje narzędzia, używa go i na podstawie wyniku wykonuje kolejny krok.

Czyli:

```text
Reason
 ↓
Act: web search
 ↓
Observation
 ↓
Reason
 ↓
Final answer
```

To jest bardzo bliskie architekturze:

## ReAct

---

## 23. Web search jako tool

Możemy więc traktować wyszukiwarkę jako jeden z wielu możliwych tools.

Inne mogłyby być:

```text
calculator
database
email
calendar
Python
API
file system
```

Web search jest po prostu jednym konkretnym przykładem.

---

## 24. Najważniejszy cel tej części kursu

W dalszej części kursu celem będzie nauczenie się:

> Jak podłączyć narzędzie wyszukiwania do LLM i zbudować agenta, który sam zdecyduje, kiedy go użyć.

---

## 25. Co warto zapamiętać?

1. **LLM sam z siebie nie ma bieżącego dostępu do internetu.**
2. **Web search można udostępnić modelowi jako tool.**
3. **Agent może sam zdecydować, kiedy wyszukiwanie jest potrzebne.**
4. **Źródła są ważne, ponieważ pozwalają zweryfikować odpowiedź.**
5. **LLM-y mogą halucynować, dlatego aktualne dane powinny być sprawdzalne.**
6. **Search agent może nie tylko znaleźć linki, ale też analizować i podsumowywać wyniki.**
7. **Web search jest typowym przykładem działania agenta ReAct.**
8. **Generative UI pozwala prezentować wyniki w bogatszej formie niż sam tekst.**
9. **Nowoczesne aplikacje AI coraz częściej łączą LLM-y z zewnętrznymi narzędziami.**
10. **Celem jest połączenie możliwości rozumowania LLM z dostępem do aktualnych danych.**

---

## 26. Całość w jednym schemacie

```text
PYTANIE UŻYTKOWNIKA
        ↓
       LLM
        ↓
czy potrzebuję aktualnych danych?
        ↓
       TAK
        ↓
   WEB SEARCH TOOL
        ↓
      WYNIKI
        ↓
       LLM
        ↓
 analiza + podsumowanie
        ↓
   ODPOWIEDŹ + ŹRÓDŁA
```

---

## 27. Najważniejsze zdanie

> **Web search daje agentowi dostęp do aktualnych informacji, a źródła pozwalają użytkownikowi sprawdzić, czy odpowiedź modelu rzeczywiście opiera się na realnych danych.**
