---
category: Rozhodnutí pro základní prvky
---

# Ikonografie

## Úvod

Text je primární forma komunikace na webu a je vysoce interoperabilní. Ikonky používejte pouze tam, kde je požadována další srozumitelnost. Ikonky mohou pomoci těm, kteří mají kognitivní poruchy, a ostatním, kteří se ocitnou na stránce, která není v jejich mateřském jazyce.

### Vyhněte se icon fontům

Ikonky implementujte za použití inline SVG. Stejně jako icon fonty, SVG ikonky jsou nekonečně škálovatelné bez degradace.

Narozdíl od SVG, ikonky icon fontů jsou namapovány na unicode body (unicode points) a interpretovány jako text. To může způsobovat problémy z přístupností. Aby nedošlo k přepsání zavedených, smysluplných znaků a symbolů, většina sad icon fontů mapuje své ikonky do, tak zvaných, unicode [Private Use Areas](https://en.wikipedia.org/wiki/Private_Use_Areas). Problém nastává, když si uživatelé nastaví vlastní fonty: ikonky jsou nahrazeny symboly 'chybějícího znaku'[^1]. Dyslektici si občas nastavují své vlastní fonty pro zlepšení čitelnosti.

Ikonky icon fontu namapované na zavedené znaky jsou stejně tak problematické. Ikonka zavření namapovaná na 'A' by se projevila jako 'A', ve chvíli kdy je font přepsán. Zároveň odečítačky obrazovek by ikonky interpretovaly a ohlásily jako 'A'.

## Doporučený markup

Ve všech případech by měl ikonky doprovázet viditelný nebo vizuálně skrytý textový popis. Samotná ikona musí mít atribut `focusable="false"`, aby jej odstranila z focus pořadí (výchozí nastavení v některých verzích Internet Explorer a Edge[^2]). Zároveň by měla mít `aria-hidden="true"`. Protože textový popis (v uvedeném příkladu _„Smazat“_) je dostačující, není identifikace grafiky SVG nezbytná. Neposkytujte alternativní text/popisy pro ikony, kde je text již k dispozici.

```html
<button>  
  <svg class="icon icon--text" focusable="false" aria-hidden="true">
    <use xlink:href="{{site.basedir}}static/images/icons-all.svg#icon-download"></use>
  </svg>
  <span>Smazat</span>
</button>
```

Pouze tam, kde jsou ikony už velmi dobře zavedeny - například tlačítka pro přehrávání a zastavení přehrávače médií - je spolehlivé je používat bez dodatečného vizuálního textu. V těchto případech se doporučuje vizuálně skrytý `<span>`, protože `aria-label` naráží na problémy s překladem textu do jiného jazyka[^3].

V těchto případech, skryjte `<span>` labelu pomocí třídy `visually-hidden` nebo podobně. Tato třída vizuálně zakrývá text, aniž by umlčovala jeho ohlášování v odečítačkách obrazovky.

```css
.visually-hidden {
  border: 0;
  clip: rect(0 0 0 0);
  height: 1px;
  margin: -1px;
  overflow: hidden;
  padding: 0;
  position: absolute;
  white-space: nowrap;
  width: 1px;
}
```

### Tooltipy

Pokud má ikonka [tooltip](https://en.wikipedia.org/wiki/Tooltip), může se tooltip chovat jako primární label.

V následující příkladě, prvek `class="tooltip"` poskytuje název (label).

```html
<a href="link/to/download" download>
  <svg class="icon icon--text" focusable="false" aria-hidden="true">
    <use xlink:href="{{site.basedir}}static/images/icons-all.svg#icon-download"></use>
  </svg>
  <span class="tooltip">Stáhnout</span>
</a>
```

By default, tooltip by byl schován za použití `display: none`. Až na eventy `:hover` a `:focus` by byl zobrazen.

```css
[download] .tooltip {
  display: none;
}

[download]:hover .tooltip,
[download]:focus .tooltip {
  display: block;
}
```

#### Title atribut
**Důležité:** Pro tooltipy [nespoléhejte na atribut `title`](https://developer.paciellogroup.com/blog/2012/01/html5-accessibility-chops-title-attribute-use-and-abuse/). Nezobrazují se při focusu, a tudíž nejsou přístupné pro uživatele klávesnic. Zároveň nejsou typicky ohlášeny odečítačkám obrazovek bez toho, aniž by se musel nastavit mluvený projev v daném softwaru odečítačky.

## Doporučený layout

### Dimenzování a zarovnání

Velikost a zarovnání inline textu zajišťuje třída `icon--text`:

```css
.icon--text {
  height: 1em;
  width: 1em;
  vertical-align: text-top;
}
```

Všimněte si, že jednotka `em` umožňuje ikoně škálování úměrné k `font-size` nadřazeného (parent) prvku. Použití `rem` nebo `px` by tento vztah narušilo a text by se změnil/škáloval bez ikony.

### Barva

Obvyklá pravidla WCAG[^4] pro barevný kontrast platí i pro ikony, i když dopad na ikonky s nedostatečným kontrastem bude pravděpodobně méně kritický než na text. Stejně jako u textu platí, že čím větší je ikona, tím menší je dopad nízkého kontrastu. Neexistuje však žádný dobrý důvod, proč nebrat v potaz dostatečný kontrast.

### Mód Vysokého kontrastu

V Módu Vysokého kontrastu ve Windows, je zesílena barva textu, takže je buď velmi tmavá, nebo velmi světlá. Použijte `currentColor`, abyste SVG dostali na stejnou úroveň kontrastu jako text. V příkladě níže je toto aplikováno ve tříde `icon`:

```css
.icon {
  fill: currentColor;
}
```

Všimněte si, že SVG bere `fill`, ne `color` nebo `background-color`.

### Nezávislost na barvě

Ikonky se občas mění na základě stavu. Je důležité, aby tyto stavy nebyly komunikovány pouze pomocí barvy samotné, jak je uvedeno v **WCAG 2.1 1.4.1: Use of Color**[^5]. Ikonky by jinak nebyly viditelné pro některé barvoslepé uživatele, a pro jiné používající nebarevné displeje.

### Text decoration

CTA (Call To Action) odkazy popsané v patternu [**Tlačítka a CTA**](/tlacitka.md) využívají `text-decoration` styl při eventu `:hover` a `:focus`. Aby linka `text-decoration` nezasahovala do ikonky, je použito `text-decoration-skip`:

```css
.cta:hover,
.cta:focus {
  border-color: $color--blue;
  text-decoration: underline;
  text-decoration-skip: objects;
}
```

## Doporučené chování

SVG ikonky jsou typicky pospolu s interaktivními prvky, které mají své vlastní chování. Specificky pro přepínače (toggle buttons), ikonka by měla pomoci komunikovat stav (zapnuto nebo vypnuto; otevřeno nebo zavřeno; atd). Toho lze dosáhnout buď výměnou ikony, nebo ji nějakým způsobem rozšířit.

## Související výzkum

Toto téma nemá související uživatelský výzkum.

### Pokračovat ve čtení, jinde na webu

[^1]: How Users Kill Your Icon Fonts — Tim Teeling, <https://timteeling.com/how-users-break-your-icon-fonts/>
[^2]: Support tabindex in SVG, don't make every `<svg>` focusable by default (issue), <https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/8090208/>
[^3]: `aria-label` Is A Xenophobe — heydonworks.com, <http://www.heydonworks.com/article/aria-label-is-a-xenophobe>
[^4]: WCAG2.1 1.4.3 Contrast (Minimum), <https://www.w3.org/TR/WCAG21/#contrast-minimum>
[^5]: WCAG2.1 1.4.1 Use Of Color, <https://www.w3.org/TR/WCAG21/#use-of-color>