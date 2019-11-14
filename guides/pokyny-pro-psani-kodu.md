---
category: Tvorba služby
expires: 2018-03-15
---

# Pokyny pro psaní kódu

## Stylizace kódu

Stylizace kódu je o konzistenci. Soulad s těmito pokyny je důležitý. Důležitější je soudržnost v rámci projektu. Soulad uvnitř
jedno modulu nebo funkce je nejdůležitější.

Je v pořádku se od pokynů odchýlit. Někdy pokyny stylizace prostě neplatí. V případě pochybností použijte svůj nejlepší úsudek. Podívejte se na jiné příklady a rozhodněte se, co vypadá nejlépe. A neváhejte se zeptat, jestli si nejste jisti.

Mezi dobré důvody pro ignorování konkrétních pokynů patří:

- při použití pokynu by kód byl méně čitelný, dokonce i pro toho, kdo je kód zvyklý číst
- konzistentnost s okolním kódem, který pokyny také porušuje (možná z historických důvodů) &ndash; i když to je příležitost vyčistit existující kód
- pokud je dotyčný kód starší než zavedení těchto pokynů a není důvod tento kód upravovat
- když kód musí zůstat kompatibilní se staršími verzemi, které nepodporují funkci doporučenou průvodcem stylem

## CSS a Sass stylizace 

### Whitespace

Použijte soft tabs s 2 space indent.

### Spacing

