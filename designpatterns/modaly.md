---
category: Rozhodnutí pro komponenty
---

# Modaly

## Úvod

Ministerstvo spravedlnosti definuje dva druhy upozorňujících zpráv:

1. **Čistě informativní:** značící, že něco se již stalo (a nevyžaduje žádný vstup ze strany uživatele).
2. **Vyžadující akci:** žádá uživatele, aby vybral postup, ve chvíli, kdy nastalo něco kritického/vyžadující rozhodnutí.

**Modaly** tvoří druhý druh. Jedná se o modální okno[^1], které omezuje interakci na sebe, dokud není uživatelem vybrána jedna z možností, které modální okno nabízí. **Modaly** používejte střídmě a pouze tam, kde je okamžitý vstup od uživatele rozhodující pro pokračování v jejich relaci (sessioně).

## Doporučený markup

V následujícím příkladu si představte, že se uživatel pokusil přidat položku do "Oblíbené položky". Vzhledem k tomu, že tato funkce je k dispozici pouze pro přihlášené uživatele, vyžaduje modální okno po uživateli, aby se přihlásil nebo zaregistroval k tomu, aby mohl pokračovat.

```html
  <div class="action-dialog" role="dialog" aria-labelledby="action-dialog__label-1" aria-describedby="action-dialog__desc-1">
    <h3 id="action-dialog-label__1" class="action-dialog__title">Přidat do oblíbených</h2>
    <div id="action-dialog-desc__1" class="action-dialog__content">
      <p>Musíte být příhlášeni pro přidání položky do oblíbených.</p>
    </div>
    <div class="action-dialog__buttons">
      <a href="#/path/to/sign-in">Přihlásit se</a>
      or 
      <a href="#/path/to/register">Registrovat</a>
    </div>
    <button class="action-dialog__close">
      <span class="visually-hidden">zavřít</span>
      <svg class="icon icon--text" focusable="false" aria-hidden="true">
        <use xlink:href="{{site.basedir}}static/images/icons-all.svg#icon-close"></use>
      </svg>
    </button>
  </div>
</body>
```

### Poznámky

