---
category: Rozhodnutí pro základní prvky
---

# Nadpisy

## Úvod

Tlačítka, odkazy a prvky formuláře mají labely. Labely říkají uživatelům, pro co tyto prvky jsou. Nadpisy jsou také labely a popisují části dokumentu. Správné použití prvků pro nadpisy je zvláště důležité pro uživatele odečítaček obrazovky[^1], jelikož nadpisy společně vytvářejí obsah stránky, kterým lze navigovat různými zkratkami a jinými prostředky.

## Doporučený markup

### Prvek `<h1>`

`<h1>` je hlavní nadpisem v dokumentu a označuje samotný dokument. Pro prvky `<h1>` musí platit následující:

1. V dokumentu je pouze jeden
2. Jeho obsah je pro aktuální dokument jedinečný
3. Je to první sémantický prvek uvnitř oblasti `<main>` dokumentu
4. Jeho text se týká / je podmnožinou `<title>`[^2]

Na stránce s výsledky vyhledávání by měl `<title>` znamenat, že stránka skutečně obsahuje výsledky vyhledávání, který výraz byl použit k vyhledávání, a připomenutí webu, na kterém se vyhledávání uskutečnilo.

```html
<title>Výsledky hledání pro "Zprávy": - justice </title>
```

`<h1>` by tedy byla podmnožinou, která neuvádí informace o webu:

```html
<h1>Výsledky hledání výrazu „Zprávy“</h1>
```

### Podnadpiy

Úroveň nadpisu (h1—h6) pro jakoukoli jinou část stránky by měla být vybrána z hlediska příslušnosti. To znamená, že pokud nadpis značí podřízenou podsekci stránky, pak je vhodné `<h2>`. Pokud nadpis značí podsekci podsekce, úroveň by měla být o jednu vyšší než nadřazená.

- Domovská stránka (h1)
  - Nejnovější zprávy (h2)
    - Tento týden (h3)
    - Minulý týden (h3)

Stejně jako ve výše uvedeném příkladu by příbuzné/ekvivalentní sekce měly mít stejnou úroveň nadpisu. V případech, kdy je jedna podsekce považována za důležitější než druhá, _není_ vhodné měnit úroveň nadpisu proto, aby se vytvořil větší nadpis (velikostně/fontem). To pak vytváří nesmyslnou strukturu dokumentu pro uživatele odečítaček obrazovky.

Ať už se podnadpisy používají explicitně s prvky sekcí[^5], měly by to být první prvky, které obsahují obsah v sekci.

#### Špatně: Příklad

V této verzi by uživatel odečítačky obrazovky, který se naviguje po nadpisech (například pomocí klávesy <kbd>H</kbd> s programem NVDA), identifikátor „Upozornění“ přeskočil a minul.

```html
<section>
  <h2>Nejnovější zprávy</h2>
  <section>
    <div class="alert">Upozornění:</div>
    <h3>Příklad upozornění</h3>
  </section>
</section>
```

#### Správně: Příklad

V této upravené verzi, „Upozornění“ je součástí nadpisu a bude přečteno zároveň s ním.

```html
<section>
  <h2>Nejnovější zprávy</h2>
  <section>
    <h3>
      <span class="breaking">Upozornění:</span>
      Příklad upozornění
    </h3>
  </section>
</section>
```

### Formulování

Stejně jako labely, i nadpisy musí popisovat sekci, kterou představují[^3]. 

Spoustu odečítaček obrazovek agreguje nadpisy jako obsah stránky. Například, program JAWS přečte všechny nadpisy při stisknutí <kbd>Insert</kbd> + <kbd>F6</kbd>. Z tohoto důvodu je důležité, aby nadpisy byly dostatečně popisné, nezávislé na jejich okolním obsahu.

#### Špatně: Příklad

```html
<h2>Dejme se do pečení!</h2>
```

#### Správně: Příklad

```html
<h2>Recept na koláč</h2>
```

## Doporučený layout

Chcete-li vytvořit logickou vizuální hierarchii, měly by se velikosti písma nadpisů zmenšovat spolu s hloubkou zanoření. `<h1>` by měla mít největší `font-size` a `<h6>` nejmenší. Existují i jiné prostředky k rozlišení úrovní nadpisů, ale velikost písma je nejběžnější, a proto nejrozšířenější.

```css
h1,
.h1 {
  font-size: 3rem;
}
h2,
.h2 {
  font-size: 2.25rem;
}
h3,
.h3 {
  font-size: 1.75rem;
}
/* atd */
```

Ve výše uvedeném příkladu jsou třídy pojmenovány podle úrovní nadpisů tak, aby odpovídaly jejich velikosti. To může být užitečné, když si přejete zvětšit libovolný text. Je důležité, aby nadpisy byly rezervovány jako labely sekcí, takže pokud chcete přidat nějaký vizuální důraz, použijte třídu namísto prvků pro nadpisy.

```html
<blockquote>
  <p class="h3">"Příklad vizualice textu s velikostí nadpisu."</p>
  <p>Nejedná se o prvek Hx</p>
</blockquote>
```

## Doporučené chování

Tam, kde nadpisy přijímají `id` a používají se jako fragmenty dokumentů (DocumentFragment), aby bylo možné mezi nimi navigovat odkazy na stejné stránce, by měly mít hodnotu `tabindex="-1"`. To vynutí prohlížeče, které by jinak neaktualizovaly svůj počáteční bod sekvenčního focusu[^4], aby přesunuly fokus klávesnice na daný nadpis a sekci.

```html
<h2 tabindex="-1">Navigovatelná podsekce</h2>
```

## Související výzkum

Toto téma nemá související uživatelský výzkum.

### Pokračovat ve čtení, jinde na webu

[^1]: "Responses To The Screen Reader Strategy" — heydonworks.com, <http://www.heydonworks.com/article/responses-to-the-screen-reader-strategy-survey>
[^2]: WCAG 2.4.2: Page Titled, <https://www.w3.org/TR/WCAG21/#page-titled>
[^3]: WCAG 2.4.6: Headings and labels, <https://www.w3.org/TR/WCAG21/#page-titled>
[^4]: "Focus should cycle from named anchor" — bugs.chromium.org, <https://bugs.chromium.org/p/chromium/issues/detail?id=262171>
[^5]: HTML 5.3: Sections (W3C), <https://www.w3.org/TR/html53/sections.html#sections>
[^6]: "There Is No Document Outline Algorithm" — Adrian Roselli , <http://adrianroselli.com/2016/08/there-is-no-document-outline-algorithm.html>
