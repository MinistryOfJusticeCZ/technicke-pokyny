---
category: Rozhodnutí pro základní prvky
---

# Grid

## Úvod

Vizuální rozvržení by mělo být efektivní a konzistentní, aniž by to narušovalo strukturu dokumentu a přístupnost. Následující pokyny staví na progresivních zlepšeních, aby se využilo nejnovějšího vývoje v rozvržení CSS.

## Doporučený markup

Mřížka (grid) je nezbytně složena z jednotlivých _položek_ mřížky, které společně tvoří množinu nebo skupinu. Někdy by položky měly být vnímány jako související a v jiných případech jsou pouze seskupeny dohromady, aby bylo dosaženo vizuálního uspořádání.

Vidomému uživateli jsou obvykle poskytovány vizuální podněty, že dvě nebo více sousedních položek mřížky patří do sady, jako je například rozdíl ve stylu a rozložení. Nevidomý uživatel se musí místo toho spoléhat na sémantiku nebo její nedostatek, který jim sděluje jejich software pro odečítání z obrazovky.

Pokud položky mřížky představují libovolné kontejnery, které se používají výhradně k umisťování položek do mřížkové formace, lze použít prvek `<div>`.

### Špatný příklad

(Sémantika tabulky není vhodná, protože obsah není tabulkový.)

```html
<table>
  <tr class="layout">
    <td class="layout__item size-onehalf">Jedna položka</td>
    <td class="layout__item size-onehalf">Další nesouvisející položka vpravo</td>
  </tr>
</table>
```

### Správný příklad

```html
<div class="layout">
  <div class="layout__item size-onehalf">Jedna položka</div>
  <div class="layout__item size-onehalf">Další nesouvisející položka vpravo</div>
</div>
```

Prvek `<div>` není v prohlížeči reprezentován ve stromu zpřístupnění[^1], a proto nepředstavuje uživateli žádné nepřiměřené nebo neočekávané sémantické informace. Bude interpretován pouze obsah položek, kde je poskytováno sémantické HTML.

V některých případech budou mít položky samostatnou sémantiku, kterou budou moci vyjádřit. V takovém případě použijte pro položky mřížky vhodné sémantické prvky. V následujícím příkladu tvoří související `<main>` a `<aside>` širší sloupec obsahu a užší postranní panel.

### Špatný příklad

(CSS třídy, které nekomunikují sémantické informace k accessability API, se používají k odlišení prvků.)

```html
<div class="layout">
  <div class="main layout__item size-twothirds">Obsah</div>
  <div class="sidebar layout__item size-onethirds">Boční panel</div>
</div>
```

### Správný příklad

(Použity správné landmarky prvky.)

```html
<div class="layout">
  <main class="layout__item size-twothirds">Obsah</main>
  <aside class="layout__item size-onethirds">Boční panel</aside>
</div>
```

### Gridy jako seznamy

Pokud položky tematicky patří do sady, standardem je označit je jako seznam. Tím se navíc splní **WCAG2.1 1.3.1 Info and Relationships**[^2].