* **`role="dialog"`:** Tato `role` je rozhodující pro to, aby se okno chovalo tak, jak předpokládáme v asistivních technologiích, jako je odečítačka obrazovky. Zároveň je modal ohlášen jako _dialog_, když je otevřen a dostává focus (dovnitř sebe).
* **`aria-labelledby` a `aria-describedby`:** Tyto atributy přidružují text nadpisu/labelu dialogu a obsah k samotnému prvku okna. Spolu s rolí dialog jsou tyto informace oznámeny při otevření okna. Budete muset napsat nebo vygenerovat jedinečné identifikátory pro `id`, které jsou zde vyžadovány.
* **`class="action-dialog__buttons"`:** Jednoduchý, nesémantický wrapper pro prvky akce. Prvky akce musí být označeny/napsány jako `<button>`, pokud podněcují něco na stejné stránce (například změna nastavení nebo stavu) nebo odkazy, pokud uživatele přivedou na novou stránku.
* **`class="action-dialog__close"`:** Poskytuje tlačítko zavření, pokud se uživatel rozhodne _nic nedělat_ (v tomto příkladě se nerozhodl přihlásit). Třída visually hidden `vh`[^2] je uvedena pro poskytování přístupného, přeložitelného textu vedle nepřístupné ikonky (pro odečítačky obrazovek). Tlačítko Zavřít je deprioritizováno ve prospěch jmenovaných akcí a jeví se jako poslední ve zdrojovém a v pořadí focusu.
* **`</body>`:** Okno musí být potomkem `<body>`, aby overlay/inert fungoval správně. Podívejte se na [Doporučené chování](#doporučené-chování)

## Doporučený layout

**Modaly** se mohou zobrazit uprostřed, nebo v dolní části, stránky. V příkladě níže, hodnota `fixed` pozice zajišťuje, že nedojde k odscrollování.

```css
.action-dialog {
  position: fixed;
  top: auto;
  left: 0;
  right: 0;
  bottom: 0;
}
```

Konfigurace okna do středu stránky vyžaduje `transform`. Prvek je tak umístěn do vertikálního středu bez ohledu na jeho přirozenou výšku:

```css
.action-dialog__center {
  top: 50%;
  bottom: auto;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

Na základě toho, okna se delším obsahem by byla v horní a dolní části viewportu skryta. Vyhnete se tomu tak, že popisný prvek okna (`class="action-dialog__desc"`) příjmá `max-height` a je mu dovolen vertikální scroll:

```css
.action-dialog__desc {
  max-height: 30vh;
  overflow-y: auto;
}
```

Ve chvíli, kdy je okno otevřeno, potomci dostávají atribut `inert` (více v [**doporučeném chování**](#doporučené-chování) níže). Inert obsah by měl vypadat inertní, tedy měla by mu být ubrána viditelnost. Pro dosažení efektu použijte opacity a/nebo filtering.

```css
.action-dialog--open [inert] {
  opacity: 0.3;
}
```

Všimněte si, že styl je aplikován za použití 'action-dialog' namespacu. To zajišťuje, že specifický inert styl je použitý pouze ve chvíli otevřeného dialogu (styl se nechtěně nepřenáší na jiné `inert` instance).

### Mód Vysokého kontrastu

Testujte modální okno v Módu Vysokého kontrastu ve Windows.

## Doporučené chování

Výsledek akce provedené prostřednictvím modálního okna bude záviset na účelu vašeho okna a není zde specifikován.

### Když se modální okno otevře

1. Okno se objeví na určené pozici
2. Okolní stránka se stává inertní (neinteraktivní, nedostupná pro asistenční technologie a není zaostřitelná pomocí klávesnice)
3. Focus se přesune na první ovládací prvek (odkaz nebo tlačítko, které není deaktivováno) uvnitř prvku `class="action-dialog__buttons"`

### Když se modální okno zavře

1. Okno je skryté
2. Okolní stránka přestane být inertní
3. Pokud bylo okno původně vyvoláno jako odpověď na akci uživatele, prvek, který akci vyvolal (jako je tlačítko), je znovu zaměřené.

#### Okna přerušení
Jedná se o okna, které se objevují spontánně, bez přímého vyvolání (vědomého či jiného) ze strany uživatele. Jsou velmi rušivé a prakticky za všech okolností se jim vyhněte. Jedním z legitimních případů použití by byla informace pro uživatele, že se blíží konec jeho časové relace (sessiony), a vy mu dáte možnost sessionu prodloužit nebo zavřít.

Notifikace a status messages, které nevyžadují zásah uživatele, by neměly "krást" focus. Místo toho zvažte použití ARIA live region[^3].

### Klávesnice a odečítačka obrazovek

Focus je po otevření umístěn dovnitř, což znamená, že uživatelé klávesnice mají přístup k funkcím a uživatelé odečítačky obrazovky jsou informováni o přítomnosti modalu. Když se modal poprvé otevře, uživatel odečítačky obrazovky uslyší oznámení o roli dialogu (_"dialog"_), přidružený label (název) a popis a roli a label původně zaměřeného ovládacího prvku.

Uživatelé mohou přesouvat focus mezi uvedenými ovládacími prvky (odkaz Přihlásit se/Registrovat) a tlačítkem Zavřít.

## Související výzkum

Toto téma nemá související uživatelský výzkum.

### Pokračovat ve čtení, jinde na webu

[^1]: "Modal window" (Wikipedia), <https://en.wikipedia.org/wiki/Modal_window>
[^2]: Gist of the `vh` (visually hidden) class,  <https://gist.github.com/Heydon/c8d46c0dd18ce96b5833b3b564e9f472> 
[^3]: "Notifications" (Inclusive Components blog), <https://inclusive-components.design/notifications/>