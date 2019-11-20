---
category: Rozhodnutí pro komponenty
---

# Akordeóny

## Úvod

**Akordéony** slouží podobnému účelu jako **Taby**: obalují obsah do svého vlastního interaktivního přehledu.

Podle všeho, interakce akordéonu je jednodušší než standardní tab interface[^1]. Nespoléhá se totiž na ať už naprogramovaný focus, nebo na šipky na klávesnici jako způsob navigace.

Akordeony jsou také lepším řešením, pokud jde o responzivní design: jejich vizuální struktura nezávisí na prvcích („tabech“), které se pak objevují v horizontální linii. Některá řešení pro taby jsou navržena tak, aby se sbalila (collapse) do vizuální struktury podobné akordeonům, kde jeden tab je jeden řádek. To je problematické, protože to způsobuje, že se sice taby podobají akordéonu, ale nechovají se jako akordéon.

## Doporučený markup

Markup sémantika by se měla lišit v závislosti na kontextu a množství vašeho obsahu.

Například obsah představující hlavní sekce obsahu stránky může být označen `<section>` a nadpisy:

```html
<h1>Hlavní nadpis stránky</h1>
<section>
  <h2>Sekce 1</h2>
  <p>Nějaký text...</p>
  <img src="http://www.example.com/path/to/image" alt="popis">
  <p>Více textu...</p>
</section>
<section>
  <h2>Sekce 2</h2>
  <p>Nějaký text sem...</p>
  <p>Další text...</p>
</section>
<section>
  <h2>Sekce 3</h2>
  <img src="http://www.example.com/path/to/image" alt="další popis">
  <blockquote>Citace odněkud</blockquote>
</section>
```

Obalením tohoto seznamu `<section>` do prvku `class="accordion"` produkuje následující _zlepšený_ markup, pokud je povolený JavaScript:

```html
<h1>Hlavní nadpis stránky</h1>
<div class="accordion">
  <section>
    <h2 class="accordion__handle">
      <button aria-expanded="false" type="button">
        <span>Sekce 1</span>
        <svg viewBox="0 0 32 32" class="icon icon--text" focusable="false" aria-hidden="true">
        <path d="M16 29L32 3h-7.2L16 18.3 7.2 3H0"></path></svg>
      </button>
    </h2>
    <div class="accordion__drawer" hidden>
      <p>Nějaký text...</p><img src="http://www.example.com/path/to/image" alt="popis">
      <p>Více textu...</p>
    </div>
  </section>
  <section>
    <h2 class="accordion__handle">
      <button aria-expanded="false" type="button">
        <span>Sekce 2</span>
        <svg viewBox="0 0 32 32" class="icon icon--text" focusable="false" aria-hidden="true">
        <path d="M16 29L32 3h-7.2L16 18.3 7.2 3H0"></path></svg>
      </button>
    </h2>
    <div class="accordion__drawer" hidden>
      <p>Nějaký text sem...</p>
      <p>Další text...</p>
    </div>
  </section>
  <section>
    <h2 class="accordion__handle">
      <button aria-expanded="false" type="button">
        <span>Sekce 3</span>
        <svg viewBox="0 0 32 32" class="icon icon--text" focusable="false" aria-hidden="true">
        <path d="M16 29L32 3h-7.2L16 18.3 7.2 3H0"></path></svg>
      </button>
    </h2>
    <div class="accordion__drawer" hidden>
      <img src="http://www.example.com/path/to/image" alt="další popis">
      <blockquote>Citace odněkud</blockquote>
    </div>
  </section>
</div>
```

* `class="accordion__drawer"` a `hidden`: Veškerý obsah mimo 'handle' je seskupen uvnitř tohoto prvku, takže jeho viditelnost může být jednoduše přepínatelná. Drawer je skrytý by default.
* `<button>` a `aria-expanded`: Viditelnost draweru (viz výše) je ovlivněna přepínacím tlačítkem. Stav `aria-expanded` je nastaven buď `false` (drawer je zavřený; defaultní chování při loadu stránky) nebo `true` (drawer otevřen)
* `SVG`: SVG je založeno na [Ikonografii `icon-down`](/ikonografie.md). Obsahuje `focusable="false"` pro odstranění pořadí focusu, a `aria-hidden="true"` pro skrytí před prohlížeči/asistivními technologiemi.

### Vyhněte se overridingu ARIA rolí
`<button>` je raději vložen dovnitř 'handlu', než aby se sám _stal_ handlem. Některé implementace přemeňují handle prvek do tlačítka pomocí `role="button"`. To by však potlačilo implicitní roli nadpisu (pouze prvky s rolí nadpisu jsou brány jako nadpisy pro odečítačky obrazovky). Viz Druhé pravidlo použití ARIA: _Neměňte nativní sémantiku, pokud opravdu nemusíte_[^2].

Vnořením tlačítka do nadpisu má uživatel prospěch ze sémantiky a chování obou prvků.

### Stručná forma obsahu

Stručnější obsah, jako jsou otázky s jednou nebo dvouvětnou odpovědí, je lepší napsat jako seznam. `<ul>` by nesla třídu `accordion`. 

```html
<ul class="accordion">
  <li>
    <p><strong>Otázka 1<strong></p>
    <p>Odpověď 1</p>
  </li>
  <li>
    <p><strong>Otázka 2<strong></p>
    <p>Odpověď 2</p>
  </li>
  <li>
    <p><strong>Otázka 3<strong></p>
    <p>Odpověď 3</p>
  </li>
</ul>
```

