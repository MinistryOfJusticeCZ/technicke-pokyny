---
category: Rozhodnutí pro komponenty
---

# Karty a Metadata proužky

## Úvod

Komponenta **Karta** slouží jako ukázka obsahu, který odkazuje na stránku s celým obsahem. Principiálně se jedná o odkaz na obsah, ale může s sebou nést doplňující obsah, jako média, data a podobně.

**Metadata proužek** je jednoduchá komponenta, která může být součástí Karty. Definuje klíčové informace přiřazené k obsahu, jako je datum jeho vydání a téma.

## Vlastnosti obsahu

| Vlastnost | Popis |
| -------- | ----------- |
| Nadpis | Text jednoznačně popisující cílový obsah |
| Obrázek | Obrázek propagující cílový obsah |
| Označení média | Informace o propagovaném médiu |
| Popis | Krátký popis cílového obsahu |
| Metadata | Viz komponenta [Metadata proužek](#metadata) |


## Doporučený markup

### O `<aside>`
Zvažte kontext a okolní obsah **Karty**. Obecný účel této komponenty je odkázat doplňkový nebo související obsah, takže je často vhodné pro karty (nebo skupinu karet), aby byly umístěny dovnitř prvku `<aside>`, tedy complementary landmarku[^1]. Pokud je však zbytek stránky věnován zobrazení seznamu položek jako karty (jako je seznam nebo index doporučeného obsahu), pak _jsou_ karty hlavní obsah. Tento úsudek je nejlépe proveden s ohledem na úhel pohledu našeho publika: co by považovali za užitečné a použitelné versus překvapivé nebo matoucí.

```html
<aside aria-labelledby="unique-card-label">
  <span id="unique-card-label" hidden>Související stránky</span>
  <!-- Karta(y) tady -->
</aside>
```

Všimněte si mechanismu pro labeling. Complementary landmarky jsou objevitelné uživateli odečítaček obrazovek skrz procházení, nebo hledáním landmark seznamů. Label identifikuje specifický účel landmarku. Vhodné labely jsou _"Přibuzné"_, _"Přečíst si více"_, nebo _"Související"_.

V tomto případě je label odebrán z DOMu pomocí `hidden`, protože vizuální label byl považován za zbytečný. Toto nepotlačuje mechanismus pro labeling; odečítačky pořád ohlásí _"complementary, související stránky"_ nebo podobně. Tam kde je to vhodné, můžete stále poskytnout vizuální label (odstraněním `hidden`). Pokud si myslíte, že je užitečné, aby byly karty součástí hlavní struktury stránky a tabulky s obsahem, můžete zároveň použít nadpis.

```html
<aside aria-labelledby="unique-card-label">
  <h2 id="unique-card-label">Související stránky</h2>
  <!-- Karta(y) tady -->
</aside>
```

Musíte zvolit nadpis úrovně, který je vizuálně schovaný daný kontext[^2]. Například, pokud karty mohou být považovány za potomka podsekce stránky, použijte `<h2>`. 

* Hlavní nadpis (`<h1>`)
    * Nadpis podsekce (`<h2>`)
    * Další nadpis podsekce (`<h2>`)
    * Související stránky (`<h2>`) ← landmark karty

### Skupina karet

Skupina karet by měla být označena jako list. To umožňuje strukturální a navigační vodítka pro odečítačky obrazovek[^3].

```html
<aside aria-labelledby="unique-card-label">
  <span class="visually-hidden" id="unique-card-label">More like this</span>
  <ul class="cards">
    <li class="card">...</li>
    <li class="card">...</li>
    <li class="card">...</li>
    <li class="card">...</li>
  </ul>
</aside>
```

### Samotná karta

Nejdůležitější věc ohledně komponenty **Karta** je si zapamatovat, že mají jediný účel: propagují/odkazují část služby justice; stránky, on-line služby, novou událost a tak dále.

#### Odkaz nadpisu

Karta musí obsahovat odkaz jako její hlavní label (nebo 'titulek'). Formulace tohoto odkazu by měla připomínat ten `<title>` a `<h1>` cílové stránky. Tato konzistence napomáhá kognitivní přístupnosti a vylepšuje <abbr title="search engine optimization">SEO</abbr>[^4].

```html
<div class="card">
  <div class="card__content">
    <a class="card__headline" href="path/to/content">Krajský soud</a>
  </div>
</div>
```

##### Nadpisy v titulcích
**Karty** nepotřebují mít nadpis (heading) jako svůj titulek/nadpis. Doplňující (complementary) landmark a/nebo list markup je brán jako dostačující pro poskytnutí sémantické struktury. Karty jsou pouze navigační vodítka.

#### Obrázky

Častokrát je použit obrázek jako pomoc k propagaci cílového obsahu karty. Toto je ve zdroji jediný povolený obsah _před_ odkazem.

```html
<div class="card">
  <div class="card__image">
    <img src="path/to/image" alt="">
  </div>
  <div class="card__content">
    <a class="card__headline" href="path/to/content">Krajský soud</a>
  </div>
</div>
```

Obrázek může a nemusí obsahovat `alt` text. Pokud titulek a/nebo text (kde je obsažen) dostatečně popisuje odkazovaný obsah, a obrázek nic dalšího nepřidává, použijte `alt=""`. 

Obrázek _nesmí_ být uvnitř svého vlastního odkazu, ať už obsahuje `alt` text nebo jinak. Toto by totiž představovalo nechtěnou a redundantní stopku při tabovaní na klávesnici.

#### Označení média

Můžete znázornit informaci o médiu, které odkazujete (pokud je to relevantní). Použijte prvek `class="card__indicator"`, uvnitř `class="card__image"`.

```html
<div class="card">
  <div class="card__image">
    <img src="path/to/image" alt="">
    <div class="card__indicator" aria-hidden="true">
      <svg class="icon icon--text" focusable="false">
        <use xlink:href="{{site.basedir}}static/images/icons-all.svg#icon-play"></use>
      </svg>
      <span class="card__indicator-text">04:35</span>
    </div>
  </div>
  <div class="card__content">
    <a class="card__headline" href="path/to/content">
      Krajský soud
      <span class="visually-hidden">Video, 4 minuty a 35 sekund</span>
    </a>
  </div>
</div>
```

Důležité je, aby `class="card__indicator"` byl schován před asistenčními technologiemi pomocí `aria-hidden="true"`. Je to proto, že by to nemělo smysl setkat se s ním před titulkem. Připojte čitelnou verzi informace ve vizuálně skrytém `span`u za text hlavního nadpisu (v příkladu _„Video, 4 minuty a 35 sekund“_).

#### Popis

Pokud je uveden, měl by se za nadpisem a před metadaty objevit popis (několik vět; víc ne). Viz následující příklad. Označení médií je pro stručnost z následujícího příkladu vynecháno.

```html
<div class="card">
  <div class="card__image">
    <img src="path/to/image" alt="">
  </div>
  <div class="card__content">
    <a class="card__headline" href="path/to/content">Krajský soud</a>
    <p>Jako první soud vyhrál cenu.</p>
  </div>
</div>
```

#### Metadata

[Základní vzhled](#metadata-1) komponenty **Metadata proužek** a [chování komponenty](#chování-pro-metadata) je pospán níže. Komponenta poskytuje metadata v páru klíčových hodnotza použití seznam definic `<dl>`[^8].

```html
<div class="card">
  <div class="card__image">
    <img src="path/to/image" alt="">
  </div>
  <div class="card__content">
    <a class="card__headline" href="path/to/content">Krajský soud</a>
    <p>Jako první soud vyhrál cenu.</p>
    <dl class="metadata-strip">
      <div>
        <dt class="visually-hidden">Publikováno:</dt>
        <dd>
          <span aria-hidden="true">
            <svg class="icon icon--text" focusable="false">
              <use xlink:href="path/to/icons-all.svg#icon-duration"></use>
            </svg>
            1m
          </span>
          <span class="visually-hidden">29. února 2019</span>
        </dd>
      </div>
      <div>
        <dt class="visually-hidden">Téma:</dt>
        <dd>
          <a href="link/to/category">Soudy</a>
        </dd>
      </div>
    </dl>
  </div>
</div>
```

* **`<dl>` a `<div>`:** Nejvíce vhodný markup pro klíč/hodnota informace je seznam definic (nebo 'popisů'). Je povoleno[^1] použít `<div>` prvky k zalomení párů prvků `<dt>` a `<dd>` pro účely layoutu
* **`class="visually-hidden"` pro `<dt>`:** Titulky definic (`<dt>`) jsou potřeba pouze pro output odečítačky obrazovek. Jsou vizuálně skryté pomocí třídy `visually-hidden`.
* **icon:** SVG ikonka (pokud je přítomna) je schovaná společně s textem. Bere `focusable="false"` pro její odebrání z pořadí focusuv některých verzích IE a Edge[^2]
* **`aria-hidden="true"`** V některých případech, vizuálně zobrazený text nemusí být dostačující pro syntetické hlasové oznámení. V těchto případech je zobrazený text (a související ikonografie) před asistivními technologiemi skryt pomocí `aria-hidden="true"`[^ 3] a alternativní formulace je poskytována nevizuálně (pomocí třídy je `visually-hidden` tento text skryt vizuálně).

## Doporučený layout

Sada karet by měla mít stejnou výšku. To je možné tím, že uděláte rodiče (`class="cards"`; `<ul>` pokud je více karet nebo jednoduchý `<div>` pokud je pouze jedna) jako kontext CSS Gridu. Je důležitá existence fallbacku, proto `display: inline-block` a `max-width` nad blokem `@supports`.

```css
.cards > * {
  display: inline-block;
  max-width: 266px;
}

@supports (display: grid) {
  .cards {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(266px, 1fr));
    grid-gap: 1rem;
  }
  
  .cards > * {
    max-width: none;
  }
}
```

Vzhled každé karty je vylepšen tím, že se metadata (pokud existují) dají na konec kontejneru. To je možné tím, že se karta a její vnořené `class="card__content"` udělají jako prvky Flexbox kontextů a prvku metadata se dá `margin-top: auto`.

```css
.card {
  display: flex;
  flex-direction: column;
}

.card__content {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
}

.card__content .metadata-strip {
  margin-top: auto;
}
```

### Obrázek

Obrázek bude muset vyplnit zbývající místo, nehledě na rozměry karty (které se napříč breakpointy budou měnit) bez rozmazání. To je možné nastavením požadované výšky obrázku a použitím vlastnosti 'object-fit'[^5]:

```css
@supports (object-fit: cover) {
  .card__image img {
    width: 100%;
    object-fit: cover;
  }
}
```

#### Podpora pro object-fit
Vlastnost `object-fit` je podporována všude mimo Internet Explorer. Kód používá `@supports` a fallbackuje na 'letterboxed' styl použitím `text-align: center` a nějaké `background-color`:

```css
.card__image {
  text-align: center;
  background-color: $color--black; 
}
```

### Horizontální karty

Horizontální konfigurace (s obrázkem vlevo od textového obsahu) může být dosažena umístěním `card--horizontal` třídy prvku `card`. To mění tak flex-direction prvku.

```css
.card.card--horizontal {
  flex-wrap: wrap;
  flex-direction: row;
}
```

Deklarace `flex-wrap: wrap` zajišťuje, že obrázek je _pouze_ umístěn na stranu, kde je místo. Prvkům `card__image` a `card__content` není dovoleno, aby byly menší než `266px` k vertikální kartě. V tuto chvíli se objevuje obalení a horizontální **Karta** se zobrazuje jako vertikální.

```css
.card.card--horizontal .card__image {
  flex: 1;
  min-width: 266px;
}

.card.card--horizontal .card__content {
  flex: 999; /* Vezmi všechny kromě 266px, pokud sousedí */
  min-width: 266px;
}
```

V grid kontextu je kartám se třídou `card--horizontal` nařízeno, aby zaujaly dva sloupce mřížky, ať už jsou umístěny kdekoli.

```css
.card.card--horizontal {
  grid-column: span 2;
}
```

### Focus styly

Focus styl `text-decoration` je doporučený pro odkaz nadpisu, spárován s jeho hover stylem. K dosažení jasnějšího zaměření karty a jejího nadpisu, může být použit přídavný `:focus-within` styl na samotnou kartu.

```css
.card__headline:hover,
.card__headline:focus {
  outline: none;
  text-decoration: underline;
}

.card:focus-within {
  outline: 0.25rem solid;
}
```

### Vysoký kontrast

Při aktivní Módu Vysokého kontrastu ve Windows jsou aplikovány neviditelné rámečky na všech stranách. Ty jsou pak viditelné při aktivním Windows HCM, a definují tvar karty při absenci `background-color`:

```css
.card {
  background-color: $color--;
  border: 2px solid transparent; /* pro vysoký kontrast */
}
```

### Metadata

`<dl>`, `<dt>`, a `<dd>` musí mít odstraněny jejich user agent styly. Potomci `<div>`y jsou nastaveny na `inline-block`, s `white-space: nowrap`.

```css
.metadata-strip > div {
  display: inline-block;
  white-space: nowrap;
}
```

Důvod `nowrap` je zajistit, že páry názvů definic a jejich definice budou zalomeny společně a nejsou nikdy rozděleny mezi řádky. To pomáhá skenování a porozumění obsahu.

Odělovací rámeček je zahrnut mezi — a pouze mezi — `<div>`y definice za použití pseudo-obsahu:

```css
.metadata-strip > div + div::before {
  content: '';
  border-left: 1px solid;
  margin: 0 0.25rem;
}
```

To je vhodnější než použití znaku jako pip ("\|"), který bude u některých odečítaček obrazovky neúmyslně oznámen.

#### Odkazy

Některé hodnoty metadat mohou být nalinkovány, například, "Téma" hodnota v příkladu [**Doporučeného markupu**](#doporučený-markup). Je důležité, aby tyto odkazy nebyly rozlišeny pouze barvou[^4]. Lidé, kteří jsou barevně slepí nebo kteří nepoužívají barevné displeje, nemohou vnímat barevné rozdíly a nebudou schopni rozlišit odkaz.

Pokud je tento styl již zděděn, doprovázejte jakoukoli barevnou diferenciaci s `text-decoration: underline`.

```css
.metadata-strip a {
  text-decoration: underline;
}
```

## Doporučené chování

Interakce s kartou musí být ergonomická pro myš, dotyk, klávesnici, a uživatelé odečítaček obrazovek. Pro myš a dotyk, je celá karta klikatelná jako jeden odkaz. Toho je dosaženo bez toho aniž by se záviselo na:

1. **Vícero, duplikovaných odkazů:** Odkázání každého z prvků se vytvoří několik a nadbytečných zarážek tabulátoru, což brání navigaci pomocí klávesnice[^6].
2. **Obalující odkaz:** Umístění veškerého obsahu karty dovnitř odkazu vytváří podrobnou, matoucí a/nebo zkrácený link label[^7], (což může ovlivnit jak SEO, a co je důležitější, prožitek ze čtení obrazovky)

Namísto toho je odkaz nadpisu udělán jako pseudo-obsah a je pozicována _přes_ celou kartu. 

```css
.card__headline::after {
  content: '';
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
}
```

Tímto ale vzniká problém s jakýmikoliv odkazy, které se objeví po nadpisu, nyní nebudou klikatelné. To je vyřešeno tím, že jsou odkazy přeneseny nad pseudo-obsah pomocí `position: relative`.

```css
.card a:not(.card__headline) {
  position: relative;
}
```

### Doplňkové odkazy
Odkaz s nadpisem (`.card__headline`) je primární důvod karty. Přestože **Metadata proužek** může obsahovat odkazy (na stránky s tématy) doplňkovým odkazům se vyhněte.

### Chování pro metadata

Kromě propojených metadat jsou Metadata proužky převážně statické. Pokud odkaz používá `target ="_blank"`, upozorněte uživatele začleněním ikony externího odkazu a nějakého vizuálně skrytého textu pro uživatele odečítačky obrazovky.

```html
<a href="/link/to/category">
  Soudy
  <span class="visually-hidden">(otevře se v novém okně)</span>
  <svg class="icon icon--text" aria-hidden="true" focusable="false">
    <use xlink:href="assets/svg/icons-core-set.svg#icon-external-link"></use>
  </svg>
</a>
```

## Související výzkum

Toto téma nemá související uživatelský výzkum.

### Pokračovat ve čtení, jinde na webu

[^1]: Complementary Landmark — W3C, <https://www.w3.org/TR/wai-aria-practices/examples/landmarks/complementary.html>
[^2]: How to structure headings for web accessibility — Nomensa, <https://www.nomensa.com/blog/2017/how-structure-headings-web-accessibility>
[^3]: "Basic screen reader commands for accessibility testing" by Léonie Watson, <https://developer.paciellogroup.com/blog/2015/01/basic-screen-reader-commands-for-accessibility-testing/>
[^4]: How To Write Page Titles For Google & Other Search Engines in 2018, <https://www.hobo-web.co.uk/title-tags/#page-titles-example-use>
[^5]: `object-fit` (caniuse data), <https://caniuse.com/#feat=object-fit>
[^6]: Keyboard Accessibility — WebAIM, <https://webaim.org/techniques/keyboard/>
[^7]: Accessibility and HTML5 Block Links — <https://simplyaccessible.com/article/html5-block-links/>
[^8]: The Description List element represents an association list consisting of zero or more name-value groups (a description list). see <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dl>