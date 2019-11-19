---
category: Rozhodnutí pro základní prvky
---

# Focus

## Úvod

Při diskusi o přístupnosti webovému obsahu často používáme termín asistivní technologie[^1]: software a zařízení, které pomáhají zdravotně postiženým. Asistivní technologie se však na rozdíl od „adaptivních technologií“ (jako jsou naslouchátka) nezabývají pouze zdravotním postižením. Odečítačka obrazovky není jen pro nevidomé a zrakově postižené uživatele. Může být použita například ke vzdělávání.

Je zavádějící si myslet, že pojem „uživatelé klávesnice“ se vztahuje pouze na osoby se zdravotním postižením. Klávesnici, ať už mechanickou nebo virtuální, používá většina uživatelů v určitém okamžiku během interakce. Všichni lidé používají klávesnice. Někteří na nich závisí více než jiní.

Pro mnoho, včetně těch s dočasnými nebo dlouhodobými onemocněními, není ovládání myši (nebo „ukazatele“) jednoduché. Klávesnice je _asistivní_ v tom, že poskytuje alternativní způsob výběru a aktivace interaktivního obsahu &ndash; ten, který nevyžaduje jemnou motoriku.

WCAG 2.1 obsahuje velké množství, tak zvaných, kritérií úspěchu pro navigování po stránce pomocí klávesnice. **Přístupnost za pomoci klávesnice (Keyboard Accessible)**[^2] má svou vlastní sekci, a je doplněna pravidly **Pořadí focusu (Focus Order)** a **Viditelnost focusu (Focus visible)** pod **Navigací (Navigation)**[^3].

## Doporučený markup

Pouze interaktivní prvky by měly být příjmat focus klávesnice. Pokud prvek nic nedělá ve chvíli, kdy je stisknut, kliknut, nebo zmáčknut, neměl by se objevit v _pořadí focusu_: posloupnost prvků zaměřitelných (focusable) klávesou <kbd>Tab</kbd>.

Standardní HTML nabízí řadu prvků pro interaktvní účely, to jsou odkazy (`<a>`), tlačítka (`<button>`), a prvky formuláře (form controls). Tyto prvky nejen že jsou zaměřitelné by default, ale zároveň má každý z nich implicitní _roli_. Role identifikuje třídu prvku nevizuálně. Například, jako výsledek zaměření prvku je odečítačkou ohlášen odkaz jako _"link/odkaz"_ a tlačítko jako _"button/tlačítko"_. Uživatelé odečítaček obrazovek jsou obvykle uživateli klávesnice, protože nemohou (snadno) vnímat topologii rozhraní, aby vedli myš/ukazatel.

Pro interaktivní prvky doporučujeme použít pouze sémantické HTML. Vytvářet přístupné interaktivní prvky pro nesémantický markup je méně robustní.

### Špatně: Závislost na JavaScriptu

Níže uvedený příklad bude identifikován jako tlačítko, a bude moci přijmat focus za pomoci `tabindex` díky hodnotě `0`[^4]. Přesto, není možné klávesnicí k prvku přistoupit jako k prvku `<button>`, jelikož by musel být přidán event listener (pro stisky kláves <kbd>Enter</kbd> a/nebo <kbd>Mezerník</kbd>) pomocí JavaScriptu.

```html
<div class="button" role="button" tabindex="0">Stiskni mě</div>
```

### Správně: Využití Built in chování

Prvek `<button>` nevyžaduje explicitní ARIA roli, a příjmá focus by default. Click events jsou spuštěné <kbd>Enterem</kbd> a/nebo <kbd>Mezerníkem</kbd> automaticky.

```html
<button class="button">Stiskni mě</button>
```

### Pořadí focusu

Pořadí focusu (Focus order) je pořadí, ve kterém interaktivní prvky příjmají focus. Grafické rozhraní se pro uživatele zrakově postižené používající klávesnici stává matoucím, když je pořadí focusu v rozporu s vizuálním rozvržením stránky. Toto je ještě umocněno použitím zvětšení/lupy: kdy je uživatel přiblížen a navigován pomocí focusu, a rozhraní jej může směrovat neočekávanými směry a na neočekávaná místa.

Jedna z hlavních věci, která často narušuje pořadí focusu je:

#### Kladné hodnoty `tabindex` 

By default, pořadí focusu se řídí pořadím zdroje: pořadí, ve kterém se prvky zobrazují v markupu. Číselné hodnoty `tabindexu` toto pořadí přepisují. Tedy, prvek s `tabindex="1"` bude prvním zaměřitelným prvkem na stránce, nehledě na pořadí ve zdrojovém markupu. Navigace pomocí klávesnice by pravděpodobně znamenala přeskočení nějakého interaktivního obsahu - obsah, na který by se mohl zaměřit _až po_, co byl prvek s `tabindex ="1"` zaměřené. To je zpravidla nevhodné.

Vyhněte se proto kladným hodnotám `tabindexu`.

## Doporučený layout

Podle **WCAG2.1 2.4.7 Focus Visible**: _"jakékoli uživatelské rozhraní ovládatelné klávesnicí má provozní režim, ve kterém je viditelný indikátor zaostření klávesnice"_. Tedy, bez viditelného znázornění není snadné zjistit, který prvek má momentálně focus, pokud vůbec.