Měli byste použít [GOV.UK Design System spacing scale](https://design-system.service.gov.uk/styles/spacing/).

#### Přidejte fallback pro rem spacing

Vyhněte se používání `rem` jednotek pro spacing.

Pokud použijete `rem`, přidejte fallback pro prohlížeče, které jednotky `rem` nepodporují.

```css
font-size: 12px;
font-size: 1.2rem; // Tento řádek bude ignorován staršími prohlížeči
```

### Vendor prefixing

Ve chvíli, kdy používáte funkce CSS, které vyžadují prefixes použijte [autoprefixer](https://github.com/postcss/autoprefixer).

Měli byste [nakonfigurovat autoprefixer](https://github.com/postcss/autoprefixer#browsers) na [naše podporované prohlížeče](https://github.com/MinistryOfJusticeCZ/tym-dodavatel/blob/master/pristupnost/podporovane-prohlizece.md).

### Sass nestování

Sass nestování byste se měli vyhnout, kde je to jen možné.

Nadměrné používání nestování může vést ke složitému hledání selectorů a schovávat komplikované CSS, které by mělo být zjednodušené.

Výjimky tvoří pseudo selectory a JavaScript enabled třídy.

```scss
.accordion {
  // styly (not enhanced)
}
.js-enabled {
  .accordion {
    // styly (enhanced)

    &:focus {
      // ...
    }
  }
}
```

## HTML stylizace

### Podpora prohlížeče

Projděte si [seznam podporovaných prohlížečů Ministerstvem spravedlnosti](https://github.com/MinistryOfJusticeCZ/tym-dodavatel/blob/master/pristupnost/podporovane-prohlizece.md). Jedná se o prohlížeče, na kterých je povinné, aby služba běžela.

Ať už máte jakoukoli úroveň podpory prohlížeče, HTML by mělo být napsáno tímto způsobem že starší prohlížeče uvidí příslušnou fallback nebo varovnou zprávu. Například, některé prohlížeče nepodporují prvek `<video>`, proto byste se měli ujistit, že je zahrnutá nějaké kontextuální informace (v tomto příkladu prvek `<p>`):

```html
<video width="320" height="240" controls>
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.ogg" type="video/ogg">
  <p>Video není možné nahrát. Aktualizujte si prohlížeč.</p>
</video>
```

Pokud používáte prvky HTML5, například `<main>`, ujistěte se, že jste vložili [shim, který umožňuje jejich stylizaci](https://github.com/aFarkas/html5shiv) ve starších prohlížečích. V podmíněném komentáři `<!--[if lt IE 9]><![endif]->`, zahrňte daný skript tak, aby ho moderní prohlížeče nestáhly.

Před přidáním jakéhokoli CSS nebo JavaScriptu by vaše stránka měla fungovat pouze v HTML. Přečtěte si, [jak budovat odolný frontend pomocí progresivního vylepšení](https://www.gov.uk/service-manual/technology/using-progressive-enhancement).

### Struktura dokumentu

Sémanticky strukturujte stránku pomocí HTML5. Aby mohly starší asistivní (komponzační) technologie mapovat sekce stránky správně, použijte ARIA role.

Léonie Watson vysvětluje [proč použití ARIA landmark rolí přináší pro uživatele odčítačky obrazovky nejlepší prožitek](http://tink.uk/screen-readers-aria-roles-html5-support/).

Tento dokument nepředepisuje, zda se v atributech použijí jednoduché nebo dvojité uvozovky, zda vynechat hodnoty z atributů, které je nepotřebují, nebo zda přidat koncové lomítka k neplatným prvkům. Přístup, který zvolíte, by však měl být konzistentní v celém kódu.

#### Hlavička (Header)

`<header role="banner">` should be used to contain your home link, branding, search,
and any global navigation you may have.

#### Hlavní obsah

`<main role="main">` je použit k indentifikaci hlavního obsahu vaší stránky.

Aby uživatelé používající odečítačku obrazovku nebo klávesnici mohli přeskočit obecnou navigaci stránky a skočit rovnou na hlavní obsah, použijte odkaz "Přeskočit na hlavní obsah stránky".

#### Sekce/Dílčí obsah

Použijte tag `<section>`, která představuje samostatné, tematické seskupení obsahu, kde by konkrétnější sémantický prvek nebyl vhodný.

Příklady sekcí by mohly být kapitoly nebo různé stránky se záložkami v dialogovém okně se záložkami,
nebo domovskou stránku rozdělenou do sekcí, jako je úvod, novinky a kontakt.

Měli byste přidat atribut `aria-labelledby` odkazující na id záhlaví, který tuto sekci identifikuje.

Příklad:

```html
<section aria-labelledby="uvod">
    <h1 id="uvod">Úvod</h1>
    <p>Lidé chytali ryby už od pradávna...</p>
</section>

<section aria-labelledby="zarizeni">
    <h2 id="zarizeni">Zařízení</h1>
    <p>První věcí, kterou budete potřebovat, je rybářský prut nebo prut, který vám připadá pohodlný a je dostatečně silný pro druh ryb, které...</p>
</section>
```

#### Navigační prvky

Použijte `<nav role="navigation">` pro obalení skupin odkazů, které zatím nejsou v jiném kontextu (například `<footer>`). Mezi běžné příklady navigačních sekcí patří nabídky (menu), tabulky obsahu a indexy.

Stejně jako u `<section>`, doporučujeme použít `aria-labelledby` k označení vašeho `<nav>`.
To pomůže k odlišení různých `<nav>` bloků na stránce.

Na blogu GOV.UK můžete [najít více informací k tagu `<nav>`](https://insidegovuk.blog.gov.uk/2013/07/03/rethinking-navigation/).

#### Aside

`<aside role="complementary">` by měl být použit pro obsah související s primárním obsahem webové stránky, který však nepředstavuje primární obsah stránky.

Příklady zahrnují informace o autorovi, související odkazy a související obsah.

#### Patička (Footer)

`<footer role="contentinfo">` by měla být použita jako patička stránky.

### Pokyny k jednotlivým prvkům

#### Nadpisy

Nadpisy jsou v daném pořadí &ndash; například `<h1>` pak `<h2>`, ne však `<h1>` pak `<h3>`.

Na stránce je pouze jeden `<h1>` prvek.

### Zvýraznění textu

Vyhněte se kurzívě. Tučné písmo lze použít střídmě, ale může způsobit, že velké bloky textu budou špatně čitelné.

Sémanticky, slovo vyjádřená důrazem mohou být označena `<em>`, zatímco slova vyjadřující smysl nebo naléhavost nebo varování by měla být označena znakem `<strong>`.

Teoreticky, by se odečítačky obrazovkyby mohli pro tuto značku zvolit jiný tón hlasu, ačkoli v současné době to není. Prohlížeče je tradičně vykreslují kurzívou a tučně, možná budete muset přepsat `em`, abyste kurzívu nepoužili.

Zpravidla se vyhněte používání `<i>` nebo `<b>`, jelikož jsou užitečné pouze ve vzácných případech. Obvykle je lepší použít vlastnosti CSS typu "font-style" a "font-weight".

#### Obrázky

Přečtěte si [sekci k obrázkům v GOV.UK Design System](https://design-system.service.gov.uk/styles/images/).

#### Tlačítka vs Odkazy

Odkazy by měly být použity pro navigaci na jinou stránku. K odesílání formulářů by měla být použita tlačítka nebo interakce na stránce (například rozbalení prvku). Vyhýbejte se prázdným odkazů (`<a href="#">`).

Odkazy by ve výchozím nastavení neměly otevírat nové karty nebo okna, ale pokud se tak uživatelé rozhodnou (pomocí klávesové zkratky nebo kontextové menu klepnutím pravým tlačítkem), respektujte jejich volbu.

#### Vizuálně skryté prvky

Někdy může být užitečné poskytnout uživatelům používající odečítačku obrazovky další obsah, který jiní uživatelé nepotřebují vidět. Například zmíněný odkaz „Přejít na hlavní obsah stránky“.

Pro tento text nepoužívejte `display: none`, protože to odečítačky obrazovky ignorují.

Místo toho použijte CSS, který zajistí, že text bude „konceptuálně“ vykreslen, i když výsledek není na obrazovce viditelný.

Vizuálně skrytý text je často známkou toho, že něco může být zjednodušeno nebo zviditelněno tak, aby to přineslo přínos i pro vidomé uživatele. Použivejte ho proto střídmě.

## TODO JavaScript stylizace

## TODO Ruby
