---
category: Rozhodnutí pro komponenty
---

# Taby

## Úvod

Rozhraní s taby, stejně jako akordéony, umožňují uživatelům prohlížet dlouhý obsah po jedné sekci. Jasně označené taby představující jednotlivé sekce usnadňují uživatelům identifikaci a rozbalení toho obsahu, který se jich týká.

Používejte taby, u nichž nejsou části předmětu příliš početné (celkem více než čtyři nebo pět tabů) a názvy tabů nejsou dlouhé. Obsah tabu by měl být soběstačný: Nenuťte uživatele, aby přepínali mezi taby pro dokončení úkonů[^1]. Pokud je přítomno více než pět sekcí/názvů, nebo jsou-li názvy tabů dlouhé, použijte pattern [**Akordéony**](/akordeony.md).

### Odchýlení se od ARIA authoring practices
Tato implementace tabů se ve svém chování liší od specifikace uvedené v ARIA Authoring Practices[^2]. Je to z důvodu, že chceme řešit problémy s použitelností, které byly nalezeny, jak v interním tak v externím výzkumu[^3] na ARIA tab rozhraní.

## Doporučený markup

### Základní markup

Tam, kde je možný server-side rendering a progresivní zlepšování, postupujte podle tohoto příkladu pro tvorbu tabů. Tam, kde je spuštěn JavaScript, bude tato tabulka obsahu stránky (propojená na následující sekce obsahu) zlepšena přidělením ARIA atributu.

```html
<div class="tabs">
  <ul>
    <li>
      <a href="#section1">Sekce 1</a>
    </li>
    <li>
      <a href="#section2">Sekce 2</a>
    </li>
    <li>
      <a href="#section3">Sekce 3</a>
    </li>
  </ul>
  <section id="section1" tabindex="-1">
    <h2>Sekce 1</h2>
    <p>Obsah sekce 1.</p>
  </section>
  <section id="section2" tabindex="-1">
    <h2>Sekce 2</h2>
    <p>Obsah sekce 2.</p>
  </section>
  <section id="section3" tabindex="-1">
    <h2>Sekce 3</h2>
    <p>Obsah sekce 3.</p>
  </section>
</div>
```

#### Poznámky

* **`<ul>`:** `<ul>` seskupuje odkazy na stejné stránce dohromady a výstup odečítačky obrazovky je pak vyjmenovává.
* **odkazy:** Každý `href` odkaz koresponduje s `id` sekce (`<section>`). Řešení staví na standardním chování prohlížeče, to znamená, uživatel může navigovat sekce (`<section>`) bez toho, aby se spoléhal na JavaScript.
* **`<section>`**: Některé odečítačky obrazovek umožňují uživatelům navigaci mezi `<section>` ('region') napřímo, pomocí zkratek. Přesto, doporučujeme, aby každá `<section>` byla také uvedena nadpisem, protože zkratky pro hledání nadpisů jsou dlouhodobějším způsobem pro navigaci.
* **tabindex="-1":** Každá `<section>` bere `tabindex="-1"`. To nutí prohlížeče přesunout focus na cílený prvek při aktivaci odkazu na té samé stránce[^4].

### Zlepšený markup

```html
<div class="tabs">
  <ul role="tablist">
    <li role="presentation">
      <a role="tab" id="tab-section1" href="#section1" aria-selected="true">Sekce 1</a>
    </li>
    <li role="presentation">
      <a role="tab" id="tab-section2" href="#section2" aria-selected="false">Sekce 2</a>
    </li>
    <li role="presentation">
      <a role="tab" id="tab-section3" href="#section3" aria-selected="false">Sekce 3</a>
    </li>
  </ul>
  <section role="tabpanel" id="section1" aria-labelledby="tab-section1" tabindex="-1">
    <h2>Sekce 1</h2>
    <p>Obsah sekce 1.</p>
  </section>
  <section role="tabpanel" id="section2" aria-labelledby="tab-section2" tabindex="-1" hidden>
    <h2>Sekce 2</h2>
    <p>Obsah sekce 2.</p>
  </section>
  <section role="tabpanel" id="section3" aria-labelledby="tab-section3" tabindex="-1" hidden>
    <h2>Sekce 3</h2>
    <p>Obsah sekce 3.</p>
  </section>
</div>
```