[User agents](https://en.wikipedia.org/wiki/User_agent) standardně poskytují focus styly. Ty se ale značně liší a v mnoha případech nestačí[^6].

Chcete-li normalizovat chování prohlížeče a zviditelnit focus styly, odstraňte výchozí styl a implementujte svůj vlastní. Jakékoli CSS deklarace může být použita na stav `:focus`. Mějte ale na paměti, že focus styly jsou styly _funkční_ a musí být viditelné za všech okolností.

V některých prohlížečích jsou obrázky na pozadí eliminovány, když je aktivní Mód Vysokého kontrastu ve Windows. Samotné změny barvy by také selhaly v kritériu **WCAG 2.1 1.4.1 Use of Color**[^7]. Místo toho aplikujte přiměřeně tlustý `outline`. Ve většině případů by měla sdílet `color` textu (protože kontrast pro `color` by měl být již odlišen od pozadí stránky). K dosažení vynechejte hodnotu barvy:

```css
:focus {
  outline: 2px solid;
}
```

Možná zjistíte, že uživatelé myši jsou zmateni nebo rozptylováni focus stylem, který se objeví při kliknutí na prvek (:active a :focus nastanou současně). Díky progresivnímu zlepšování můžete potlačit focus styly pro uživatele myši v prohlížečích, které podporují `:focus-visible`.

```css
:focus {
  outline: 2px solid;
}

:focus:not(:focus-visible) {
  outline: none;
}
```

Druhá deklarace je zahozena, pokud prohlížeč nepodporuje `:focus-visible`, to znamená, první deklarace zůstává nedotčena a všech uživatelům klávesnic jsou poskytnuty focus styly.

## Doporučené chování

Hlavní zodpovědností za chování při focusu není narušit standardní chování prohlížeče. Pouze interaktivní prvky by obvykle měly obdržet focus, a to v očekávaném a logickém pořadí. Jak již bylo uvedeno, měli byste se proto vyhýbat pozitivním hodnotám `tabindex` a vizuálnímu uspořádání, které by bylo v rozporu s pořadím ve zdroji. Uživatelé by měli mít možnost stisknout <kbd>Tab</kbd>, aby zaměřili na další interaktivní prvek, a <kbd>Shift</kbd> a <kbd>Tab</kbd>, aby zaměřili na předchozí.

Ve vzácných případech je nutné naprogramovaný focus (focus vyvolaný uživatelem). Například [**Dialogy akcí/Modaly**]() se musí při otevření zaměřit. Pokud se tak nestane, uživatel klávesnice nebude schopen snadno najít a ovládat ovládací prvky tohoto dialogu. Je důležité, aby dialogy měly správné sémantické informace (v tomto případě: `role="dialog"` a přidružený label). Tyto informace poskytují kontext, když je přesunut focus uživatele, a dávají mu vědět, co se stalo a kde je.

### Klávesové pasti (Keyboard traps)

Za všech okolností by mělo být možné opustit webovou stránku a prohlížeč pomocí klávesnice. V souladu s tím **WCAG 2.1 2.1.2 No Keyboard Traps**[^8] mluví o _přesunutí_ focusu z libovolného prvku, který lze zaměřit.

Nedoporučuje se omezovat focus na vnitřek dialogu (a mezi různými prvky dialogu). Tato technika uživatelům ztěžuje focus.

Místo toho, jak je uvedeno v [**Dialogy akcí/Modaly**](), veškerý obsah mimo dialog by měl během otevření dialogu obdržet vlastnost `inert`. Tato vlastnost (a její polyfill[^9]) odstraní obsah jak z pořadí focus, tak z oznámení odečítačky obrazovky. Dialog se stane jediným zaměřitelným kontextem _by default_.

```html
<div class="content" inert></div>
<div role="dialog" open>...</div>
```

## Související výzkum

Toto téma nemá související uživatelský výzkum.

### Pokračovat ve čtení, jinde na webu

[^1]: Assistive technology — Wikipedia, <https://en.wikipedia.org/wiki/Assistive_technology>
[^2]: WCAG2.1 2.1 KeyboardAccessible, <https://www.w3.org/TR/WCAG21/#keyboard-accessible>
[^3]: WCAG2.1 2.4 Navigable, <https://www.w3.org/TR/WCAG21/#navigable>
[^4]: Using the `tabindex` attribute — The Paciello Group, <https://developer.paciellogroup.com/blog/2014/08/using-the-tabindex-attribute/>
[^5]: `writing-mode` — MDN, <https://developer.mozilla.org/en-US/docs/Web/CSS/writing-mode>
[^6]: Avoid default browser focus styles — Adrian Roselli, <http://adrianroselli.com/2017/02/avoid-default-browser-focus-styles.html>
[^7]: WCAG2.1 1.4.1 Use of Color, <https://www.w3.org/TR/WCAG21/#use-of-color>
[^8]: WCAG2.1 2.1.2 No Keyboard Trap, <https://www.w3.org/TR/WCAG21/#no-keyboard-trap>
[^9]: WICG `inert` (polyfill), <https://github.com/WICG/inert>
