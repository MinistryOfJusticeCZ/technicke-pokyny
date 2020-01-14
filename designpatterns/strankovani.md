---
category: Rozhodnutí pro komponenty
---

# Stránkování a Načíst další

## Úvod

Pro navigaci mezi stránkami s celistvým obsahem použijte stránkování. Pattern **Načíst další** na tomto chování staví. Dává tak možnost inkrementálního načítání položek s obsahem přímo na té samé stránce. Nejčastěji se používá ve spojení s mechanismem filtrování obsahu.

**Načíst další** je pro průzkum obsah upřednostňováno před stránkováním. Nepožaduje po uživatelích odčítačky obrazovky a klávesnice, aby pro každý požadavek na obsah a s novým načtením stránky procházeli hlavičku/preambuli. Tento pattern se navíc vyhýbá problémům, které se objevují v automatických nekonečných rolovacích implementacích[^1][^2]. Tyto problémy zahrnují:

* Nevyžádané rolování vedoucí k nesprávnému umístění
* Neschopnost přesunout oblast obsahu na konec stránky
* Nedostatek jasné zpětné vazby a focus management

## Doporučený markup

V implementaci by měl skript reagovat na prvek `class="loader"`. V ukázce kódu níže, je to zobrazeno v očekávaném kontextu elementu `<main>`. Tato ukázka pouze ukazuje základní strukturu s popisy dílčích složek patternu, který má následovat.

```html
<main id="main" tabindex="-1">
  <h1>Hledali jste "soud"</h1>
  <div class="loader">
    <ul class="loader__items">
      <!-- načtené položky -->
    </ul>
    <div class="loader__foot">
      <div class="loader__loading" role="status" hidden>
        <!-- indikátor načítání (spinner) -->
      </div>
      <button class="loader__button button" hidden>Načíst další</button>
      <nav class="pages" aria-labelledby="pagination-label">
        <!-- funkcionality stránkování -->
      </nav>
    </div>
  </div>
</main>
```

### Prvek main
Main element[^3] má atributy `id="main"` a `tabindex="-1"`. Je to z toho důvodu, protože je očekáváno, že bude zaměřen "skip link odkazem" na té samé stránce. V případech, kdy JavaScript neběží a uživatel se musí spoléhat na komponentu stránkování, skip link je nechá obejít hlavičku stránky, aby se uživatel dostal na následující obsah.

### `loader__items`

Načtené položky obsahu jsou prezentovány jako seznam. Položky seznamu jsou vyjmenovány v softwaru odečítaček obrazovky. Uživateli je sdělováno, kolik položek je celkem přítomno a s jakou z nich interaguje (_"4 z 37"_).

