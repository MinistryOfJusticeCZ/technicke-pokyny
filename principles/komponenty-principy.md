---
category: Principy pro vývoj
expires: 2019-03-31
---

# Principy pro vývoj frontend komponent

Komponenta je balíček, který se skládá ze šablony, stylu, chování a dokumentace. V případě, že se chystáte vytvořit novou komponentu, ujistěte se, že splňujete tyto principy.

Co dělá komponentu dobrou?

## Dobrá komponenta je:

* přístupná
* zdokumentována
* izolována
* otestována
* přeložitelná do jiného jazyka

### Komponenta je přístupná, pokud:

* je [WCAG AA compliant](https://www.w3.org/WAI/WCAG20/quickref/)
* má automatizované testování přístupnosti s dobrým pokrytím kvůli regresím
* je responzivní
* je [progresivně zlepšována](https://en.wikipedia.org/wiki/Progressive_enhancement)

### Komponenta je zdokumentována, pokud je:

* její význam jasný
* znázorněna tak, jak bude ve skutečnosti vypadat
* jasné, jak ji použít
* definováno to, co ji dělá přístupnou
* v jednom dokumentu s ostatními komponenty

### Komponenta je izolována, pokud:

* její styly a skripty nemají žádný efekt na stránku nebo jiné komponenty
* není závislá na externích selektorech, které by přiřazovaly styly svým potomkům

### Komponenta je otestována, pokud má:

* unit testy
* testy pro vizuální regresi
* automatizované testy přístupnosti

## Architektura dobré komponenty:

* API komponenty je poskytováno konzistentně
* definuje konvence, kterých se komponenty drží
* používá lintování komponent pro konzistetní stylizování kódování
* umožňuje komponenty snadno budovat, přesouvat a mazat
* umožňuje komponenty snadno poskládat a dávat dohromady bez dalšího kódu, který by je dohromady slepoval
* umožňuje komponenty najít
* interní implementace komponenty je aplikacím skryta