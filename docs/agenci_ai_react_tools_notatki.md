# Agenci AI — definicja, narzędzia i architektura ReAct

## 1. Czym jest agent AI?

Nie istnieje jedna, powszechnie przyjęta definicja agenta AI.

Różne osoby mogą definiować agentów trochę inaczej.

W tym wykładzie przyjmujemy następującą intuicję:

> **Agent to system programowy, który używa LLM do decydowania, jakie działania należy wykonać, a następnie te działania wykonuje.**

Najważniejszy element definicji:

> **LLM decyduje, co zrobić dalej.**

To właśnie odróżnia agenta od zwykłego chaina.

---

# 2. Agent vs chain

W prostym chainie kolejność działań jest z góry ustalona przez programistę.

Przykład:

```text
1. pobierz dane
2. podsumuj tekst
3. wygeneruj odpowiedź
```

Programista definiuje cały control flow.

LLM może pojawić się w jednym z kroków, ale nie decyduje, co nastąpi później.

---

## 3. W chainie flow jest hardcoded

Przykładowo:

```text
input
  ↓
LLM: podsumowanie
  ↓
parser
  ↓
wynik
```

Kolejność jest ustalona wcześniej.

Model nie może sam zdecydować:

```text
"najpierw wyszukam dane"
```

albo:

```text
"muszę użyć API"
```

---

# 4. W agencie LLM steruje kolejnymi krokami

W agencie sytuacja wygląda inaczej.

Model może zdecydować:

```text
co zrobić dalej?
```

Na przykład:

```text
pytanie użytkownika
        ↓
LLM
        ↓
"muszę wyszukać dane"
        ↓
tool: search
        ↓
wynik
        ↓
LLM
        ↓
"teraz mogę odpowiedzieć"
```

To dynamiczne podejmowanie decyzji jest podstawą działania agenta.

---

# 5. Najważniejsza różnica

Możemy uprościć różnicę tak:

## Chain

```text
programista decyduje o kolejności
```

## Agent

```text
LLM decyduje o kolejności
```

To jest kluczowa różnica.

---

# 6. Tools — narzędzia

Najbardziej typowym sposobem budowania agentów jest wyposażenie LLM w:

## tools

czyli narzędzia.

Narzędzie to funkcja, którą model może wywołać.

Przykłady:

- wykonanie zapytania do API,
- wyszukiwanie w internecie,
- odczyt z bazy danych,
- zapis do bazy danych,
- uruchomienie kodu,
- wykonanie obliczeń.

---

# 7. Agent dostaje dodatkowe możliwości

Sam LLM przede wszystkim generuje tekst.

Dzięki tools możemy dać mu możliwość:

```text
czytania danych
wyszukiwania
wywoływania API
uruchamiania kodu
wykonywania akcji
```

Można powiedzieć:

> Tools dają LLM możliwość działania poza samym generowaniem tekstu.

---

# 8. Przykład

Użytkownik pyta:

```text
Jaka jest dziś pogoda w Londynie?
```

Sam model może nie znać aktualnej pogody.

Agent może zrobić:

```text
1. zrozum pytanie
2. zdecyduj, że potrzebne jest API pogodowe
3. wywołaj tool
4. pobierz wynik
5. odpowiedz użytkownikowi
```

---

# 9. Tools jako wcześniej napisane funkcje

W praktyce tools są zwykle funkcjami napisanymi przez programistę.

Przykładowo:

```python
def get_weather(city):
    ...
```

albo:

```python
def search_database(query):
    ...
```

Agent dostaje informację:

> masz dostęp do takich funkcji i możesz ich używać.

---

# 10. LLM wybiera narzędzie

To bardzo ważne.

Programista przygotowuje narzędzia, ale:

> model może zdecydować, którego z nich użyć.

Przykład:

```text
tools:
- search_web
- calculator
- database
```

LLM analizuje zadanie i może wybrać:

```text
calculator
```

albo:

```text
search_web
```

w zależności od problemu.

---

# 11. ReAct

Jedną z najważniejszych architektur agentowych jest:

## ReAct

Nazwa pochodzi od:

```text
Reasoning
+
Acting
```

czyli:

```text
rozumowanie
+
działanie
```

---

# 12. Idea ReAct

Agent działa w cyklu:

```text
myślenie
  ↓
decyzja
  ↓
działanie
  ↓
obserwacja wyniku
  ↓
kolejne myślenie
```

I powtarza tę pętlę aż do rozwiązania problemu.

---

# 13. Przykład ReAct

Załóżmy pytanie:

```text
Ile lat ma obecny CEO firmy X?
```

Agent może wykonać:

```text
Reason:
muszę ustalić, kto jest CEO

Act:
wyszukaj CEO firmy X

Observation:
CEO to osoba Y

Reason:
muszę znaleźć wiek osoby Y

Act:
wyszukaj wiek osoby Y

Observation:
Y ma 52 lata

Reason:
mam odpowiedź

Final:
CEO firmy X ma 52 lata
```

---

# 14. Iteracyjna pętla

Agent nie musi znać całego planu od początku.

Może działać krok po kroku:

```text
LLM
 ↓
tool
 ↓
wynik
 ↓
LLM
 ↓
tool
 ↓
wynik
 ↓
LLM
 ↓
final answer
```

To właśnie daje agentom elastyczność.

---

# 15. Reasoning

Pierwsza część ReAct to:

## reasoning

Model analizuje problem i zastanawia się:

```text
czego potrzebuję?
```

albo:

```text
jaki powinien być następny krok?
```

---

# 16. Acting

Druga część to:

## acting

