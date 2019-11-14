---
category: Principy pro vývoj
expires: 2019-03-31
---

# Principy frontend vývoje

Seznam několika principů, které vývojáři při implentaci frontendu služby na MSp dodržují. Tyto principy nejsou povinností, ale ve chvíli, kdy se od nich odchýlíte, musíte zdůvodnit proč.

Tyto principy jsou rozšířením pro [Principy vývoje MSp]({{ '/principles/vyvoj-principy' | relative_url }}).

## 1. Řiďte se Standardem pro tvorbu služby

## 2. Udělejte vaši službu přístupnou
Stránka musí splňovat úroveň AA [Web Content Accessibility Guidelines 2.0](https://www.w3.org/WAI/intro/wcag). Zároveň byste měli, co nejčastěji zkoušet asistenční technologie. I přesto všechno, nebudete mít kompletní přehled problémů, které služba může obsahovat. Další co vám může pomoc je, jako součást continuous integration, automatizované testovaní přístupnosti.

- [pa11y](https://github.com/springernature/pa11y)
- [Automatizovné testování přístupnosti v Rails za pomoci capybara-accessible](https://content.pivotal.io/blog/automated-accessibility-testing-in-rails-with-capybara-accessible)


## 3. Udělejte kód jednoduchý natolik, aby se dal kopírovat nebo přeložit
Veškerý text v aplikaci, který uvidí uživatel, by měl být na tom samém místě v kódu, typicky v šablonách. Tím je zároveň myšlen i obsah zobrazený na základě podmínky nebo chybové zprávy. Dále se ujistěte, že lze službu přeložit do jiného jazyka.

## 4. Podporujte co možná nejvíce prohlížečů
Předpokládáme, že stránka bude fungovat na všech prohlížečích zmíněních na stránace [Verified Browsers](https://www.gov.uk/service-manual/user-centred-design/browsers-and-devices.html#verified-browsers). I přesto se ale snažte zahrnout, co nejvíce možných prohlížečů. Prohlížeč s 0.1% podílem na trhu stále znamená přes 4 000 návštěvníků měsíčně na portálu justice. Vyhněte se [browser sniffingu](http://www.sitepoint.com/why-browser-sniffing-stinks/). Namísto toho, použijte feature detection. Tím zároveň zajistíte, že stránka bude [dopředně kompatibilní](https://cs.wikipedia.org/wiki/Dopředná_kompatibilita).

## 5. Používejte responzivní design
Používejte media queries, preferujte relativní jednotky (zároveň s použitím px kvůli kompatibilitě) a používejte plynulé mřížkové rozložení (fluid grid layouts).

## 6. Používejte progresivní zlepšování
Navrhujte stránky s nejmenším spločeným jmenovatelem pro funkčnost (například, HTML4, žádný JavaScript, žádné CSS). Až na základě toho, budujte styly a chování. Ujistěte se, že všechno skriptování a stylizování, které přidáváte, je důležité pro použití dané služby.

## 7. Neoptimalizujte předčasně
Optimalizování webových stránek pro jejich rychlost může vést k velice složitému kódu, který rozbíjí základní principy, jako separace stylu a obsahu. Optimalizujte pouze v případě, že máte fakta a důkazy, že současná performance front-endu je podstatnou částí pro to, aby se mohla služba používat.

## 8. Pracujte s týmem
Měli byste vytvořit úzký tým s vizuálním designérem a designérem pro obsah. Věci se začnou řídit waterfall přístupem, když je tak budete předpokládat: designér designuje design, designér pro obsah poskytuje obsah, vy implementujete. Takto to ale nesmí fungovat. Nepřetržitá zpětná vazba a komunikace funguje mnohem lépe.

## 9. Nepředpokládejte příliš mnoho o chování uživatele
Nejste uživatel. Můžete dělat pouze předpoklady, jak bude stránka uživateli používána. Často budou špatně. Naslouchejte uživatelskému výzkumníkovi (user researcher), jelikož ví více než vy. Povědomí o fungování stránky mohou poskytnout Analytics, ty ale nenahradí pozorování uživatelů při používání služby.

## 10. Buďte obezřetní s volbou technologie
(<i>frameworky, build nástroje, jazyky</i>)

Můžete být seznámeni, nebo vás mohou zajímat, spousty technologií. Například, SASS, Gulp, CoffeeScript, TypeScript, EcmaScript 6, Polymer, a další. Pamatujte však na to, že váš kód by měl být udržitelný dlouho po vašem odchodu. Musíte si být jisti, že technologie, kterou chcete použít, bude i nadále mít aktivní komunitu: budou ji vyvojáři znát nebo o ní budou slyšet, bude existovat dokumentace, budou nové verze zpětně kompatibilní, a tak dál. jQuery, Grunt/Gulp a SASS jsou dnes velice rozšířenými, a jsou považovány za vhodné.

## 11. Pozor na single-page apps
Je obtížné navrhnout single-page aplikace (SPA) tak, aby byly přístupné, nezavislé na prohlížeči, umožnily progresivní zlepšování, a být search-engine friendly. Proto jsou ve státní správě vzácné. Předtím než se rozhodnete pro SPA, musíte si být jisti, že všichni uživatelé mohou službu využít, nehledě na jejich zařízení. Nejčastěji budou SPA implementovány pro interní služby, kde je potřeba nejnižší možné použití technologie na jiné úrovni.

## 12. Pište unit testy
Zpravidla aplikace nebudou obsahovat "pure" JS kód. Používejte proto unit testy, pouze pokud JS funkce nepřímo modifikují DOM.

## 13. Pište funkční testy
To znamená, testy, které replikují cestu uživatele službou (user journey). Zahrňte všechny možné cesty, ne jen tu hezkou cestu.

- mělo by jich být hodně a měly by proběhnout při každé změně kódu
- měly by být vyjádřeny v čitelné formě (například, [Prison visits](https://github.com/ministryofjustice/prison-visits/blob/master/spec/features/unexpected_journey_spec.rb#L56))
- nekontrolujte pouze obsah stránky, ale i vypočítané CSS

BDD (za použití [cucumber](https://cucumber.io/) nebo jakékoliv jiné human-readable formy psaní testů) bude fungovat, pokud má pro to tým tu správnou workflow. Pokud nemáte někoho (ideálně věcného garanta), který by tyto testy psal a kontroloval, zda procházejí, budete ztrácet čas překládáním testů v kódů, namísto psaní těchto testů rovnou.

## 14. Používejte ty správné nástroje
Při psaní kódu kontrolujte a debuggujete výsledky ve svém oblíbeném prohlížeči. Ujistěte se ale, že výsledek zkontrolujete i v ostatních běžně používaných prohlížečích. Použijte [browsersync](https://www.browsersync.io/)
pro vyzkoušení několika prohlížečů naráz a použijte virtuální stroje pro starší prohlížeče (například, [modern.ie](https://dev.windows.com/en-us/microsoft-edge/tools/vms/))

## 15. Implementujte SEO a sémantický markup
Mimo interní stránky a ty na .servis.justice.cz (které by měly blokovat web spidery), byste měli stránky dělat tak, aby je roboti snadno pochopili. Service standards vám sice dají základy SEO, ale někdy je dobré dělat víc, například, použít sémantický markup, aby mohly search enginy nalézt věci jako otevírací dobu soudů, adresu. [Schema.org](https://schema.org/) v současné chvíli nabírá na popularitě, a davá smysl prouzkomat to blíže.

