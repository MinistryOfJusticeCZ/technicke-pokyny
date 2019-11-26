---
category: Rozhodnutí pro komponenty
---

# Tabulky

## Úvod

Ministerstvo spravedlnosti se řídí dlouhodobými konvencemi pro strukturování tabulkových dat. Některá vylepšení jsou však zahrnuta v následujících příkladech.

Protože by _zalomování_ buněk tabulky, pro účely responzivního designu, dělalo nesmyslnou datovou strukturu, musí si datové tabulky zachovat své rigidní dvourozměrné uspořádání. Umístění tabulek s mnoha sloupci se proto stává otázkou umožňující horizontální scrollování. Protože se horizontálnímu scrollování obvykle vyhýbá a samo může být neočekávané, musí být použito pouze tam, kde je to nutné, a to zároveň s ohledem na použití vizuálních vodítek (takzvaná affordance), viz [**Doporučený layout**](#doporučený-layout), a interakce s klávesnicí.

V případě, že má tabulka značný počet řádků, implemetujte uzamknuté (sticky) záhlaví sloupců.

## Doporučený markup

Nehledě na šablonovací nebo renderovací technologii, tabulky musí být napsány jako `<table>` a tomu souvisejícími prvky. Tabulky vytvořeny z `<div>` prvků vyžadují značné použití ARIA atributů pro vyvolání očekávaného chování v softwarech odečítaček obrazovek a netriviální množství CSS k emulaci chování rozvržení podobného mřížce. 

### Záhlaví sloupce

Jakákoliv `<table>`, která nemá záhlaví sloupce nebo řádků (`<th>` prvky) nemůže být považována za opravdovou datovou tabulku. Dokonce některé odečítačky obrazovky, pokud narazí na tabulku bez záhlaví, se k takové tabulce chovají jako k tabulce pro způsob rozložení ('layout table') a komunikují ji docela jinak[^1].

#### Špatně: Příklad

```html
<table>
  <tr>
    <td>Falešné záhlaví sloupce 1</td>
    <td>Falešné záhlaví sloupce 2</td>
    <td>Falešné záhlaví sloupce 3</td>
  </tr>
  <tr>
    <td>Řádek 1, buňka 1</td>
    <td>Řádek 1, buňka 2</td>
    <td>Řádek 1, buňka 3</td>
  </tr>
  <tr>
    <td>Řádek 2, buňka 1</td>
    <td>Řádek 3, buňka 2</td>
    <td>Řádek 4, buňka 3</td>
  </tr>
</table>
```

#### Správně: Příklad

```html
<table>
  <tr>
    <th>Záhlaví sloupce 1</th>
    <th>Záhlaví sloupce 2</th>
    <th>Záhlaví sloupce 3</th>
  </tr>
  <tr>
    <td>Řádek 1, buňka 1</td>
    <td>Řádek 1, buňka 2</td>
    <td>Řádek 1, buňka 3</td>
  </tr>
  <tr>
    <td>Řádek 2, buňka 1</td>
    <td>Řádek 3, buňka 2</td>
    <td>Řádek 4, buňka 3</td>
  </tr>
</table>
```

Prvky záhlaví tabulky jsou _labely_ pro sloupcová data. Existence záhlaví sloupců umožňuje navigaci z buňky tabulky v jednom sloupci do buňky tabulky v sousedním sloupci, kde je obsah `<th>` nového sloupce oznámen pro kontext. To umožňuje uživatelům vizuálně procházet a porozumět tabulkám.

### Záhlaví řádku

Ve většině případů záhlaví sloupců stačí. Přesto, v některých tabulkách, první buňka každého řádku může být povážována jako 'key' buňka a chovat se jako zahlaví řádku. Doporučujeme explicitně odlišovat mezi záhlavím sloupce a řádku za použití atributu `scope`:

```html
<table>
  <thead>
    <tr>
      <th scope="col">Záhlaví sloupce 1</th>
      <th scope="col">Záhlaví sloupce 2</th>
      <th scope="col">Záhlaví sloupce 3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Záhlaví Řádku 1</th>
      <td>Řádek 1, buňka 2</td>
      <td>Řádek 1, buňka 3</td>
    </tr>
    <tr>
      <th scope="row">Záhlaví Řádku 2</th>
      <td>Řádek 3, buňka 2</td>
      <td>Řádek 4, buňka 3</td>
    </tr>
  </tbody>
</table>
```

Všimněte si, že `<table>` je teď rozdělena na hlavičku (`<thead>`) a tělo (`<tbody>`). Toto nemá žádný dopad na chování tabulky a jejích záhlaví. Ve chvíli, kdy tabulku _zlepšíme_, budou tyto prvky užitečné při stylizování a skriptování.

#### Prázdné záhlaví tabulky
V posledním příkladu výše, záhlaví prvního sloupce funguje jako záhlaví pro _záhlaví řádků_. Někdo toto považuje za zbytečné a buňku nechává prázdnou. Takovýto přístup nedoporučujeme, jelikož to může způsobovat nepředvídatelné chování[^2]. Zároveň je dobré být explicitní tam, kde je k tomu možnost.

### Popisek/legenda

Do teď jsme popisovali řádky a sloupce tabulky, ne tabulku samotnou. Můžete mít sklon tabulku označit prvkem pro nadpis, jako je `<h2>`. To by určitě pomohlo uživatelům procházet stránku vizuálně. Nadpis by však nebyl přímo asociován s tabulkou, což znamená, že uživatel odečítačky obrazovky hledající tabulku (pomocí zkratky jako <kbd>T</kbd> v NVDA) by neslyšel následně oznámený popis.

Namísto toho použijte `<caption>`[^3]. Tam, kde již existuje požadavek pro nadpis a vy chcete vyloučit opakování a redundanci, je dovoleno umístit nadpis _dovnitř_ prvku `<caption>`.

```html
<table>
  <caption>
    <h2>Příklad tabulky</h2>
  </caption>
  <!-- záhlaví a data tabulky -->
</table>
```

To bude znamenat, že se uživatelé odečítačky obrazovky mohou dostat ke tabulce pomocí klávesových zkratek nebo záhlaví, které poskytuje jejich software. V obou případech bude identifikována textem legendy/nadpisu.

## Doporučený layout

Důležité je, že mřížková struktura datových tabulek musí zůstat neporušená bez ohledu na dostupné místo. To znamená, že prvky nesmí zalamovat nebo jinak měnit polohu, protože budou označeny nesprávně. U tabulek s mnoha sloupci to může mít za následek vodorovný scrolling. Doporučujeme použít kontejner (container) s `overflow-x: auto`, který zajišťuje vodorovný scrolling.

```css
.table {
  overflow-x: auto;
}
```

Aby byl tento prvek posouvatelný klávesnicí, musí být nejprve zaměřitelný. To vyžaduje atribut `tabindex ="0"`. Pro uživatele odečítačky obrazovky bude tento nově interaktivní prvek potřebovat label. Doporučujeme, aby prvek převzal roli `group` a byl spojen s `<caption>`.

```html
<div class="table" role="group" aria-labelledby="caption" tabindex="0">
  <table>
    <caption id="caption">
      <h2>Příklad tabulky</h2>
    </caption>
    <!-- záhlaví a data tabulky -->
  </table>
</div>
```

#### Poznámka k interaktivitě
Nedoporučujeme, aby byl kontejner s tabulkou zaměřitelný by default. Tabulky, které nespouštějí scrollbar by vyústily v interaktivní kontejner, který nic nedělá. Tam, kde kontejner nepřetýká (does not overflow) odeberte `tabindex="0"` skrz `ResizeObserver`.

`tabindex="0"` je přidán do markupu od začátku, pokud není k dispozici JavaScript nebo `ResizeObserver`.

### Vizuální indikování funkcionality scrollu

V současné době pouze rozpůlení sloupce značí přetečení a možnost scrollovat. To neposkytuje dostatek vizuálních vodítek (affordance). Můžete proto použít orientační stín/vyblednutí na kteroukoli stranu, ke které dochází k přetečení.

Pro dosažení tohoto efektu je aplikována sada `linear-gradient`ů s různými hodnotami `background-attachment`:

```css
.table {
  overflow-x: auto;
  background-color: #fff;
  background-image: 
    linear-gradient(90deg, #fff 0%, transparent 4rem),
    linear-gradient(90deg, rgba(0, 0, 0, 0.3) 0%, transparent 2rem),
    linear-gradient(270deg, #fff 0%, transparent 4rem),
    linear-gradient(270deg, rgba(0, 0, 0, 0.3) 0%, transparent 2rem);
  background-attachment: local, scroll;
}
```

Pokud nedojde k přečetní, bílý `local` gradient skrývá šedý `scroll`[^4].

### Uzamknuté (sticky) záhlaví

Tam, kde je mnoho řádků, je možné scrollovat až zmizí záhlaví. To ztěžuje interpretaci dat (protože by to vyžadovalo buď dobrou paměť, nebo hodně scrollování nahoru a dolů). Konvenčním řešením tohoto problému je učinit řádek záhlaví sloupce „uzamknutým“. Ten pak uživatele následuje po stránce.

To je nyní možné v CSS s `position: sticky`. Avšak kontejnery s explicitním `overflow`, jaké se používá v sekci nahoře, zruší chování `position: sticky`. Následující je proto poskytováno jako zlepšení pro tabulky, které nevyvolávají přetečení.

```css
.table th {
  position: sticky;
  top: 0;
}
```

Tam, kde nedojde k přečetní, je styl `overflow-x: auto` dynamicky odstraněn skrz `ResizeObserver`.

## Doporučené chování

Jak bylo popsáno v [**Doporučeném layoutu**](#doporučený-layout), chování pro přetečení/scrolling je řešeno progresivně, pomocí první funkce detekující `ResizeObserver`. 

```js
if ('ResizeObserver' in window) {
  var ro = new ResizeObserver(entries => {
    for (var entry of entries) {
      var cr = entry.contentRect;
      var noScroll = cr.width >= this.table.offsetWidth;
      entry.target.tabIndex = noScroll ? -1 : 0;
      entry.target.style.overflowX = noScroll ? 'visible' : 'auto';
      this.thead.classList.toggle('sticky', noScroll);
    }
  });

  ro.observe(elem);
}
```

### Řazení

Funkcionalita pro řazení je poskytována tam, kde je druhý argument konstruktoru je nastaven na `true`.

```js
var tableContainer = document.querySelector('.table');
var table = new Table.constructor(tableContainer, true);
```

Tabulka je progresivně zlepšována tak, aby každému sloupci přidávala tlačítko pro řazení. Tyto tlačítka pak nesou popis _'Seřadit'_ za použití vizuálně skrytého prvku `<span class="visually-hidden" />`.

```html
<th scope="col" aria-sort="none">
  Týmy
  <button>
    <span class="visually-hidden">Seřadit</span> 
    <svg viewBox="0 0 32 32" class="icon icon--text" focusable="false" aria-hidden="true">
      <path d="M18.033 25.5v-19l5.6 5.7 2.4-2.4-10-9.8-10 9.8 2.4 2.4 5.6-5.7v19l-5.6-5.7-2.4 2.4 10 9.8 10-9.8-2.4-2.4"></path>
    </svg>
  </button>
</th>
```

Ve chvíli, kdy uživatel klikne na tlačítko seřazení, je upřednostněno vzestupné řazení a hodnota `aria-sort` je přepnuta z `none` do `ascending`. Další kliknutí na stejné tlačítko přepíná pořadí mezi vzestupem a sestupem (`aria-sort="descending"`). Všechny sloupce, které nejsou použity k řazení, mají záhlaví s hodnotou `none`.

## Související výzkum

Toto téma nemá související uživatelský výzkum.

### Pokračovat ve čtení, jinde na webu

[^1]: Layout Tables Versus Data Tables — WebAim, <https://webaim.org/techniques/tables/#uses>
[^2]: VoiceOver and Tables with an Empty First Header Cell, <http://accessibleculture.org/articles/2010/10/voiceover-and-tables-with-an-empty-first-header-cell/>
[^3]: The Table Caption element — MDN, <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/caption>
[^4]: Pure CSS scrolling shadows with `background-attachment: local`, <http://lea.verou.me/2012/04/background-attachment-local/>