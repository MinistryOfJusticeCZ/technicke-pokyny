---
category: Rozhodnutí pro komponenty
---

# Inset text, breakout boxy, alerty

## Úvod

Někdy stojí za to zobrazit poznámku nebo upozornit uživatele na určité informace, ale tyto informace nelze považovat za součást hlavního smyslu dokumentu. Pro tyto případy použijte komponentu **Inset text**.

**Inset text** může obsahovat tipy, varování, poznámky k výzkumu, zajímavá fakta a podobně.

## Doporučený markup

Následující příklad je typu „Poznámky“ (Note). Stejná struktura by měla být použita i pro jiné typy, které se liší pouze obsahem a ikonografií.

```html
<aside class="inset-text" aria-labelledby="aside-1540915290281">
  <h4 aria-hidden="true" id="aside-1540915290281">
    <svg class="icon icon--text">
      <use xlink:href="{{site.basedir}}static/images/icons-all.svg#icon-info"></use>
    </svg>
    Unikátní název inset textu
  </h4>
  <!-- Obsah insetu; obvykle odstavec nebo dva -->
</aside>
```

### Poznámky

* **`<aside>`:** Prvek `<aside>` je dělící prvek a jako takový je (někdy pomocí termínu „complementary“) v softwaru pro odečítačku obrazovky identifikován. Je brán jako landmark[^1] a je součástí souhrnných nabídek všech landmarků odečítačky obrazovky, díky čemuž je vysoce zjistitelný (JAWS otevírá menu landmarku/regionu pomocí <kbd>Insert</kbd> + <kbd>Ctrl</kbd> + <kbd>R</kbd>)[^2].
* **`aria-labelledby`:** Tato vlastnost připojuje (přidružuje) `<aside>` `id` nadpisu. V souhranných nabídek všech landmarků, je možné identifikovat **Inset text** na základě jeho labelu. Tento label je ohlášen společně se svou ('complementary') rolí, ve chvíli, kdy uživatel přejde do prvku `<aside>`.
* **`aria-hidden="true"`:** **Inset texty** by neměly být považovány za součást hlavní struktury dokumentu rodiče; jejich záhlaví slouží pouze k označení a prezentaci. Atribut `aria-hidden` odstraní nadpis z dokumentu (a z navigace odečítačky obrazovky), ale _neumlčí_ textový uzel jako přidružený label.

#### Unikátní hodnoty
Pro přidružení `aria-labelledby` s daným `id`, musí sdílet hodnotu a tato hodnota musí být unikátní. Ve vašem templatovacím systému, vytvořte například string v čase kompilace za použití `Date()`:

```js
const unique = + new Date(); // například 1540915290281
```

`id`, které není unikátní je považováno za parsing error a, jelikož to ovlivňuje pojmenování z hlediska přístupnosti, znamenalo by to nesplnění **WCAG 4.1.1 Parsing**[^3].

## Doporučený layout

Prvek `<h4>` je použit, aby vyvolal nadpis/label dané velikosti, i přesto že jeho sémantika je odstraněna z dokumentu (viz [**Doporučený markup**](#doporučený-markup)).

Případná ikonka musí mít výplň (fill) `currentColor`, aby odpovídala barvě sousedního textu a respektovala nastavení pro vysoký kontrast.

### Vysoký kontrast

`background-color`, která ohraničuje **Inset text** ve stránce, bude v Módu Vysokého kontrastu odstraněna. Použijte průhledný horní a dolní rámeček. Ten se stane viditelným jako plná barva, když je zapnuta funkce Windows HCM, čímž se dosáhne stejného účelu ohraničení.

```css
.inset-text {
  background: #eee;
  border-top: 1px solid transparent; /* pro mód vysokého kontrastu */
  border-bottom: 1px solid transparent; /* pro mód vysokého kontrastu */
}
```

## Doporučené chování

**Inset text** nemá žádné speciální chování (je to statická komponenta).

## Související výzkum

Toto téma nemá související uživatelský výzkum.

### Pokračovat ve čtení, jinde na webu

[^1]: "ARIA Landmarks Example: Complementary Landmark" — W3C, <https://www.w3.org/TR/wai-aria-practices/examples/landmarks/complementary.html>
[^2]: "JAWS Keystrokes" — Freedom Scientific, <https://doccenter.freedomscientific.com/doccenter/archives/training/jawskeystrokes.htm>
[^3]: WCAG 1.4.1 Parsing, <https://www.w3.org/TR/WCAG20/#ensure-compat>