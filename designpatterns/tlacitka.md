---
category: Rozhodnutí pro komponenty
---

# Tlačítka a calls-to-action

## Úvod

Tlačítka a výzvy k akci (calls-to-action) se významně liší ve formě a účelu. Měla by se však řídit některými základními pravidly. Pouze v případě, že jsou tato pravidla dodržena, prožije uživatel příjemnou interakci.

Nejdůležitější z těchto pravidel je to, že prvky `<a>` a `<button>` jsou přiřazeny k příslušným úkonům. To znamená, že odkazy by měly být použity tam, kde je vyvolána navigace, a `<button>`, kde je vyvolána ne-navigační akce. Například, ovládací prvek 'Přihlásit se' by vyhovoval prvku `<button>`, protože jeho úlohou je odeslat formulář pro přihlášení. Sousední výzva k akci „Registrovat“ &ndash; uživatel by byl přesunut na jiný formulář, na jiné stránce nebo obrazovce &ndash; by prospělo sémantice `<a>`.

Toto je užitečné zejména pro uživatele asistenčních technologií, například pro ty, kteří používají software pro odečítání obrazovky. Prvek ohlášený jako odkaz, který se chová jako tlačítko vytváří matoucí prožitek. Vizuální design tlačítek a výzev k akci by také měl naznačovat jejich chování. Pokud se změní stav tlačítka, měla by být tato změna také zobrazena vizuálně.

## Doporučený markup

### Základní call-to-action odkaz

Ve většině případů by měla být výzva k akci označena jako odkaz, protože chováním je obvykle přenést uživatele na novou stránku nebo sekci[^1]. Měl by být použit standardní markup odkazu, včetně `href` (odkazy vynechávající atributy `href` nejsou klávesnicí zaměřitelné).

Třída je přidána k prvku jako styling hook, který umožňuje vizuálně prvek odlišit od obecných odkazů.

```html
<a class="cta" href="path/to/location">Vstoupit na portál</a>
```

### Základní tlačítko

Je bezpodmínečně nutné, aby ovládací prvky, které nevyvolávají chování podobné odkazům, byly označeny jako prvky `<button>`. Všechna tlačítka, která nejsou určena pro odesílání formulářů, by měla mít `type="button"`[^2]. Toto je zvlášť důležité _uvnitř_ `<form>`, jelikož některé user agents považují jakékoliv bez explicitního typu nalezené `<button>` jako tlačítko pro odeslání formuláře.

```html
<button class="button" type="button">Vypočítat daň</button>
```

Obecně řečeno: pokud je ovládací prvek, na který lze kliknout, určen pro spuštění JavaScriptu, měl by to být `<button>` s `type="button"`.

### Labely

Většina tlačítek a výzev k akci by měla být jednoduše označena pomocí jejich textového uzlu. Textové uzly jsou interoperabilní, protože je lze vidět, slyšet prostřednictvím syntetických hlasů odečítačky obrazovky a používají se jako aktivační prvky pro software umožňující aktivaci hlasem.

Ve většině případů ovládací prvky, které se spoléhají pouze na ikony, nedoporučují, a to ani v případech, kdy jsou pro asistivní technologie poskytovány neviditelné labely. Tlačítka pouze jako ikonka jsou zvláště těžko aktivovatelná hlasem, protože (skrytý) textový label musí uživatel uhodnout.

Tam, kde je použito tlačítko pouze jako ikonka (například tlačítka přehrát/zastavit), poskytněte text skrz vizuálně skrytý `<span>`, ne `aria-label`. Bohužel, `aria-label` není běžnými službami pro překlad textu zachytávána[^3].

```html
<button class="button" type="button">
  <!-- ikonka zde -->
  <span class="visually-hidden">Přehrát</span>
</button>
```

Vyhněte se použití atributů `title`. Ty jsou ve většině prohlížečů viditelné pouze při hoveru, ne focusu. Navíc jsou ve výchozím nastavení odečítaček obrazovky potlačeny by default.

### Ikonky

Ikony by neměly obsahovat text ani být jinak přístupné asistivním technologiím, a neměly by být zaměřitelné. Proto by měly dostat `aria-hidden="true"` a `focusable="false"`.

```html
<a class="cta" href="path/to/help-page">
  <span class="button__label">Pomoc</span>
  <svg aria-hidden="true" focusable="false">
    <use xlink:href="#help"></use>
  </svg>
</a>
```

### Stavy

#### Disabled stav

