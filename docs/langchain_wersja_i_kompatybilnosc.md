# Wersja LangChain i kompatybilność kursu

## 1. Wersja LangChain użyta w kursie

Kurs jest przygotowany dla wersji:

```text
LangChain 1.0.2
```

W momencie nagrywania była to aktualna wersja używana przez autora kursu.

---

## 2. Jak sprawdzić wersję?

Wersję używaną w projekcie można sprawdzić w pliku:

```text
uv.lock
```

Plik ten zawiera dokładne wersje zależności zainstalowanych w projekcie.

W repozytorium kursu również znajduje się odpowiadający mu plik `uv.lock`.

---

## 3. Co się stanie, jeśli oglądasz kurs później?

Jeżeli od nagrania kursu minęło trochę czasu, aktualna wersja LangChain może być nowsza.

Przykładowo zamiast:

```text
1.0.2
```

może to być:

```text
1.0.7
```

lub inna wersja z tej samej głównej linii.

Według autora kursu niewielkie aktualizacje nie powinny powodować większych problemów z kompatybilnością.

W obrębie tej samej linii **MAJOR** (np. cała rodzina `1.x`) przejście z `1.0.x` na nowszy **MINOR** (np. `1.3.x`) zwykle oznacza nowe możliwości i poprawki, a nie automatyczne łamanie kontraktu API tak jak przy skoku `1.x → 2.x`. Nadal warto porównać zachowanie z materiałem kursu, gdy coś działa inaczej niż na nagraniu.

---

## 4. Małe aktualizacje wersji

Mniejsze aktualizacje mogą zawierać:

- poprawki błędów,
- drobne ulepszenia,
- nowe funkcjonalności.

Nie powinny jednak znacząco zmieniać sposobu korzystania z biblioteki.

---

## 5. Semantic Versioning

Wykład wspomina o:

## semantic versioning

czyli wersjonowaniu semantycznym.

Typowy zapis wygląda tak:

```text
MAJOR.MINOR.PATCH
```

Przykład:

```text
1.0.2
```

możemy czytać jako:

```text
1 = major
0 = minor
2 = patch
```

---

## 6. Co oznaczają kolejne części wersji?

### MAJOR

Duża zmiana wersji.

Może oznaczać:

- większe zmiany API,
- możliwe problemy kompatybilności,
- konieczność zmiany kodu.

Przykład:

```text
1.x.x → 2.x.x
```

---

### MINOR

Nowa funkcjonalność bez dużego łamania kompatybilności.

Przykład:

```text
1.0.x → 1.1.x
```

---

### PATCH

Drobne poprawki i bugfixy.

Przykład:

```text
1.0.2 → 1.0.3
```

---

## 7. Kiedy mogą pojawić się problemy?

Największej ostrożności wymagają:

- duże zmiany wersji,
- zmiany API,
- usunięcie lub przeniesienie klas i funkcji.

Jeżeli pojawi się niekompatybilność między kodem z kursu a nowszą wersją LangChain, może być konieczna drobna aktualizacja kodu.

---

## 8. Najważniejsza praktyczna rada

Jeżeli chcesz odtworzyć środowisko kursu możliwie dokładnie, korzystaj z wersji zapisanych w:

```text
uv.lock
```

To daje największą szansę, że kod będzie działał dokładnie tak samo jak w materiale.

---

## 8a. Zakres w deklaracji zależności vs dokładny pin

W konfiguracji projektu często pojawia się **zakres** (np. „wersja co najmniej X”), a menedżer pakietów utrwala **dokładną** wersję w pliku lock.

Intuicja:

```text
deklaracja: „akceptuję 1.3.11 lub nowsze w linii 1.x”
     ↓
lock: „w tym środowisku naprawdę zainstalowano konkretną wersję”
```

Dzięki temu:

- zakres pozwala bezpiecznie brać nowsze **patch/minor** przy aktualizacji środowiska,
- lock zapewnia powtarzalność — wszyscy z tym samym lockiem powinni dostać te same wersje.

Przy debugowaniu „u mnie działa inaczej niż w kursie” najpierw porównaj dokładną wersję z locka, nie tylko luźny zakres z deklaracji.

---

## 9. Co warto zapamiętać?

1. **Kurs był przygotowany dla LangChain 1.0.2.**
2. **Dokładne wersje zależności można sprawdzić w `uv.lock`.**
3. **Drobne aktualizacje powinny być zazwyczaj kompatybilne.**
4. **Patch version zwykle oznacza poprawki błędów.**
5. **Większe zmiany wersji (zwłaszcza MAJOR) mogą zmieniać API.**
6. **Jeżeli pojawi się problem z kompatybilnością, warto porównać używaną wersję z wersją z kursu.**
7. **Zakres w deklaracji zależności ≠ pin w locku — przy różnicach zachowań porównuj dokładną wersję.**
8. **Skok MINOR w obrębie tego samego MAJOR jest zwykle łagodniejszy niż skok MAJOR.**

---

## 10. Najważniejsze zdanie

> **Jeżeli kod z kursu zachowuje się inaczej, jedną z pierwszych rzeczy do sprawdzenia powinna być wersja LangChain i zależności zapisane w `uv.lock`.**
