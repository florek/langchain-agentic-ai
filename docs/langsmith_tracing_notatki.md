# LangSmith — tracing i obserwowalność aplikacji LLM

## 1. Cel lekcji

W tej części wykładu konfigurujemy **LangSmith** w aplikacji i włączamy **tracing**.

Celem jest możliwość obserwowania:

- wywołań modeli,
- promptów,
- wyników,
- czasu wykonania,
- liczby tokenów,
- błędów,
- kolejnych kroków chaina.

Najważniejsza idea:

> Po poprawnym skonfigurowaniu LangSmith tracing może działać automatycznie dla aplikacji zbudowanej w LangChain.

---

## 2. Czym jest LangSmith?

**LangSmith** to platforma do obserwowania, debugowania i analizowania aplikacji korzystających z modeli językowych.

Pozwala zobaczyć, co dokładnie wydarzyło się podczas pojedynczego uruchomienia aplikacji.

Przykładowo:

```text
użytkownik
   ↓
prompt template
   ↓
LLM
   ↓
odpowiedź
```

LangSmith potrafi zarejestrować każdy z tych kroków.

---

## 3. Co oznacza tracing?

**Tracing** oznacza śledzenie kolejnych operacji wykonywanych przez aplikację.

Dzięki temu zamiast widzieć tylko:

```text
wejście → odpowiedź
```

możemy zobaczyć bardziej szczegółowy przebieg:

```text
wejście
  ↓
formatowanie promptu
  ↓
wywołanie modelu
  ↓
wynik modelu
  ↓
kolejny krok chaina
  ↓
wynik końcowy
```

To bardzo przydatne przy bardziej złożonych aplikacjach.

---

## 4. Konto i projekt w LangSmith

Po zalogowaniu do LangSmith tworzymy projekt.

W wykładzie projekt nazwano:

```text
hello world
```

Projekt służy do grupowania wszystkich trace'ów związanych z konkretną aplikacją.

```text
LangSmith
   ↓
projekt "hello world"
   ↓
run 1
run 2
run 3
...
```

---

## 5. API key

Aplikacja musi mieć dostęp do LangSmith API.

Dlatego generujemy:

```text
API key
```

Klucz API zapisujemy w pliku:

```text
.env
```

Nie powinien być wpisywany bezpośrednio do kodu aplikacji.

---

## 6. Konfiguracja przez zmienne środowiskowe

Integracja sprowadza się głównie do ustawienia odpowiednich zmiennych środowiskowych.

W wykładzie ustawiane są m.in.:

```text
tracing = true
API key
project
endpoint
```

Czyli aplikacja musi wiedzieć:

- że tracing ma być włączony,
- jaki klucz API wykorzystać,
- do którego projektu wysyłać trace'y,
- z jakiego endpointu LangSmith korzystać.

---

## 7. Region EU

W wykładzie zwrócono uwagę na konfigurację endpointu dla regionu europejskiego.

Jeżeli korzystamy z regionu EU, należy wskazać odpowiedni adres endpointu.

To ważne, ponieważ bez poprawnego endpointu aplikacja może nie być w stanie wysyłać trace'ów do LangSmith.

Najważniejsza intuicja:

```text
aplikacja
   ↓
właściwy endpoint LangSmith
   ↓
projekt
   ↓
trace
```

---

## 8. Przykładowy plik `.env`

Schematycznie konfiguracja może wyglądać tak:

```env
LANGSMITH_TRACING=true
LANGSMITH_API_KEY=...
LANGSMITH_PROJECT=hello-world
LANGSMITH_ENDPOINT=...
```

Nazwy i wartości zależą od konfiguracji środowiska.

Klucz API powinien pozostać prywatny.

---

## 9. Co dzieje się po włączeniu tracingu?

Po poprawnym skonfigurowaniu zmiennych środowiskowych nie trzeba ręcznie dodawać tracingu do każdego wywołania.

Jeżeli korzystamy z obsługiwanych obiektów LangChain, kolejne uruchomienia mogą być automatycznie rejestrowane.

```text
uruchamiamy chain
      ↓
LangChain wykonuje kolejne kroki
      ↓
LangSmith zapisuje trace
```