#### Poznámky

* **`role="tablist"`, `role="tab"`, a `role="tabpanel"`:** Ve chvíli, kdy běží JavaScript, rozhraní tabu získává potřebnou sémantiku, která má být sdělena jako tab rozhraní v softwaru asistivních technologiích.
* **`role="presentation"`:** S danými `role="tablist"` a `role="tab"` jsou taby ohlašovány správně jako _"tabs"_ a jsou vyčteny. Sémantika seznamu je pak redundantní, a `role="presentation"` je použita k jejímu potlačení[^5].
* **`aria-labelledby`:** Tab panely (`<section>` s `role="tabpanel"`) jsou popsány (labelled) jejich taby, takže ve chvíli, kdy je uživatel zaměří nebo je otevře, dávájí smysl celému rozhraní jako celku. Když je tab panel zaměřen nebo otevřen, většina odečítaček obrazovky ohlásí něco podobného jako _"tab panel, sekce 1"_.
* **`aria-selected="true"` a `hidden`:** Atribut stavu identifikuje tab korespondující otevřenému/vybranému panelu. Všechny ostatní panely jsou odstraněny (vizuálně a odečítačkám obrazovek) použitím vlastnosti `hidden`.

## Doporučený layout

Ve chvíli, kdy JavaScript neběží, taby se zobrazují jako odkazy v něčem, co připomíná tabulku s obsahem stránky. Každá `<section>` je viditelná po celou dobu, a aktivováním odkazu (jakýmsi tabem) se focus přesune na korespondující sekci, ukazující jeho focus styl. Doporučujeme jasný, plný outline styl.

```css
.tabs > ul a:focus,
.tabs > section:focus {
  outline: 2px solid;
  outline-offset: -2px;
}
```

Pokud běží JavaScript, odkazy vypadají jako taby, a jsou zobrazeny vedle sebe. Kratší názvy jsou preferovány namísto zalomeného textu, takže `white-space: nowrap` a `text-overflow: ellipsis` jsou aplikovány by default. Pamatujte, že tohle funguje pouze, pokud je na flex potomkovi (tedy `<li>`) použito `min-width: 0`.