Ať už jsou vhodné sémantiky sekcí, nadpisů nebo seznamů, existují určitá strukturální pravidla pro progresivní zlepšování:

* Položky **Akordéonu** musí být obaleny v běžném prvku `accordion`
* Každá položka musí mít alespoň dva prvky
* První prvek nesmí být `<button>` (jelikož jeho vlastní obsah bude obalený v `<button>`)

```js
if (section.handle.nodeName === 'BUTTON') {
  console.error('První potomek každého akordeónu nesmí být <button>');
  return;
}
```

## Doporučený layout

Ať už šipka dolů, nebo symbol plusu, `icon-down` musí mít stav (`aria-expanded="false"`) při zavření, šipka nahoru/mínus při stavu (`aria-expanded="true"`) otevření. V zájmu stručnosti kódu toho dosáhněte pomocí jednoduché transformace CSS:

```css
.accordion__handle [aria-expanded="true"] svg {
  transform: rotate(180deg);
}
```

Text se objeví vlevo od tlačítka a SVG napravo (díky `justify-content: space-between`). Nějaký margin je přidán vlevo od SVG, aby se oddělil od textu tlačítka. Deklarace `flex-shrink: 0` zabraňuje tomu, aby se SVG nechtěně zmenšil, když se zmenší celkový prostor.

```css
.accordion__handle button {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.accordion__handle svg {
  margin-left: 0.5rem;
  flex-shrink: 0;
}
```

### Vysoký kontrast

Testujte komponentu v [Módu vysokého kontrastu ve Windows](https://support.microsoft.com/en-gb/help/13862/windows-use-high-contrast-mode).

## Doporučené chování

Akordéon navržený pomocí progresivního zlepšování nabízí pre-renderovaný strukturovaný obsah, rozbalený a dostupný by default ve chvíli, kdy není JavaScript dostupný.

Výhodou akordéonové interakce je její jednoduchost. Když někdo stránku prochází, namísto toho, aby narazil na spoustu obsahu, přes který musí procházet, narazí na tlačítka, která obsah odhalí. Pokud název tlačítka vzbudí zájem, mohou stisknout handle/tlačítko, aby se obsah odkryl (skrytý v „draweru“ pod tímto tlačítkem).

Každé tlačítko jednoduše přepíná zobrazení (a dostupnost pro asistenční technologie) jeho přidruženého obsahu („drawer“). Stav přepínacího tlačítka je sdělován odečítačkám obrazovky pomocí `aria-expand`[^4]. Příklad:

```js
// Rozbaleno metoda
self.constructor.prototype.open = function (section) {
  section.button.setAttribute('aria-expanded', 'true');
  section.drawer.hidden = false;
}

// Zabaleno metoda
self.constructor.prototype.close = function (section) {
  section.button.setAttribute('aria-expanded', 'false');
  section.drawer.hidden = true;
}
```

Některé implementace obahují `aria-controls` jako referenci pro daný přidružený (drawer) prvek. Díky přibližnému pořadí v kódu to není nutné. Dejte pozor, že `aria-controls` je podporována pouze v programu JAWS (v současné době)[^5].

Referenční implementace obsahuje dvě další metody: `openAll` a `closeAll`. To vám umožňují implementovat tlačítka pro otevírání nebo zavírání každé položky akordéonu současně.

```js
// Otevírání všech drawerů najednou
var openAllButton = document.getElementById('openAll');
openAllButton.addEventListener('click', function () {
  accordion.openAll();
});

// Zavírání všech drawerů najednou
var closeAllButton = document.getElementById('closeAll');
closeAllButton.addEventListener('click', function () {
  accordion.closeAll();
});
```

Funkcionalita `closeAll` může být zejména užitečná pro lidi, kteří otevřeli několik položek jednu po druhé, a následně je pro ně snadné ztratit se v rámci rozbaleného obsahu.

### Automatické zabalování položek
Nedoporučujeme, aby rozbalení jedné položky akordéonu vedlo ke zabalení další. Pokud uživatel otevřel položku, předpokládá se, že chce, aby zůstala otevřená. Je možné, že chtějí odkazovat mezi dvěma nebo více otevřenými položkami. Vždy se ujistěte, že uživatel má věc pod kontrolu a nedělejte předpoklady o jeho potřebách[^6].

## Související výzkum

Toto téma nemá související uživatelský výzkum.

### Pokračovat ve čtení, jinde na webu

[^1]: Tabs — ARIA Authoring Practices, <https://www.w3.org/TR/wai-aria-practices-1.1/#tabpanel>
[^2]: The Second Rule Of ARIA Use — W3C, <https://www.w3.org/TR/using-aria/#second>
[^3]: `@supports` — MDN, <https://developer.mozilla.org/en-US/docs/Web/CSS/@supports>
[^4]: `aria-expanded` (state) — W3C, <https://www.w3.org/TR/wai-aria-1.0/states_and_properties#aria-expanded>
[^5]: `aria-controls` Is Poop — heydonworks.com, <http://www.heydonworks.com/article/aria-controls-is-poop>
[^6]: Give Control — Inclusive Design Principles,  <https://inclusivedesignprinciples.org/#give-control>