Po podjęciu decyzji agent wykonuje akcję.

Przykład:

```text
call API
```

albo:

```text
search database
```

albo:

```text
run Python
```

---

# 17. Observation

Po wykonaniu toola agent otrzymuje wynik.

To jest:

## observation

Przykład:

```text
tool result:
Warsaw temperature = 12°C
```

Ten wynik trafia z powrotem do LLM.

---

# 18. Kolejna decyzja

Model analizuje observation i decyduje:

```text
czy zadanie jest już zakończone?
```

Jeżeli nie:

```text
wykonaj kolejną akcję
```

Jeżeli tak:

```text
zwróć odpowiedź użytkownikowi
```

---

# 19. Pętla ReAct

Najprostszy schemat:

```text
Reason
  ↓
Act
  ↓
Observe
  ↓
Reason
  ↓
Act
  ↓
Observe
  ↓
...
  ↓
Final Answer
```

---

# 20. Dlaczego agenci są ważni?

LLM-y są bardzo dobre w:

- rozumieniu języka,
- analizie problemów,
- podejmowaniu decyzji.

Tools pozwalają dodać im możliwość:

- działania,
- pobierania aktualnych danych,
- wykonywania operacji.

Połączenie tych dwóch elementów daje bardzo potężne systemy.

---

# 21. LLM jako „mózg”, tools jako „ręce”

Można użyć prostej metafory:

```text
LLM
= mózg

tools
= ręce
```

LLM decyduje:

```text
co zrobić
```

a tool wykonuje:

```text
konkretną operację
```

---

# 22. Co może być toolem?

Praktycznie dowolna funkcja.

Na przykład:

```text
API
database
calculator
search engine
Python code
file system
email
calendar
external service
```

Jeżeli programista potrafi napisać funkcję wykonującą daną operację, potencjalnie można ją udostępnić agentowi.

---

# 23. Agent może wykonywać złożone workflow

Dzięki dynamicznemu wyborowi narzędzi agent może obsługiwać bardziej złożone zadania.

Przykład:

```text
Znajdź najtańszy lot,
sprawdź hotel,
porównaj ceny,
przygotuj podsumowanie.
```

Agent może wykonać kilka różnych akcji po kolei.

---

# 24. LangChain i LangGraph

Wykład wskazuje, że:

## LangChain

oraz:

## LangGraph

udostępniają gotowe rozwiązania do budowania agentów ReAct.

Dzięki temu nie trzeba implementować całej pętli od zera.

---

# 25. Co oferują gotowi agenci?

Mogą m.in.:

- wywoływać tools,
- obsługiwać wieloetapowe workflow,
- utrzymywać stan,
- powtarzać działania w pętli,
- kończyć pracę, gdy zadanie jest wykonane.

---

# 26. State — stan agenta

W bardziej zaawansowanych systemach agent może utrzymywać informacje o tym:

- co już zrobił,
- jakie wyniki otrzymał,
- jakie kroki pozostały.

To pozwala obsługiwać dłuższe procesy.

---

# 27. Przykład search agenta

W kolejnej części kursu ma zostać pokazany:

## search agent

Taki agent może:

```text
1. dostać pytanie
2. zdecydować, że potrzebuje wyszukiwania
3. użyć search tool
4. przeanalizować wynik
5. ewentualnie wyszukać ponownie
6. wygenerować odpowiedź
```

---

# 28. Cel kolejnej części

Na początku najważniejsze jest nauczenie się:

> **jak wyposażyć LLM w tools za pomocą LangChain.**

Nie chodzi jeszcze o pełne zrozumienie wszystkich szczegółów wewnętrznych.

Najpierw uczymy się interfejsu.

---

# 29. Później — jak agent działa pod spodem

W dalszej części kursu ma zostać pokazane:

- jak działa agent wewnętrznie,
- jak zbudowana jest pętla,
- jak wybierane są narzędzia,
- jak przekazywane są wyniki,
- jak działa control flow.

---

# 30. Agent jako system dynamiczny

Można więc podsumować:

## Chain

```text
A → B → C → D
```

kolejność zdefiniowana wcześniej.

## Agent

```text
A
↓
LLM decyduje
├→ B
├→ C
└→ D
```

Kolejna akcja zależy od aktualnej sytuacji.

---

# 31. Co warto zapamiętać?

1. **Agent to system, w którym LLM decyduje, co zrobić dalej.**
2. **W chainie control flow definiuje programista.**
3. **Agent korzysta z tools, aby wykonywać akcje.**
4. **Tool może być praktycznie dowolną funkcją.**
5. **ReAct oznacza Reasoning + Acting.**
6. **Agent działa iteracyjnie: reason → act → observe.**
7. **Pętla trwa aż do wykonania zadania.**
8. **LangChain i LangGraph oferują gotowe mechanizmy do budowania agentów.**
9. **Najważniejszą różnicą między agentem a chainem jest to, kto decyduje o kolejnym kroku.**
10. **Agents + tools pozwalają LLM nie tylko generować tekst, ale także wykonywać działania.**

---

# 32. Całość w jednym schemacie

```text
USER
  ↓
LLM
  ↓
REASON
  ↓
WHAT NEXT?
  ↓
TOOL
  ↓
OBSERVATION
  ↓
LLM
  ↓
REASON
  ↓
kolejna akcja
  ↓
...
  ↓
FINAL ANSWER
```

---

# 33. Najważniejsze zdanie

> **Agent AI różni się od zwykłego chaina tym, że to LLM dynamicznie decyduje o kolejnych krokach i może używać narzędzi do wykonywania realnych działań.**
