---
category: Rozhodnutí pro komponenty
---

# Carousely

## Úvod

Komponenta **Carousel** je komponentou pro procházení seznamu tématicky podobného obsahu. Musí být implementován tak, aby jej bylo možné ovládat pomocí myši, klávesnice a dotyku.

Tato řešení na rozdíl od některých implementací **Carousel** neposouvá obsah automaticky[^1]. Uživatel má úplnou kontrolu nad tím, které položky carouselu chce vidět a se kterými chce interagovat.

### Doporučený markup

Markup je záměrně zestručněn. Prvky `<li>` reprezentují kontejnery pro položky s obsahem.

```html
<div class="carousel" role="group">
  <div class="carousel__buttons" hidden>
    <button class="carousel__prev" type="button">
      <span class="visually-hidden">předchozí</span>
      <svg class="icon icon--text" aria-hidden="true" focusable="false">
        <use xlink:href="{{site.basedir}}static/images/icons-all.svg#icon-previous"></use>
      </svg>
    </button>
    <button class="carousel__next" type="button">
      <span class="visually-hidden">následující</span>
      <svg class="icon icon--text" aria-hidden="true" focusable="false">
        <use xlink:href="{{site.basedir}}static/images/icons-all.svg#icon-next"></use>
      </svg>
    </button>
  </div>
  <div class="carousel__scrollable">
    <ul class="carousel__list">
      <li>...</li>
      <li>...</li>
      <li>...</li>
      <li>...</li>
      <li>...</li>
      <li>...</li>
      <li>...</li>
      <li>...</li>
    </ul>
  </div>
</div>
```

### Poznámky

* **`role="group"`** Toto je obecná ARIA role. Zde je použita jako indikátor, že tlačítka a plocha, kterou lze scrollovat, jsou sobě příbuzné.
* **tlačítka předchozí a následující:** Někdo může scrollovat skrz obsah za použití tlačítek pro zobrazení předchozího a následujícího obsahu. Je důležité, že tyto tlačítka jsou prvky `<button>` s `type="button"`. Jejich labely jsou doprovázeny vizuálně skrytým textem (třída `visually-hidden`), protože, na rozdíl od `aria-label`, budou labely přeložitelné rozšířeními v prohlížeči. Tlačítka, která nejsou použitelná (například, tlačítko předchozí při načtení stránky) dostávají vlastnost `disabled`. Tlačítko je pak odstraněno z pořadí focusu a je indentifikováno jako nepoužitelné ve výstupu odečítačky obrazovky.
* **`hidden`:** Tlačítka jsou schovaná by default, protože nefungují při absenci JavaScriptu. Jsou viditelné, když JavaScript běží.
* **`icon` SVG:** Aby došlo k jejich skrytí před asistenčními technologiemi a odebrání z pořadí focusu, mají `aria-hidden="true"` a `scrollable="false"`.
* **`carousel__scrollable`:** Toto je scrollovatelné 'okno' pro seznam položek s obsahem.
* **`carousel__list`:** Singulární potomek `carousel__scrollable` musí být `<ul>`, s každým prvkem jako `<li>`. Markup jako seznam je také jako seznam identifikován v asistivních technologiích a jeho položky jsou postupně vyjmenovány. To umožňuje uživatelům odečítačky obrazovky vědět, že se setkali se sadou souvisejícího obsahu a kolik z něj tam je.

## Doporučený layout

Základní funkce scrollování je bez JavaScriptu dosaženo tak, že:

1. Položky se nezalomují
2. Rodič má `overflow-x: auto`

Tento základní layout využívá `inline-block` a zlepšuje se jako Flexbox (například, pomocí at-rule `@supports`). Výhodou Flexboxu je, že jeho stretching algoritmus dělá každou položku stejně vysokou.

```css
.carousel__list {
  display: flex;
  list-style: none;
  padding: 0;
  margin: 0;
  white-space: nowrap;
}

.carousel__list > li {
  display: flex;
  flex-shrink: 0;
  white-space: normal;
  width: 266px; /* příklad pro standardní výšku pro nějaký obsahu */
  transition: opacity 0.5s linear;
}

.carousel__list > li + li {
  margin-left: 1rem;
}
```

V některých operačních systémech není horizontální scrollbar viditelný by default, což znamená, že scrollovatelný kontejner postrádá viditelná vodítka (affordance)[^2]. Ve webkit prohlížečích je možné scrollbar ukázat pomocí vlastních stylů:

```css
.carousel__scrollable::-webkit-scrollbar {
  height: 0.5rem;
}

.carousel__scrollable::-webkit-scrollbar-track {
  background-color: $color--grey;
}

.carousel__scrollable::-webkit-scrollbar-thumb {
  background-color: $color--black;
}
```

### Tlačítka