Seznamy jsou identifikovány asistivními technologiemi a jejich položky jsou vyjmenovávány. Mnoho odečítaček obrazovky také poskytuje zkratky pro procházení položkami seznamu, například [klávesa <kbd>i</kbd> v NVDA](https://webaim.org/resources/shortcuts/nvda).

Mřížku naplánovaných jednání lze považovat za tematickou množinu, protože každá položka je ve třídě "jednání".

```html
<ul class="layout">
  <li class="layout__item size-onefourths">Krajský soud</li>
  <li class="layout__item size-onefourths">Okresní soud</li>
  <li class="layout__item size-onefourths">Městský soud</li>
  <li class="layout__item size-onefourths">Vrchní soud</li>
</ul>
```

Seznamy obsahující potomky, které nejsou položkami seznamu, jsou neplatné a mohou v prohlížečích způsobit nekonzistentní výstup. Takové parsing problémy jsou považovány za selhání podle **WCAG2.1 4.1.1 Parsing**[^3].

Správný způsob, jak začlenit řádku do seznamu je proto používat WAI-ARIA:

```html
<div role="list">
    <div role="presentation" class="layout">
        <div role="listitem" class="layout__item size-onefourths">Krajský soud</div>
        <div role="listitem" class="layout__item size-onefourths">Okresní soud</div>
        <div role="listitem" class="layout__item size-onefourths">Městský soud</div>
        <div role="listitem" class="layout__item size-onefourths">Vrchní soud</div>
    </div>
    <div role="presentation" class="layout">
        <div role="listitem" class="layout__item size-onefourths">Krajský soud 1</div>
        <div role="listitem" class="layout__item size-onefourths">Okresní soud 1</div>
        <div role="listitem" class="layout__item size-onefourths">Městský soud 1</div>
        <div role="listitem" class="layout__item size-onefourths">Městský soud 2</div>
    </div>
</div>
```

V příkladě je grid definován za pomoci `role="list"`. Potomci jsou explicitně odebrány ze stromu zpřístupnění za pomoci `role="presentation"`, a položky jsou označeny jako `role="listitem"`. Výsledkem je, že asistivní technologie správně hlásí jeden seznam zapouzdřující osm položek seznamu.

#### Kompatibilita a role presentation
Aplikace `role="presentation"`[^4] na `<div>` by měla být redundantní, jelikož `<div>` už nemá žádnou implicitní `role`. Nicméně, některé prohlížeče, společně s odečítačkami obrazovek, se k potomkům od `role="list"` chovají jako k položce seznamu. Z toho důvodu použití explicitní `role`.

### Nadpisy

Nadpisy společně se seznamy vytvářejí nevnímatelnou strukturu a poskytují navigační vodítka[^2]. Stránka by měla liberálně, ale vhodně používat nadpisy, ať už je rozvržení rozděleno do mřížky nebo existuje jako jediný sloupec textu.

Je zcela legitimní umístit nadpisy do položek seznamu a je výhodné, pokud představují položky mřížky, které obsahují hodně strukturovaného obsahu. Mějte však na paměti, že seznam představuje _plochou_ hierarchickou strukturu, takže každý nadpis by měl být na stejné úrovni, aby to odrážel. Tato úroveň je určena úrovní nadpisu, který seznam sám představuje.

```html
<h2>Nadcházející jednání</h2>
<ul class="layout">
    <li class="layout__item size-onefourths">
        <h3>Krajský soud</h3>
        <p>[popis]</p>
    </li>
    <li class="layout__item size-onefourths">
        <h3>Okresní soud</h3>
        <p>[popis]</p>
    </li>
    <li class="layout__item size-onefourths">
        <h3>Městský soud</h3>
        <p>[popis]</p>
    </li>
</ul>
```

Struktura vnoření pro výše uvedený příklad vytvoří následující obrys, přičemž „Nadcházející jednání“ označí seznam podsekcí jednání.

* Nadcházející jednání
    * Krajský soud
    * Okresní soud
    * Městský soud

Pro více informací, viz [**Napisy**](../headings).

### Tabulky

Datové tabulky jsou čteny shora dolů a zleva doprava, záhlaví podél vrcholu a (někdy) dolů zleva. Je nezbytné, aby vizuální struktura odpovídala sémantické struktuře ve všech výřezech: prvkům tabulky nesmí být dovoleno se zalamovat a musí se nacházet pod různými záhlavími.

Neaplikujte grid na datové tabulky. Ty by měly být označeny pomocí `<table>`, `<th>` (table header) a `<td>` prvky, vyvolávající základní, požadovaný layout skrz user agent stylizaci.

Vzhledem k tomu, že obtékání není přípustné, je vytvoření responzivní tabulky věcí, která uživateli umožní vodorovně posouvat tabulku. Chcete-li to zpřístupnit, musíte:

1. Obalit prvek tabulky s `overflow-x: auto`
2. Udělat tento prvek zaměřitelný pomocí `tabindex="0"`, takže může být scrollovatelný pomocí klávesnice
3. Označte takto zaměřitelný prvek pro uživatele odečítaček obrazovek, nejlépe pomocí titulku tabulky (caption)

[Inclusive components: Data tables](https://inclusive-components.design/data-tables/) má k tomuto příklad. Všimněte si role `group`, který vyvolá oznámení role spolu s přidruženým labelem prvku.

```html
<div class="table-container" tabindex="0" role="group" aria-labelledby="caption">  
  <table>
    <caption id="caption">Grindcore bands</caption>
    <!-- table content -->
  </table>
</div>
```

## Doporučený layout

### Pořadí

Doporučujeme použít Flexbox, jelikož pak máte možnost změnit pořadí položek mřížky v layout pomocí vlastnosti `order`.

```html
<style>
.promote {
    order: 1;
}
</style>

<div class="layout">
  <div class="layout__item size-onethirds">První položky</div>
  <div class="layout__item size-onethirds" class="promote">Druhá</div>
  <div class="layout__item size-onethirds">Třetí</div>
</div>
```

V tomto příkladě, je položka `class="promote"` "povýšena" z druhé pozice na první. To způsobuje problémy nevidomým, slabozrakým a uživatelům klávesnice.

* **Nevidomí uživatelé:** Markup konzumovaný uživatelovou odečítačkou obrazovky zůstává v původním pořadí, což znamená, že existuje rozdíl mezi jejich prožitkem a prožitkem vidomých uživatelů
* **Slabozrací uživatelé:** Pokud uživatel používá odečítačku obrazovky, existuje rozdíl mezi vizuálním uspořádáním, které může částečně vnímat, a pořadí, v jakém obsah odečítačka obrazovky oznamuje. To může způsobit selhání pod **WCAG2.1 1.3.2 Meaningful Sequence**[^5].
* **Uživatelé klávesnice:** Všechny interaktivní/zaměřitelné prvky obdrží focus v pořadí, které je v rozporu s pořadím, ve kterém se objevují. Jedná se o selhání podle **WCAG2.1 2.4.3 Focus Order**[^6].
 
Z těchto důvodů by se ve většině případů mělo jiné vizuální pořadí dosáhnout úpravou pořadí ve zdroji - nikoli využitím vlastnosti `order`.

### Jazyky psané zprava doleva (RTL) 

Flexbox vnitřně podporuje `direction` tak, že obrátí pořadí položek mřížky tam, kde je `dir="ltr"` přepnuto do `dir="rtl"`, nebo CSS vlastnost `direction` je přepínána mezi `ltr` a `rtl`. 

Budete muset explicitně obrátit vaše pořadí mřížky ve vašem CSS, jelikož přepnout do RTL `lang` automaticky nemění směr[^7]. V následujícím příkladě, `my-grid` přepíná směr jeho položek na stránce do arabštiny.

```css
:lang(ar) .my-grid {
    direction: rtl;
}
```

V závislosti na obsahu položek mřížky a jejich vzájemných vztazích nemusí být nutné ani vhodné přepínat směr mřížky, a to ani v kontextu jazyka psaného zprava doleva.

### CSS Grid zlepšení

CSS Grid modul[^8] je v určitých případech vhodnější než Flexbox. Pomocí CSS Grid je možné vytvořit responzivní mřížku neurčeného počtu položek, kde každá položka automaticky vyhovuje stejné šířce, bez ohledu na chování při zalomování. Toto chování odstraňuje nutnost breakpointů `@ media` a je zvláště užitečné při uspořádání sad [**Karet**](../karty-metadata).

V patternu [**Karty**](../karty-metadata), je progresivní zlepšení dosaženo za použití `@supports`:

```css
.cards > * + * {
  list-style: none;
  margin-top: 1rem;
}

@supports (display: grid) {
  .cards > * + * {
    margin-top: 0;
  }

  .cards {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(266px, 1fr));
    grid-gap: 1rem;
  }
}
```

Všimňte si, jak je nastavené odsazení(margin) nad blokem `@supports` odstraněno uvnitř bloku `@supports`, protože je nahrazeno `grid-gap`. Minimální mezera mezi buňkami mřížky je `8px` (`0.5rem`).

## Doporučené chování

### Zvětšení

Někteří uživatelé zvětšují[^9] své webové stránky pomocí doplňkového softwaru, jako je [ZoomText](https://www.zoomtext.com/products/zoomtext-magnifierreader/). Ostatní budou používat „zvětšení na celou stránku“, stisknutím <kbd>Cmd</kbd> (nebo <kbd>Ctrl</kbd>) a <kbd>+</kbd>.

Celostránkové přiblížení spouští breakpointy stejným způsobem, jakým by se fyzicky zužoval nebo rozšiřoval viewport. To znamená, že responzivní rozhraní podporují zvětšení a zrakově postižené. Stačí se ujistit, že položky _uvnitř_ položek gridu jsou také responizivní. To znamená:

* Vyhýbaní se fixním šířkám tak, nastavené pomocí `width`, že obsah mřížky nepřetéká
* Vyhýbaní se fixním výškám, nastavené pomocí `height`, to by způsobilo překrývání zalomeného obsahu
* Vyhýbaní se `white-space: nowrap`, které zabraňuje zalomení textu

## Související výzkum

Toto téma nemá související uživatelský výzkum.

### Pokračovat ve čtení, jinde na webu

[^1]: The Accessibility Tree — Google Developers, <https://developers.google.com/web/fundamentals/accessibility/semantics-builtin/the-accessibility-tree>
[^2]: WCAG2.1 1.3.1 Info & Relationships, <https://www.w3.org/TR/WCAG21/#info-and-relationships>
[^3]: WCAG2.1 4.1.1 Parsing, <https://www.w3.org/TR/WCAG21/#parsing>
[^4]: Using the presentation role — MDN, <https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_presentation_role>
[^5]: WCAG2.1 1.3.2 Meaningful Sequence, <https://www.w3.org/TR/WCAG21/#meaningful-sequence>
[^6]: WCAG2.1 2.4.3 Focus Order, <https://www.w3.org/TR/WCAG21/#focus-order>
[^7]: `:lang()` — MDN, <https://developer.mozilla.org/en-US/docs/Web/CSS/:lang>
[^8]: CSS Grid Layout Module Level 1 — W3, <https://www.w3.org/TR/css-grid-1/#overview-grid>
[^9]: How to make your site accessible for screen magnifiers — Axess Lab, <https://axesslab.com/make-site-accessible-screen-magnifiers/>