Jak je rozebráno v [Doporučeném chování](#doporuřené-chování), pokaždé kdy je načtena nová sada položek, je příchod této sady uvozen prvkem "oddělovač". Ten bere `role="separator"` ARIA atribuce[^4], prvek tak není započítán do celkového počtu v seznamu, takže bude celkový počet výsledků asistenčními technologiemi reportován přesně.

```html
<li role="separator">
  Výsledky 6 až 12
</li>
```

### `loader__loading`

Tento konstrukt zahrnuje vizuální indikátor načítání — SVG ikonka — a live region (`role="status"`)[^5]. Při implementaci, jakmile je request o načtení položek, je text _"Načítání, čekejte prosím"_ neviditelně vložen do live regionu. Toto připojení textu spustí okamžité oznámení ve výstupu odečítačky obrazovky.

```html
<div class="loader__loading" role="status" hidden>
  <svg class="icon icon--text icon-loading" focusable="false" aria-hidden="true">
    <use xlink:href="{{site.basedir}}static/images/icons-all.svg#icon-loading"></use>
  </svg>
  <div class="loader__loading-text visually-hidden"></div>
</div>
```

### Tlačítko 'načíst další'

```html
<button class="loader__button button" type="button" hidden>Načíst další</button>
```

Tlačítko je schováno by default. Pouze tam, kde běží JavaScript, je zobrazeno (a fallback komponenta _stránkování_ je odstraněna). Musí se jednat o standardní prvek `<button>`, s `type="button"`. Je vždy na konci seznamu načtených položek a může být jednoduše překročen/odtabován klávesnicí.

### Jednotlivé výsledky

Tento pattern nepředepisuje, jak by měly být jednotlivé výsledky vytvářeny, kromě toho, že by to měly být položky seznamu a ve většině případů by měl obsahovat odkaz na permalink na výsledek.

Každý výsledek můžete udělat jako nadpis, ale nadpis každého výsledku musí být na stejné úrovni: o jednu úroveň vyšší, než je hlavní nadpis stránky. Protože hlavní nadpis by měl být vždy `<h1>`, výsledky by měly být` <h2>`, aby se popsala správná struktura vnoření:

* Vyhledali jste "soud" (h1)
    * Nejvyšší soud (h2)
    * Okresní soud v Břeclavi (h2)
    * Okresní soud v Teplicích (h2)
    * atd.

### Komponenta stránkování

Komponenta stránkování zahrnuje labelovaný navigační landmark. Pro rychlou navigaci mají odečítačky obrazovky tendenci poskytovat souhrnné seznamy prvků landmarků.

Všimněte si použití `role="separator"` pro odstranění prvku výpustky z výčtu. Tam kde odkaz na předchozí a následující není použitelný, je 'disabled' tím, že je mu odstraněn jeho `href`. Tak je zároveň odstraněn z pořadí focusu. Současná stránka je identifikována žpřístupněně pomocí `aria-current="page"`[^6].

```html
<nav class="pages" aria-labelledby="pagination-label">
  <div id="pages-label" hidden>Stránka</div>
  <a class="pages-prev">
    <span class="visually-hidden">Předchozí stránka</span>
    <svg class="icon icon--text" focusable="false" aria-hidden="true">
      <use xlink:href="{{site.basedir}}static/images/icons-all.svg#icon-previous"></use>
    </svg>
  </a>
  <ol class="pages-numbered">
    <li><a href="?page=1" aria-current="page">1</a></li>
    <li><a href="?page=2">2</a></li>
    <li><a href="?page=3">3</a></li>
    <li><a href="?page=4">4</a></li>
    <li><a href="?page=5">5</a></li>
    <li><a href="?page=6">6</a></li>
    <li><a href="?page=7">7</a></li>
    <li role="separator">&hellip;</li>
    <li><a href="?page=999">999</a></li>
  </ol>
  <div class="pages-text">Stránka 1 z 999</div>
  <a class="pages-next" href="?page=2">
    <span class="visually-hidden">Další stránka</span>
    <svg class="icon icon--text" focusable="false" aria-hidden="true">
      <use xlink:href="{{site.basedir}}static/images/icons-all.svg#icon-next"></use>
    </svg>
  </a>
</nav>
```

Tento fallback mechanismus je zobrazen těm uživatelům, jejichž prohlížeč nepodporuje JavaScriptové `Promise` rozhraní, nebo těm kterým neběží JavaScript.

## Doporučený layout

Estetika vašich výsledků vyhledávání není předepsána tímto patternem, ale je třeba dodržovat několik obecných pravidel:

* Každý výsledek/položka je rozdílná od svých sousedních výsledků/položek
* Oddělovací prvek, který představuje nové dávky výsledků hledání (viz [Doporučené chování](#doporučené-chování)), je odlišný od samotných výsledků
* Tlačítko 'Načíst další' by mělo dostávat standardní styl tlačítka (skrz třídu `class="button"`)

## Indikátor načítání

Indikátor načítání je standardní SVG ikonka,  `icon-loading`. Objevuje se nad tlačítkem, zatímco jsou stahovány výsledky. Není širší než samotné tlačítko. Animace je implementována následně:

```css
@keyframes spin {
  0% {
    transform:rotate(0deg);
  }
  100% {
    transform:rotate(360deg);
  }
}

.icon-loading {
  animation-name: spin;
  animation-duration: 1s;
  animation-iteration-count: infinite;
  animation-timing-function: linear;
}
```

### Oddělovač

Prvek oddělovač, který uvozuje každou dávku nových výsledků, bere focus, aby další položky byly v následujícím pořadí focusu. Tudíž by měl mít nějakou formu focusu. Můžete použít mizející animovaný focus styl, který upozorní na změnu, ale následně zmizí. Přetrvávající focus styl může uživatele mást, jelikož prvek není interaktivní.

```css
@keyframes focus {
  0%{
    outline: 2px dotted $color--black;
  }
  100% {
    outline: 2px dotted transparent;
  }
}

.loader__items [role="separator"]:focus {
  outline-width: 0;
  outline-offset: 2px;
  animation: focus 1s linear 1;
}
```

## Komponenta stránkování

Pro maximální zpětnou kompatibilitu s minimem kódu, dá `inline-block` položky vedle sebe. Doporučujeme pokud se šířka okna změnší na `650px`, aby komponenta stránkování obsahovala pouze tlačítka na předchozí a následující stránku, a informaci na jaké stránce z kolika se uživatel nachází.

```css
@media (min-width: 650px) {
  .pages-text {
    display: none;
  }

  .pages-numbered {
    display: inline-block;
  }
}
```

## Doporučené chování

Chování se liší na základě toho, zda JavaScript běží a zda jsou rozhraní `Promise` a `fetch` podporována:

### Bez zlepšení JavaScriptem

1. Uživateli je zobrazena úvodní stránka výsledků / položek obsahu
2. Na konci výsledků je komponenta stránkování, která mu umožňuje navigaci mezi stránkami s výsledky
3. Stisknutím odkazu na stránku je daná stránka načtena (uživatel odečítačky obrazovky uslyší oznamení `<title>` stránky; uživatele klávesnici bude mít svůj bod focusu inicializován na prvek `<body>` dokumentu)
4. Současná stránka je indikována stylem, který odpovídá stylu `:focus`, a asistečním technologiím je komunikována skrz `aria-current="page"`
5. Předchozí a následující odkazy jsou 'disabled' a nezaměřitelné, kde nejsou použitelné odstraněním jejich `href`

### Zlepšení JavaScriptem

1. Uživateli je zobrazena úvodní stránka výsledků / položek obsahu. Ty jsou server-rendered za účelem, aby stránkování fallback fungoval
2. Na konci výsledků, je tlačítko 'Načíst další', které umožňuje načíst více výsledků
3. Ve chvíli, kde je tlačítko stisknuto, indikátor načítání ('spinner') se zobrazí nad tlačítkem, a _"Načítání, čekejte prosím"_ je ohlášeno odečítačkami skrz doplňkový live region
4. Když jsou výsledky načítány (fetched), dějí se dvě věci:
    1. Indikátor načítání je schován, a obsah live regionu vyprázdněn. V některých nastavení dojde k jeho umlčení okamžitě; v jiných _zpráva "Načítání, čekejte prosím"_ bude ohlášena celá (pokud už nebyla)
    2. Výsledky se objeví, uvozeny oddělovacím prvkem. Tento prvek potvrzuje, kolik položek bylo načteno (_"položky 12 z 18:"_, například) a bere focus
5. Zaměřený oddělovací prvek ohlašuje svůj potvrzovací text v softwaru odečítaček obrazovky a umisťuje uživatele klávesnice na vhodné místo k procházení nově připojených položek

## Související výzkum

Toto téma nemá související uživatelský výzkum.

### Pokračovat ve čtení, jinde na webu

[^1]: Automatic Infinite Scrolling And Accessibility — Simply Accessible, <https://simplyaccessible.com/article/infinite-scrolling/>
[^2]: So You Think You Built A Good Infinite Scroll — Adrian Roselli, <http://blog.adrianroselli.com/2014/05/so-you-think-you-built-good-infinite.html>
[^3]: The Main Element — HTML5 Doctor, <http://html5doctor.com/the-main-element/>
[^4]: Separator (role) — W3C, <https://www.w3.org/WAI/PF/aria/roles#separator>
[^5]: ARIA Live Regions — MDN, <https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions>
[^6]: Using The ARIA Current Attribute — tink.uk, <https://tink.uk/using-the-aria-current-attribute/>