---
category: Rozhodnutí pro komponenty
---

# Pole formuláře a validace

## Úvod

Elegantní zacházení s uživatelskými vstupy je rozhodující pro použitelnost služeb Ministerstva spravedlnosti. Účelem tohoto dokumentu je stanovit robustní přístupy k prezentaci a validaci polí formuláře.

## Doporučený markup

### Labeling

Jakékoli prvek vstupního pole musí být programátorsky spojen s labelem. Toho je dosaženo tím, že atribut <code>for</code> labelu a atribut <code>id</code> vstupního pole sdílí tu samou hodnotu.

```html
<label for="username">Uživatelské jméno</label>
<input type="text" id="username" name="username" />
```

#### Informace k `name` a `id`
Běžná mylná představa je to, že atribut `name` souvisí se vstupním polem. I přesto, že dostává stejnou hodnotu jako `id`, pouze `id` přidružuje label k vstupu.

#### Skupinové labely

Někdy by mělo být více prvků formuláře seskupeno pod společným labelem. Standardní metoda pro vytvoření takové skupiny je prvky `<fieldset>` a `<legend>`. `<legend>` musí být první potomek uvnitř `<fieldset>`u.

```html
<fieldset>
  <legend>Label pro skupinu</legend>
  <!-- jednotlivé labely prvků -->
</fieldset>
```

Toto je nejvíce důležité ve chvíli, kdy máte radio buttony: skupina přepínačů, sdílející stejný `name` atribut, představují _jediné_ pole formuláře a `<legend>` toto pole označuje.

```html
<fieldset>
  <legend>Vaše oblíbené zvíře</legend>
  <label>
    <input type="radio" name="favourite-pet">
    Kočka
  </label>
  <label>
    <input type="radio" name="favourite-pet">
    Pes
  </label>
  <label>
    <input type="radio" name="favourite-pet">
    Morče
  </label>
</fieldset>
```

#### Obalování labely
V příkladě výše si všimněte _obalení_ inputů labely. Přidružení `for` a `id` můžete vypustit pouze v tomto případě obalení `<label>`y. Tohle je běžná praxe pro přepínače (radios) a zaškrtávací pole (checkboxes).

Je zcela legitimní umístit nadpisy dovnitř `<legend>`y. Ve skutečnosti to pomáhá dát formuláři sémantickou strukturu (zlepšení navigace uživateli odečítačky obrazovky), aniž byste museli vytvářet samostatné a nadbytečné labely.

```html
<fieldset>
  <legend><h2>Label pro skupinu</h2></legend>
  <!-- jednotlivé labely prvků -->
</fieldset>
```

### Popisy, ne placeholdery

`placeholder` (zástupný text) atribut je často zneužíván k nahrazení primárního labelu. Nejenže je `placeholder` méně spolehlivý pro přístupnost, ale představuje také řadu problémů týkajícího se tlaku na poznání (cognitive load), překladu a interoperability[^1].

Pokud je používáte, placeholdery by měly poskytovat pouze doplňující informace, například nápovědu ohledně očekávaného formátu vstupu. Pro tento účel důrazně doporučujeme použít popisy místo `placeholder`ů. Popisy jsou připojeny k `<label>`, a proto jsou zahrnuty do výpočtu zpřístupněného názvu. Na rozdíl od `placeholder`ů, přetrvávají během zadávání a jejich textový obsah se může zalomit, takže nehrozí, že budou zakryty.

V následující příkladu, prvek `<small>` je použit pro vizuální popis. Bude zobrazen menší text, by default. Můžete ho vložit na další řádek za použití `display: block`. Všimněte si, že prvky`<label>` jsou inline, to není tedy v souladu s tím, aby do nich byly vloženy blokové prvky.

```html
<label for="email">
  E-mailová adresa
  <small>E-mail bude odeslán na</small>
</label>
<input type="text" id="email" name="email" />
```
Ne všechny pole formuláře potřebují popisy.

### Input typy

Tam, kde jsou dobře podporovány speciální typy vstupu HTML5, doporučujeme, aby byly použity místo generických (a defaultních) `type="text"`. Například `number` `type` užitečně omezuje vstup na číslice, umožňuje inkrementaci – obvykle pomocí tlačítek nahoru a dolů – a vyvolává zobrazení numerické virtuální klávesnice.

#### Vyhněte se custom prvkům formuláře
Nejúčinnějším a nejrobustnějším způsobem implementace přístupných polí formuláře je použití nativních elementů `<input>`, `<textarea>` a `<select>`. Tyto prvky mají předdefinované a očekávané chování a automaticky sdělují své role, hodnoty a stavy asistivním technologiím.

Je možné emulovat chování nativního elementu formuláře s atributem WAI-ARIA a JavaScriptem, ale to je zřídka dobrý nápad. Implementace směřují ke složitosti a s větší pravděpodobností se rozbijí, protože závisí na JavaScriptu, aby fungovaly.