---

## 10. Run

Każde wykonanie aplikacji lub fragmentu chaina może pojawić się w LangSmith jako:

## run

Run reprezentuje konkretne wykonanie.

Przykładowo:

```text
Run #1
prompt → Gemma 3 → odpowiedź
```

albo:

```text
Run #2
prompt → GPT-5 → odpowiedź
```

---

## 11. Co możemy zobaczyć w runie?

Po wejściu w pojedynczy run można analizować m.in.:

- wejście,
- prompt,
- odpowiedź modelu,
- użyty model,
- kolejne elementy chaina,
- czas wykonania,
- liczbę tokenów.

To znacznie ułatwia zrozumienie, co właściwie zrobiła aplikacja.

---

## 12. Śledzenie struktury chaina

Jeżeli aplikacja korzysta np. z:

```text
PromptTemplate
     ↓
Chat model
     ↓
Parser / wynik
```

LangSmith pokazuje te elementy jako kolejne kroki.

Dzięki temu można zobaczyć:

1. jak utworzono prompt,
2. co zostało przekazane do modelu,
3. jak model odpowiedział,
4. jaki był wynik końcowy.

---

## 13. Analiza modelu

LangSmith pozwala zobaczyć, którego modelu użyto w danym wywołaniu.

W wykładzie najpierw wykorzystywany jest lokalny model przez Ollama, a następnie model OpenAI.

Możemy więc porównywać różne runy:

```text
Run A → Gemma 3
Run B → GPT-5
```

---

## 14. Latency

Jedną z ważnych metryk jest:

## latency — czas odpowiedzi

Pozwala sprawdzić:

> Ile czasu trwało wykonanie danego wywołania?

Przykładowo:

```text
Model A → 2 s
Model B → 16 s
```

To ważne, ponieważ jakość modelu to nie jedyny parametr aplikacji.

Liczy się również szybkość.

---

## 15. Time to first token

W LangSmith można analizować również:

## time to first token

czyli:

> czas od wysłania zapytania do pojawienia się pierwszego tokenu odpowiedzi.

Jest to szczególnie ważne w aplikacjach interaktywnych.

---

## 16. Token usage

Możemy również zobaczyć liczbę wykorzystanych tokenów.

Przykładowo:

```text
input tokens
output tokens
total tokens
```

To ważne z dwóch powodów:

1. wpływa na koszt,
2. pomaga optymalizować prompty.

---

## 17. Koszt i efektywność

Dzięki tracingowi możemy analizować kompromis:

```text
jakość
↕
czas
↕
liczba tokenów
↕
koszt
```

Może się okazać, że większy model daje lepszą odpowiedź, ale działa znacznie wolniej i zużywa więcej tokenów.

---

## 18. Tagi

Do runów można dodawać:

## tags — tagi

Pozwalają później łatwiej filtrować dane.

Przykład:

```text
model:gpt5
environment:dev
feature:chat
experiment:v2
```

---

## 19. Filtrowanie runów

LangSmith pozwala filtrować zarejestrowane wykonania.

Możemy np. wyszukiwać:

- konkretne modele,
- wybrane typy runów,
- runy oznaczone tagami,
- błędne wykonania.

To szczególnie ważne, gdy aplikacja zaczyna generować setki lub tysiące trace'ów.

---

## 20. Statystyki

Platforma może agregować wyniki z wielu runów.

W wykładzie pokazano m.in.:

```text
error rate
median tokens
p50
p90
```

---

## 21. Error rate

**Error rate** pokazuje procent uruchomień zakończonych błędem.

To jedna z podstawowych metryk stabilności aplikacji.

---

## 22. Mediana

Mediana pozwala zobaczyć typową wartość bez dużego wpływu pojedynczych ekstremalnych wyników.

Przykład:

```text
1 s
1 s
2 s
2 s
30 s
```

Średnia zostanie mocno podbita przez 30 sekund, ale mediana nadal dobrze opisuje typowe zachowanie.

---

## 23. P50 i P90

### P50

50% wywołań zakończyło się w czasie równym lub krótszym od tej wartości.

