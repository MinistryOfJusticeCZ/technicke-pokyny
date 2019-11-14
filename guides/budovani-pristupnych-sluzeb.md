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

Pokud je služba vytvořena pomocí existujícího systému nebo systémem třetích stran, například CMS nebo JavaScript framework, měla by být testována její přístupnost.

### Testování se skutečnými uživateli

Abyste vyhodnotili, jak jsou služby přístupné měly by být testovány se skutečnými uživateli. To může znamenat veřejnost i specializované organizace, které takové testování s uživateli nabízí.

Testování se skutečnými uživateli může být časově náročný proces. Doporučujeme ho až poté, co už nějaké testování přístupnosti proběhlo.

## Specifické pokyny

Existuje běžná mylná představa, že přístupnost je obtížná, časově náročná a nákladná. To není pravda, nicméně rozsah možných problémů, které by se mohly vyskytnout, je příliš široký na to, aby zde byl podrobně popsán. Uvádíme tedy několik běžných problémů s přístupností, které má mnoho webů, proč jsou problémem a jak jim zabránit.

### HTML není semantické

Sémantické HTML znamená použití příslušného prvku pro popisovaný prvek. Běžným příkladem je nepoužívání prvku tlačítka pro tlačítko, ale spoléhání se na JavaScript, který zajistí správnou akci při použití tohoto prvku.

To je problém, protože nesémantické HTML je pro technologie, jako jsou odečítačky obrazovky, těžko pochopitelné. To znamená, že někteří uživatelé nebudou moci s těmito prvky pracovat nebo s nimi interagovat, což potenciálně činí použitelnost služby.

### Špatná navigace na stránce

Jak se webové stránky stávají stále většími, je běžné, že v horní části stránky jsou navigační sekce obsahující spoustu odkazů na jiné části webu. Je důležité poskytnout mechanismus pro přeskočení těchto prvků k hlavnímu obsahu pro uživatele používající klávesnice, například odkaz Přeskočit na...

Měli byste vytvářet stránky pomocí landmarků, abyste uživatelům umožnili snadnější navigaci. 

### Nevhodná struktura nadpisů

Uživatelé odečítačky obrazovky se mohou spolehnout na správnou strukturu nadpisů na stránce, aby se jí mohli navigovat a porozumět jejímu obsahu. Název stránky by měl být uveden v prvku H1 a všechny ostatní nadpisy by se měly řídit logickým uspořádáním, např. Hl, H2, H3, ne Hl, H3, H2.

### Prvky formuláře nemají přiřazený label

All form controls should have a label associated with them that describes what the form control should be used for. Placeholder text is not an acceptable alternative.

Labels should provide a short but clear explanation of a form control. Further detail can be provided using a separate element. This should also be associated with the form control by using an attribute such as aria-describedby.

### Text odkazu je bez významu nebo je duplikovaný

Text odkazu by měl popisovat, na co tento odkaz odkazuje. Kontext by se neměl spoléhat na okolní text. Uživatelé kompenzančních technologií mohou procházet stránkami podle typu prvku, což znamená, že text odkazu, například „klikněte sem“ nebo „přečíst více“, nepomůže uživateli pochopit, kam odkaz směřuje.

### Špatný kontrast barev

Uživatelé mohou mít řadu problémů se zrakem, včetně barevné slepoty nebo situačních postižení, jako je například oslnění obrazovky. Text na webové stránce by měl být vždy jasný a čitelný a obecně je třeba se vyhnout obrázkům na pozadí za textem.

Úroveň AA WCAG 2.1 vyžaduje kontrastní poměr alespoň 4,5: 1 pro normální text a 3:1 pro velký text. Nástroje pro kontrolu kontrastu, jako je například kontrola kontrastu WebAIM, mohou poskytnout rychlý způsob, jak otestovat, zda je text čitelný.

### Špatné navigování pomocí klávesnice

Uživatelé používající klávesnici se spoléhají na stavy zaměření (focus), aby věděli, kde na stránce jsou. To znamená, že prvky, jako jsou odkazy a ovládací prvky formulářů, musí mít v CSS nastaven stav zaměření, aby bylo možné určit, kdy jsou tyto prvky zaměřeny klávesnicí.

Tímto problémem se vracíme zpět k sémantickému HTML. Používání nevhodných prvků může uživatelům používající klávesnice odebrat přístup k ovládacím prvkům na stránce, což dělá službu nepoužitelnou.

### Neflexibilita

Služby by měly mít responizivní layout, aby fungovaly na jakékoli velikosti obrazovky. Měl by být brán zřetel na uživatele, kteří si přizpůsobují prohlížení webu, a to buď pomocí ovládacích prvků webového prohlížeče nebo pomocí konkrétních nástrojů. Příklady těchto přizpůsobení zahrnují, zvětšení velikosti písma nebo použití vlastní šablony stylů, jako je mód s vysokým kontrastem.

### Složité interaktivní stránky

Stránky, které poskytují uživateli detailní interakci, musí být vytvořeny s ohledem na přístupnost. Pokud jsou prvky stránky dynamicky aktualizovány pomocí JavaScriptu, měli byste použít atributy jako aria-control a aria-live, takže uživatelé odečítačky obrazovky jsou informováni o změnách obsahu stránky.

### Obrázky bez alternativního popisu

Nevidomí uživatelé nemohou pochopit obrázek bez smysluplného alternativního textu (alt text). Alt text by měl stručně vyjádřit záměr obrázku. Nemusí jej nutně úplně popsat.

Například alt text „vřelý vánoční stůl“ může být užitečnější při zprostředkování záměru obrazu než „talíř s řízky, bramborami, nádivkou, omáčkou, bábovkou a mrkví“. Obrázek s prázdným alt textem je však lepší než obrázek bez atributu alt.