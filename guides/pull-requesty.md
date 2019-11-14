---
category: Git
expires: 2018-05-31
---

# Pull requesty - NEPOUŽÍVAT

Jako tvůrce budu pull requesty (PRs):

- udržovat PRs malé

Jako kontrolor (reviewer) PRs budu:

- dělat vše proto, abych je zkontroloval, co možná nejdříve
- schvalovat, jakmile jsou lepší než současný kód

Při všech změnách praktikujeme na MSp kontrolu kódu (code review), jelikož to:

- zlepšuje kvalitu kódu

## Velikost

## Kdy review dělat

## Kdo by měl review dělat

## Techniky pro review

## Jak dlouho by měl být PR otevřený

Pull requesty by měly být malé a časté. Pokud jsou otevřeny déle než 2-3 dny, tak je pravděpodobně něco špatně. 

## Nástroje pro usnadnění

### Zanedbané PRs

Pro automatizavané připomínání dlouho otevřených PRs, použijte [probot-stale](https://probot.github.io/apps/stale/). Ten je i zavře v případě, že se nad PRs neděje žádná práce.

### Šablona pro PRs

Naše standardní šablona pro PR:

```markdown
**Před vytvořením pull requestu se ujistěte, že:**

- [ ] commit messages jsou smysluplné a splňují pokyny pro dobrou commit message
- [ ] README a další dokumentace byla aktualizována / přidána (pokud bylo třeba)
- [ ] testy byly aktualizovány / nové testy byly přidány (pokud bylo třeba)

Tento řádek a vše nad ním odstraňte a vyplňte následující sekce:


### Odkaz na ticket (pokud je třeba) ###



### Zmeňte tento popis ###
<!-- Uveďte, co měníte, jestli proběhly testy, a tak dále -->


**Obsahuje PR breaking change?** (zaškrtněte jedno pole pomocí "x")

\``` (při zkopírování-a-vložení odstraňte tento komentář a zpětné lomítko)
[ ] Ano
[ ] Ne
\```

```

## Pokračovat ve čtení
Přečtěte si alespoň článek na hackernoon:

- [https://hackernoon.com/the-art-of-pull-requests-6f0f099850f9](https://hackernoon.com/the-art-of-pull-requests-6f0f099850f9)
- [https://www.atlassian.com/blog/git/written-unwritten-guide-pull-requests](https://www.atlassian.com/blog/git/written-unwritten-guide-pull-requests)
- [https://github.community/t5/Support-Protips/Best-practices-for-pull-requests/ba-p/4104](https://github.community/t5/Support-Protips/Best-practices-for-pull-requests/ba-p/4104)