```html
<!-- nativní -->
<input type="checkbox">

<!-- WAI-ARIA (vyžaduje JavaScript pro switch zaškrtnutého stavu) -->
<div role="checkbox" aria-checked="false" tabindex="0"></div>
```

### Chybové stavy

Pokud má pole neplatnou hodnotu, jeho neplatnost a související chybová zpráva musí být sdělena jasně a přístupně. Protože ověřování nativních formulářů HTML5 je implementováno nekonzistentně[^2], doporučuje se do každého pole nebo nadřazeného formuláře umístit `novalidate`, což upřednostňuje vlastní proces ověřování.

Proces validace je popsán v [doporučeném chování](#doporučené-chování).

#### `aria-invalid`

Prvek pole v nevalidním stavu by měl mít `aria-invalid="true"`, a `aria-invalid="false"` ve chvíli, kdy je nevalidnost opravena.

#### Chybová zpráva

Chybové zprávy by měly být stručné ale popisné. Svému prvku pole jsou přiřazeny zpřístupněným popisem skrz `aria-describedby`. Tento prvek je naplněn příslušnou chybovou zprávou, když se pole stane nevalidním, a vyprázdní se, když je opraveno. Pro to, aby byla změna stavu ohlášena uživatelům, obsahuje tento prvek `role=alert`.

```html
<!-- neurčitý (počáteční) stav -->
<label for="username">
  E-mailová adresa
  <small>Použito při registraci</small>
</label>
<input type="text" id="username" name="username" aria-describedby="username-error" />
<div id="username-error"></div>

<!-- invalidní stav-->
<label for="username">
  E-mailová adresa
  <small>Použito při registraci</small>
</label>
<input type="text" id="username" name="username" aria-describedby="username-error" aria-invalid="true" />
<div id="username-error">Chyba: Vaše e-mailová adresa nemůže obsahovat mezery</div>

<!-- validní stav-->
<label for="username">
  E-mailová adresa
  <small>Použito při registraci</small>
</label>
<input type="text" id="username" name="username" aria-describedby="username-error" aria-invalid="false" />
<div id="username-error"></div>
```

#### Povinné pole

Předpokládá se, že všechna pole předložená uživateli by měla být vyplněna, jinak by neměla být přítomna. V takovém případě se považuje za zbytečné označování polí jako povinných před nebo během validace jednotlivých polí.

Pouze v případě pokusu o odeslání by mělo být vloženo atribut `aria-required="true"` a chybová zpráva. Toto přiřazení je upřednostňováno před HTML5 `required` Boolean stejně jako `aria-invalid` je preferována před `invalid`.

```html
<label for="username">
  Username
  <small>Použito při registrace</small>
</label>
<input type="text" id="username" name="username" aria-required="true" aria-invalid="true" aria-describedby="username-error" />
<div id="username-error">Chyba: Toto pole je povinné</div>
```

Místo určení povinných polí uveďte nepovinná. Těch by vcelku mělo být méně. Textu `<label>` dejte příponu '(nepovinné)'.

```html
<label for="fact">(nepovinné) Zajímavý fakt</label>
<input type="text" id="fact" name="fact" aria-describedby="username-error" />
```

## Doporučený layout

### Pořadí a orientace

Umístěte labely (a jejich popisy, jsou-li k dispozici) _nad_ prvky formuláře. To je obzvláště důležité na mobilních platformách, protože vyvolaná virtuální klávesnice má zvyk zakrývat labely na straně nebo pod vstupy.

Kvůli tlaku na kognici se vyhněte animovaným labelům, které se objevují jako `placeholder`y a poté se animují směrem nahoru, aby zaujaly pozici labelu[^3], tak zvané, floating labels. Label _uvnitř_ vstupu, podobný placeholderu, může působit jako pole s již vyplněnou hodnotou.

#### 'Obecná' chybová zpráva

Když se uživatel pokusí odeslat formulář obsahující chyby, objeví se obecná chybová zpráva (a uživatele odečítačky obrazovky upozorní jako ARIA live region). Tato chybová zpráva by se měla objevit přímo nad tlačítkem Odeslat, aby byla viditelná pro uživatele bez potřeby pro scrollování.

Obecná chybová zpráva se objeví, když odeslání selže a live region (živá oblast) je naplněn chybovou zprávou.

```html
<!-- Počáteční stav -->
<div role="alert"></div>
<button type="submit">Odeslat</button>

<!-- Odeslání selhalo -->
<div role="alert">Chyby: Opravte následující chyby</div>
<button type="submit">Odeslat</button>
```

### Neviditelné labely

Důrazně doporučujeme, aby pole formuláře měla viditelné a trvalé labely; labely, které nezmizí při zaostření nebo vstupu.

Za určitých zvláštních okolností je neviditelný, avšak přístupný label přijatelný. Například, jeden formulář pro vyhledávání vstupu může mít tlačítko odeslání, které zní „Vyhledat“ - účinně poskytuje označení jak pro vstup, tak pro samotné tlačítko. V tomto případě můžete skrýt `<label>` vizuálně pomocí třídy, která udržuje label dostupný pro asistenční technologie[^4].

```html
<label for="search" class="visually-hidden">Your search term</label>
<input id="search">
<button type="submit">Vyhledat</button>
```

### Mód Vysokého kontrastu

Pro lepší podporu Módu vysokého kontrastu systému Windows je k chybovým zprávám přidáno průhledné ohraničení. Zobrazují se tak jako pole, a, pro podporující prohlížeče, zpráva dostává filtr inverze, aby získala vzhled pozadí:

```css
.form__field-error,
.form__warning {
  border: 1px solid transparent;
}

@media (-ms-high-contrast: active) {
  .form__field-error,
  .form__warning {
    filter: invert(100%);
  }
}
```

### Indikace chyby

Je naprosto nezbytné, aby chyby byly jasně identifikovány. Nespoléhejte se na barvu na označení chybového stavu[^5], protože dojde k selhání na monochromatických displejích a pro ty, kteří nemohou přesně vnímat barvu.

Tam, kde jsou chyby, by vždy měly být chybové zprávy. Předpona chybové zprávy s chybou „Chyba:“ nebo varovným symbolem zajišťuje, že povaha zprávy je zprostředkována výslovně.

```html
<div id="username-error"><strong>Chyba:</strong> Váš e-mail nemůže obsahovat mezery</div>
```

### Vysoký kontrast

Použijte CSS filtr pro překlopení barev chybové zprávy:

```css
@media (-ms-high-contrast: active) {
  .form__field-error,
  .form__warning {
    filter: invert(100%);
  }
}
```

## Doporučené chování

Validace formuláře by mělo sestávat ze dvou fází:

1. Validace jednotlivého pole
2. Odeslání a ověření formuláře

### 1. Initial stav

* Žádná pole nemají atribut `aria-invalid`
* Povinná pole mají `aria-required="true"`
* Obecný live region pro chybu je přítomen DOMu, ale nic zatím neobsahuje

### 2. Individuální validace pole

* `aria-invalid` přepíná mezi `true` a `false` podle toho, jestli je pole validní nebo nevalidní

### 3. Nezdařené odeslání

* Obecný live region pro chybu je vyplněn
* Focus zůstává na tlačítku pro odeslání

### 4. Oprava chyb po nezdařeném odeslání

* Chybové zprávy jsou odstraňovány tak, jak jsou jednotlivá pole opravovány
* Po opravě všech jednotlivých chyb bude odstraněna obecná chybová zpráva

### 5. Úspěšné odeslání

* Custom `submitted` event je vyvolán na formuláři. Může například vyvolávat `alert`.

### Varianty a upozornění

* Některé implementace deaktivují tlačítko Odeslat, dokud není formulář bez chyb. To nedoporučujeme, protože to může být pro některé uživatele matoucí a frustrující[^6]. Je lepší povolit odeslání a pak být explicitní s upozorněním.
* Některé implementace zakazují pole, která byla správně vyplněna, aby bylo jasnější, která pole je třeba ještě vyplnit. Doporučujeme, aby všechna pole zůstala povolena, takže uživatelé mohou kdykoli upravit svůj vstup. V některých případech může korekce hodnoty jednoho vstupu znamenat nutnost úpravy jiného, i když je z hlediska formátu prakticky správná.
* Možná budete chtít použít "pozitivní ověření", přiněmž vstupy, které jsou úspěšně dokončeny, zobrazí zelenou "fajfku". Zde je obtížné rozlišovat mezi správným formátem a správnými informacemi. Například, fajfka vedle správně naformátovaného čísla bankovní karty je zavádějící: uživatel se může domnívat, že víte, že se jedná o číslo pro jeho konkrétní kartu.

## Související výzkum

Toto téma nemá související uživatelský výzkum.

### Pokračovat ve čtení, jinde na webu

[^1]: Do Not Use The Placeholder Attribute — Eric Bailey (Smashing Magazine), <https://www.smashingmagazine.com/2018/06/placeholder-attribute/>
[^2]: Native Form Validation — Peter-Paul Koch, <https://medium.com/samsung-internet-dev/native-form-validation-part-3-8e643e1dd06>
[^3]: Floating Labels Are Problematic — Adam Silver, <https://medium.com/simple-human/floating-labels-are-a-bad-idea-82edb64220f6>
[^4]: Gist of the `vh` (visually hidden) class,  <https://gist.github.com/Heydon/c8d46c0dd18ce96b5833b3b564e9f472> 
[^5]: WCAG2.1 1.4.1 Use Of Color, <https://www.w3.org/TR/WCAG21/#use-of-color>
[^6]: Disabled buttons suck — Axesslab, <https://axesslab.com/disabled-buttons-suck/>