Standardní `<button>` prvky lze zakázat pomocí vlastnosti/atributu `disabled`. Dejte pozor, že přidání `disabled` způsobí, že `<button>` nebude zaměřitelný. To může způsobit zmatek některým uživatelům odečítačky obrazovky a uživatelům, kteří stránku projíždějí pomocí <kbd>Tab</kbd> (například při vysoké úrovni přiblížení stránky). Disabled tlačítka můžete nastavit tak, aby byla znovu zaměřitelná přidáním `tabindex="0"`.

```html
<button class="button" type="button" disabled tabindex="0">Stáhnout</button>
```

#### Vyhněte se disabled submit tlačítkům
Existuje několik problémů s použitelností a přístupností deaktivovaných tlačítek při odeslání formuláře, dokud uživatel nezadá platná/očekávaná data[^4]. Je často lepší nechat uživatele, aby se pokusil o odeslání a na základě toho poskytnout zpětnou vazbu.

Odkazy nemají stav `disabled`. Můžete však vyvolat oznámení, že odkaz je 'deaktivován' použitím atributu `aria-disabled="true"`.

```html
<a class="cta" href="path/to/forbidden-place" aria-disabled="true">Zakázané místo</a>
```

Buď použijte `event.preventDefault()` nebo odstraňte `href` pro funkcionální deaktivování odkazu. Jak bylo výše zmíněno, odstraněním `href` je odkaz nezaměřitelný. Stejně jako s `<button>`, můžete focus znovu nastavit za použití `tabindex="0"`. 

#### Přepínání stavů

WAI-ARIA poskytuje sadu stavových atributů, mnoho použitelných pro tlačítka. Dva z nejběžnějších jsou `aria-press` a `aria-expand`, které definují dva druhy přepínacího stavu. První označuje druh přepínače zapnuto/vypnuto a druhý označuje, zda je přidružený/ovládaný prvek rozbalen/expanded (viditelný) nebo zabalen/collapsed (skrytý).

Přítomnost těchto atributů stavu mění ohlašovanou roli `<button>` z _"button"_ do _"push button"_ nebo _"toggle button"_ (v závislosti na stavu a použitých asistivních technologiích). Je proto důležité, aby byl atribut vždy přítomen a aby měl jednoznačnou hodnotu `true` nebo `false`.

```html
<!-- collapsed -->
<button class="button button-switch" type="button" aria-expanded="false">Více informací</button>

<!-- expanded -->
<button class="button button-switch" type="button" aria-expanded="true">Více informací</button>
```

### Použijte nativní checked stavy
Nedoporučujeme používat `<button>` prvky s `aria-checked`. Místo toho použijte nativní zaškrtávací políčko (checkbox) a prvky přepínače (radio). Základní chování těchto prvků se nespoléhá na JavaScript a zajišťuje robustnější a přístupnější komponenty.

Tlačítka mohou po stisknutí svůj focus přesunout. Příklady zahrnují tlačítka, která otevírají modaly nebo vyvolávají nabídky/menu. Za těchto okolností použijte vlastnost `aria-haspopup="true"`[^5]. Ta varuje uživatele odečítačky obrazovky, že se jejich kontext změní tím, že bude tlačítko oznámeno jako _"popup button"_ nebo podobně.

Pokud tlačítko jednoduše přesune focus do nové sekce na stránce nebo zcela na novou stránku, nemělo by to být tlačítko, ale odkaz. Místo toho použijte obecný odkaz nebo call-to-action.

## Doporučený layout

Řiďte se následujícími principy:

1. **Buďte konzistentní:** Všechny vaše tlačítka by měla sdílet stejný základní design, stejně jako všechny vaše call-to-action. Buďte v souladu s barvou, velikostí a tučností písma a odsazením.
2. **Odlišujte tlačítka a odkazy:** Doporučujeme, aby vaše calls-to-action byla vizuálně odlišná od vašich `<button>`: forma následuje funkci. Běžný přístup je, že dáte všem `<button>` barvu pozadí, ale calls-to-action dáte transparentní background ale s rámečkem (border) okolo.
3. **Poskytněte vizuální vodítka:** Ujistěte se, že vaše tlačítka a calls-to-action zvou uživatele, aby na ně klikl. Použijte barvu odkazu stanovenou na webu, pokud není zděděna; použijte jasné styly pro hover a focus (viz níže); použijte imperativní jazyk.

### Kontrast
Kontrast mezi popředím (textem) a barvami pozadí musí splňovat minimální požadavky na kontrast (úroveň WCAG AA[^6])

### Hover a focus styly

Styly hoveru a focusu by měly být jasné, aby je bylo možné odlišit od jejich výchozího stavu.