To praktycznie mediana.

### P90

90% wywołań zakończyło się w czasie równym lub krótszym od tej wartości.

Przykład:

```text
P50 = 2 s
P90 = 6 s
```

oznacza, że połowa odpowiedzi trwa maksymalnie 2 sekundy, a 90% odpowiedzi maksymalnie 6 sekund.

---

## 24. Dlaczego LangSmith jest przydatny?

Bez tracingu widzimy głównie:

```text
prompt → wynik
```

Z LangSmith:

```text
prompt
  ↓
PromptTemplate
  ↓
model
  ↓
tokeny
  ↓
czas
  ↓
kolejne kroki
  ↓
wynik
```

Możemy więc dokładnie prześledzić działanie aplikacji.

---

## 25. Debugowanie

LangSmith jest szczególnie pomocny, kiedy:

- odpowiedź modelu jest zła,
- chain wykonuje nieoczekiwany krok,
- aplikacja działa zbyt wolno,
- zużywa za dużo tokenów,
- pojawia się błąd,
- chcemy porównać dwa modele.

---

## 26. LangSmith i agenci

Wykład podkreśla, że tracing staje się jeszcze bardziej wartościowy przy:

## agentach

Agent może wykonywać wiele kroków:

```text
LLM
 ↓
decyzja
 ↓
tool
 ↓
wynik
 ↓
LLM
 ↓
kolejna decyzja
 ↓
tool
 ↓
...
```

Bez dobrego tracingu trudno byłoby zrozumieć, dlaczego agent zrobił coś niepoprawnie.

LangSmith może pokazać cały przebieg.

---

## 27. Przykład przyszłego agenta

Załóżmy:

```text
Użytkownik:
"Znajdź mi lot i hotel."
```

Agent może wykonać:

```text
1. analiza zadania
2. wywołanie wyszukiwarki lotów
3. analiza wyników
4. wywołanie wyszukiwarki hoteli
5. porównanie
6. przygotowanie odpowiedzi
```

LangSmith może zarejestrować każdy z tych kroków.

---

## 28. Porównywanie modeli

W wykładzie pokazano również zmianę modelu z lokalnego modelu na GPT-5.

Dzięki temu można porównywać:

```text
model lokalny
vs
model chmurowy
```

pod kątem:

- jakości,
- latency,
- tokenów,
- kosztu,
- błędów.

---

## 29. Minimalna konfiguracja

Najważniejsza idea konfiguracji sprowadza się do:

```text
1. utwórz konto LangSmith
2. wygeneruj API key
3. włącz tracing
4. ustaw projekt
5. ustaw endpoint
6. uruchom aplikację
7. sprawdź trace w LangSmith
```

---

## 30. Co warto zapamiętać?

1. **LangSmith służy do obserwowania i debugowania aplikacji LLM.**
2. **Tracing pozwala zobaczyć cały przebieg wykonania chaina.**
3. **Każde uruchomienie może być zapisane jako run.**
4. **Run zawiera wejście, wyjście i informacje o kolejnych krokach.**
5. **Możemy analizować latency i time to first token.**
6. **Możemy mierzyć zużycie tokenów.**
7. **Tagi ułatwiają późniejsze filtrowanie runów.**
8. **Statystyki takie jak error rate, P50 i P90 pomagają ocenić jakość systemu.**
9. **Tracing jest szczególnie wartościowy przy agentach.**
10. **Po poprawnej konfiguracji LangSmith może śledzić wiele wywołań LangChain automatycznie.**

---

## 31. Całość w jednym schemacie

```text
Aplikacja
   ↓
LangChain
   ↓
PromptTemplate
   ↓
LLM
   ↓
wynik

Jednocześnie:

LangSmith
   ↓
trace
   ↓
runy
   ↓
latency
tokeny
błędy
struktura chaina
statystyki
```

---

## 32. Najważniejsze zdanie

> **LangSmith daje nam wgląd w to, co naprawdę dzieje się wewnątrz aplikacji LLM — od promptu, przez wywołanie modelu, aż po czas, tokeny, błędy i wszystkie pośrednie kroki.**