Tlačítka předchozí a následující, `class="carousel__buttons"`, jsou absolutně napozicována v pravém horním rohu carouselu, vyžadující `position: relative` na rodiči prvku `carousel`. Disabled tlačítka mají redukovanou opacity.

```css
.carousel__buttons button[disabled] {
  opacity: 0.5;
}
```

### Plynulý scrolling

Pokud je podporováno, `scroll-behavior: smooth` animuje scrollování, ať už je scrollování vyvoláno stisknutím předchozího nebo následujícího tlačítka nebo jinými prostředky. Pokud vlastnost není podporována, prohlížeč přes ni přejde, ale rozhraní zůstává použitelné.

Tato funkce se použije pouze v případě, že se uživatel rozhodl pro omezení animací v nastavení systému:

```css
.carousel__scrollable {
  scroll-behavior: smooth;
}

@media (prefers-reduced-motion: reduce) {
  .carousel__scrollable {
    scroll-behavior: auto;
  }
}
```

### Vysoký kontrast

Ujistěte, že jste komponentu otestovali v [Módu Vysokého kontrastu ve Windows](https://support.microsoft.com/en-gb/help/13862/windows-use-high-contrast-mode).

## Doporučené chování

Pokud JavaScript chybí, rozhraní je i tak použitelné. Někdo může scrollovat **Carousel** pomocí gest dotyku nebo dotykové podložky, scrollbaru nebo zaměřením položek, které jsou (v současné chvíli) skryté. JavaScript přidá předchozí a následující tlačítka a `inert` chování (viz níže).

### Inert položky

Ve chvíli, kdy uživatel scrolluje za pomoci myše, dotyku nebo gesta, prohlížeče, které podporují `IntersectionObserver` přidávají `inert` atribut položkám, které nejsou ve viewportu a odebírá ho těm, které do něj přicházejí.

```js
Array.prototype.forEach.call(items, function (item) {
  if (item.isIntersecting) {
    item.target.removeAttribute('inert');
  } else {
    item.target.setAttribute('inert', 'inert');
  }
});
```

Toto odstraní inert položky z pořadí focusu a z accessibility tree[^3]. Výsledkem je, že stejně jako uživatelé s nějakým zrakovým postižením používající myš, uživatelé klávesnice a odečítačky obrazovky mohou vnímat a interagovat pouze s položkami, které nejsou inertní.

Pokud nebyl použit inertní atribut, uživatel klávesnice mohl přejít na konec dlouhého seznamu položek, pouze aby zjistil, že první (aktuálně skrytá) položka byla stále první v pořadí focusu. Stisknutím Tabu by se kontejner posunul zpět na začátek a postup by se ztratil.

### Inert polyfill
Pro `inert` atribut použijte malý polyfill[^4].

### Ovládání tlačítky

Uživatelé klávesnice musí scrollovat obsah pomocí tlačítek. Při každém stisknutí tlačítka scrollovatelný kontejner posouvá polovinu své vlastní šířky (měřeno na načtení stránky).

```js
var scrollAmount = list.offsetWidth / 2;
```

Tlačítka, která nejsou použitelná (předchozí tlačítko, pokud uživatel odscrolluje úplně doleva, nebo tlačítko následující, pokud uživatel odscrolluje úplně doprava), jsou `disabled`. To je možné díky poslechu k eventu `scroll`, který umožňuje debouncing[^5], kvůli případným problémům s výkonem.

```js
var debounced;
scrollable.addEventListener('scroll', function () {
  window.clearTimeout(debounced);
  debounced = setTimeout(disableEnable, 200);
});
```

### Lazy loading

Doporučujeme použít řešení lazy loading[^6] pro obrázky uvnitř carouselu. Dejte pozor, jelikož s lazy loadingem mohou nastat problémy s odskakováním, pokud kontejnery uvnitř `carousel__list` nemají pevnou (fixed) výšku.

## Související výzkum

Toto téma nemá související uživatelský výzkum.

### Pokračovat ve čtení, jinde na webu

[^1]: "Image Carousels: Getting Control of the Merry-Go-Round" — usability.gov, <https://www.usability.gov/get-involved/blog/2013/04/image-carousels.html>
[^2]: "Perceived Affordances and Designing for Task Flow" — Johnny Holland, <http://johnnyholland.org/2010/04/perceived-affordances-and-designing-for-task-flow/>
[^3]: "The Accessibility Tree" — developers.google.com, <https://developers.google.com/web/fundamentals/accessibility/semantics-builtin/the-accessibility-tree>
[^4]: Inert polyfill on Github, <https://github.com/GoogleChrome/inert-polyfill>
[^5]: Throttling and Debouncing in JavaScript — Codeburst.io, <https://codeburst.io/throttling-and-debouncing-in-javascript-b01cad5c8edf>
[^6]: Lazy loading images using Intersection Observer — Dean Hume,  <https://deanhume.com/lazy-loading-images-using-intersection-observer/>