Protože tlačítka a výzvy k akci mají tendenci mít obdélníkové ohraničení/border. Tenký a slabý focus outline, který nabízí některé user agents, může být velmi nejasný. Pokud je použit styl outline, udělejte jej plný a za použití offsetu jej oddělte od okrajů ovládacího prvku.

```css
.button:focus, .cta:focus {
  outline: 2px solid; /* přebírá barvu fontu */
  outline-offset: 2px;
}
```

Doporučejem, aby call-to-action odkazy měly `text-decoration: underline` pro `:hover` a `:focus`. Tímto způsobem je pak jasnější, že je prvek odkazem. Pro odstranění podtržení u ikonek, použijte `text-decoration-skip: objects`.

### Indikování stavu

Přepínací tlačítka, která používají `aria-pressed` nebo `aria-expanded` (viz [Přepínání stavů](#přepínaní-stavů)) musí jasně indikovat, v jakém jsou současném stavu. Zapnuto/vypnuto (`aria-pressed`) toho může dosáhnout tím, že explicitně uvedete text 'zapnuto' nebo 'vypnuto'. Tento text by měl pak být skryt odečítačkám obrazovek pomocí `aria-hidden="true"`. Stav už je totiž předem ohlášen skrz `aria-pressed`. 

```html
<!-- Tmavý režim neaktivní -->
<button class="button button-switch" type="button" aria-pressed="false">
  Tmavý režim
  <span aria-hidden="true">vypnuto</span>
</button>

<!-- Tmavý režim aktivní -->
<button class="button button-switch" type="button" aria-pressed="true">
  Tmavý režim
  <span aria-hidden="true">zapnuto</span>
</button>
```

### Mód Vysokého kontrastu

Mód Vysokého kontrastu ve Windows má tendenci odstraňovat pozadí z tlačítek, takže ty se pak jeví jako samotný text. Chcete-li obnovit jejich obdělníkový tvar, můžeme použít průhledný rámeček[^7]. Tento rámeček bude viditelný pouze, když je spuštěn Windows HCM.

```css
.button, .cta  {
  border: 2px solid transparent; /* pro mód vysokého kontrastu */
}
```

## Doporučené chování

Tam, kde jsou odkazy `<a>` a `<button>` použity správně pro výzvy k akci a tlačítka, vyvolají prohlížeče standardní a očekávané chování. Jedná se o následující:

### Odkazy

* Prvek je identifikován jako odkaz skrz asistenční technologie
* Prvek lze zaměřit pomocí klávesnice
* Prvek lze aktivovat kliknutím myši, dotykem a klávesou <kbd>Enter</kbd>
* Kontextové menu specifické pro odkaz lze vyvolat pomocí pravého tlačítka myši nebo <kbd>Shift</kbd> + <kbd>F10</kbd> (Windows)
* Prvek lze přetáhnout myší na panel záložek prohlížeče

### Tlačítka

* Prvek je identifikován jako tlačítko skrz asistenční technologie
* Prvek lze zaměřit pomocí klávesnice
* Prvek lze aktivovat kliknutím myši, dotykem a klávesami <kbd>Enter</kbd> a <kbd>Space</kbd>
* Atribut `type ="submit"` umožňuje tlačítku odeslat nadřazený `<form>`
* Při atributu `disabled` (Boolean) není tlačítko zaměřeno (by default; viz [disabled stav](#disabled-stav)) a identifikuje tlačítko jako disabled skrz asistenční technologie

## Související výzkum

Toto téma nemá související uživatelský výzkum.

### Pokračovat ve čtení, jinde na webu

[^1]: Anchors, buttons, and accessibility — Formidable Labs, <https://formidable.com/blog/2014/05/08/anchors-buttons-and-accessibility/>
[^2]: The `button` element — W3C, <https://www.w3.org/TR/2011/WD-html5-20110525/the-button-element.html#the-button-element>
[^3]: ARIA Label Is A Xenophobe — heydonworks.com, <http://www.heydonworks.com/article/aria-label-is-a-xenophobe>
[^4]: Disabled Buttons Suck — Access Lab, <https://axesslab.com/disabled-buttons-suck/>
[^5]: `aria-haspopup` property — W3C, <https://www.w3.org/TR/wai-aria-1.1/#aria-haspopup>
[^6]: WCAG2.1 1.4.3, Contrast (Minimum), <https://www.w3.org/TR/WCAG21/#contrast-minimum>
[^7]: Transparent border for high contrast mode (test case) — Joe Watkins, <https://codepen.io/joe-watkins/pen/mApBvo>