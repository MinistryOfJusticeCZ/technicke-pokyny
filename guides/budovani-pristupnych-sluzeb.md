---
category: Tvorba služby
expires: 2018-03-15
---

# Budování přístupných služeb

Vládní služby musí být přístupné všem. To zahrnuje kohokoli s poruchami zraku, sluchu, řeči, motoriky nebo učení. Patří sem také kdokoli s dočasným nebo situačním postižením, jako je zlomená ruka nebo práce v hlučném prostředí.

Problém s přístupností webu může být něco, co ovlivňuje každého, nejen lidi, kteří web používají pouze pomocí klávesnice nebo odečítačky obrazovky.

Vládní služby musí být ze zákona přístupné. Standard WCAG 2.1 AA pro přístupnost je nutností. Nová regulace znamená, že stávající webové stránky musí být kompatibilní do roku 2020. Nativní aplikace musí být kompatibilní do června 2021.

## Jak zpřístupnit vaši službu

### Myslete na přístupnost od začátku

Přístupnost nemůžete dosáhnout přidáním některých kosmetických úprav na konci &ndash; musí být brána v potaz ve všech fázích projektu. Měli byste zkontrolovat návrhy pro možné problémy, psát a spouštět testy během vývoje a testovat služby s ohledem na přístupnost.

### Pochopte, že ne každý čte stejným způsobem

Uživatel může procházet stránkou shora dolů, možná že čte pouze nadpisy a podnadpisy a skáče mezi odstavci, aby našel požadovaný obsah.

Nevidomí uživatelé mohou stránku také skákat. Odečítačky obrazovky mohou oznamovat obsah podle typu prvku, jako jsou nadpisy, odstavce nebo odkazy. Pokud například uživatel odečítačky obrazovky očekává, že stránka obsahuje data v elementu tabulky, jako je například jízdní řád, může začít pročítat všechny tabulky na stránce.

Proto je při budování přístupných služeb důležité [sémantické značkování](https://html.com/semantic-markup/) a dobrá struktura nadpisů.

### Automatizované testování

Nástroje pro testování přístupnosti, jako jsou [aXe](https://www.deque.com/axe/) a [Lighthouse](https://developers.google.com/web/tools/lighthouse/), jsou užitečné při hledání problémů, ale [automatizované testování najde pouze kolem 30 % pravděpodobných problémů s přístupností](https://alphagov.github.io/accessibility-tool-audit/). Automatizované testování by nemělo být jediným způsobem, jak testovat přístupnost. Mělo by být také dojít k testování se skutečnými uživateli.

### Testování kompatibility prohlížeče

Služby by měly být důkladně testovány ve všech prohlížečích, u nichž se vyžaduje kompatibilita, včetně mobilních zařízení a nástrojů pro jako jsou odečítačky obrazovky. Emulované testování prohlížečem služby je dovoleno, ale doporučuje se také některé testování na skutečných mobilních zařízeních.

Pro informaci jsou to [prohlížeče, s nimiž je justice.cz kompatibilní](https://github.com/MinistryOfJusticeCZ/tym-dodavatel/blob/master/pristupnost/podporovane-prohlizece.md) a služba by také měla být použitelná s těmito [asistenčními technologiemi](https://www.gov.uk/service-manual/technology/testing-with-assistive-technologies#what-to-test).

### Otestujte obsah serverovaný systémy třetích stran

Pokud je služba vytvořena pomocí existujícího nebo systémem třetích stran, například CMS nebo JavaScript framework, měla by být testována na přístupnost.

### Testování se skutečnými uživateli

Abyste vyhodnotili, jak jsou služby přístupné měly by být testovány se skutečnými uživateli. To může znamenat veřejnost i specializované organizaceme, které nabízejí takové testování.

Testování se skutečnými uživateli může být časově náročný proces. Doporučuje se to až poté, co už nějaké testování přístupnosti proběhlo.