```css
.tabs > ul li {
  min-width: 0;
}

.tabs > ul a {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

To však může snadno vést k nečitelným názvům (labelům) pro úzké viewporty (například, mobil). Doporučujeme vytvořit breakpoint na základě obsahu, ve kterém se taby překonfigurují do jednoho sloupce. To by mohlo být implementováno jako následující:

```css
@media (max-width: 400px) {
  .tabs > ul {
    flex-wrap: wrap;
  }

  .tabs > ul li {
    flex-basis: 100%;
    margin: 0;
    margin-bottom: 0.25rem;
  }
}
```

### Mód Vysokého kontrastu

Tam, kde je aktivní Mód Vysokého kontrastu (HCM) ve Windows, jsou odstraněny pozadí, které označují taby a panely. Na základě toho jsou přidány další rámečky (borders). Ty jsou nastaveny jako `transparentní` a budou neviditelné, pokud nebude spuštěn Windows HCM.

```css
.tabs > section {
  border: 2px solid transparent; /* kvůli módu vysokého kontrastu */
}
```

Dále, je použit `@media` query detekující mód vysokého kontrastu[^7] pro vytvoření alternativního `aria-current` stylu pro právě vybraný tab. Je napozicován `2px` dolů od své přirozené pozice. Tím se zakrývá čára mezi tabem a panelem, takže vypadají jako jedna.

```css
@media (-ms-high-contrast: active) {
  .tabs [aria-current] {
    position: relative;
    top: 2px;
  }
}
```
![Rámeček (border) definuje taby a panely](/images/hcm_tabs.png)


## Doporučené chování

### Bez JavaScriptu

Ve chvíli, kdy JavaScript neběží, taby se zobrazují jako odkazy v něčem, co připomíná tabulku s obsahem stránky. Jak bylo popsáno v [doporučení pro layout](#doporuceny-layout), každá `<section>` je viditelná po celou dobu, a aktivováním odkazu (jakýmsi tabem) se focus přesune na korespondující sekci, ukazující jeho focus styl. Doporučujeme jasný, plný outline styl.

### S JavaScriptem

#### Výběru tabu

Kliknutím myší nebo klepnutím na tab zobrazíte jemu odpovídající panel tabu (tab panel). Pro uživatele klávesnice jsou nevybrané taby zaměřitelné a lze je aktivovat klávesami <kbd>Enter</kbd> a <kbd>Space</kbd>.

Pro zachování chování odkazů na stejné stránce (na základě kterých jsou taby vytvořeny) a pro řešení problémů, které odečítačky obrazovek mají při změně z tabu do tab panelu, kliknutím na tab přesuňte focus na zobrazený tab panel. V odečítačkách obrazovek je tab panel identifikován jako tab panel, a název (label) tab panelu (půjčený z odpovídajícího tab pomocí `aria-labelledby`) je také ohlášen.

Tab panel je teď počáteční bod pořadí focusu. Přesto tab panel není uživatelem zaměřitelný (bere `tabindex="-1"`, ne `tabindex="0"`), to znamená <kbd>Shift</kbd> + <kbd>Tab</kbd> přesune uživatele zpět na tab list. Chování "virtuální kurozoru"[^6] odečítaček obrazovek  není nijak potlačeno nebo přepsáno. Prvky (prvky s `tabindex="0"`) které jsou uživatelem zaměřitelné, ale nejsou interaktivní, jsou považovány jako porušení **WCAG 2.1 2.4.3 Focus Order**[^8].

#### Podpora zpětného tlačítka

Protože je rozhraní řízeno `hashchange` eventy vyvolanými kliknutím na prvky tab odkazu, je podporováno tlačítko Zpět. Stisknutím klávesy zpět se dostanete na dříve otevřený tab, pokud tam byl.

```js
window.addEventListener('hashchange', function (e) {
  var selected = tablist.querySelector('[aria-current]');
  var oldIndex = selected ? Array.prototype.indexOf.call(tabs, selected) : undefined;
  switchTab(oldIndex, tabInfo());
}, false);
```

Kdykoliv se hash změní do něčeho, co _nekoresponduje_ s tabem, je první tab panel zobrazen, ale nemá focus (jelikož to není část dokumentu (document fragment), který si uživatel zrovna vyžádal).

### Načtení stránky

Jak jsou taby vybrány, mají taby povoleno pokračovat v aktualizaci hashe (`hash`) dokumentu, kvůli zachování chování odkazů na stejné stránce. Při prvním načtení dokumentu, pokud hash URL odpovídá tab panelu, je tento panel standardně zobrazen. Pokud k adrese URL neexistuje žádná hashovací komponenta nebo hash neodpovídá `id` panelu tabu, zobrazí se první tab panel a vybere se první tab.

Níže uvedený kód je snippetem takové implemetance:

```js
window.addEventListener('DOMContentLoaded', function () {
  switchTab(undefined, tabInfo());
});
```

## Související výzkum

Toto téma nemá související uživatelský výzkum.

### Pokračovat ve čtení, jinde na webu

[^1]: Tabs, Used Right — Nielsen Norman Group, <https://www.nngroup.com/articles/tabs-used-right/>
[^2]: Tabs: ARIA Authoring Practices, <https://www.w3.org/TR/wai-aria-practices-1.1/#tabpanel>
[^3]: Danger! ARIA Tabs — Simply Accessible, <https://simplyaccessible.com/article/danger-aria-tabs/>
[^4]: In-Page Links and Input Focus — Accessible Culture, <http://accessibleculture.org/articles/2010/05/in-page-links/>
[^5]: Removing Semantics Using The Presentation Role — Accessibility Developer Guide, <https://www.accessibility-developer-guide.com/examples/sensible-aria-usage/presentation/>
[^6]: Reading Commands and Cursors — Freedom Scientific, <https://doccenter.freedomscientific.com/doccenter/doccenter/rs11f929e9c511/2012-04-24_teachersandtrainers-l7/02_jawsandmagicreadingcommandsandcursors.htm>
[^7]: `-ms-high-contrast` @media query — MDN, <https://developer.mozilla.org/en-US/docs/Web/CSS/@media/-ms-high-contrast>
[^8]: WCAG2.1 2.4.3 Focus Order — W3C, <https://www.w3.org/TR/WCAG21/#focus-order>