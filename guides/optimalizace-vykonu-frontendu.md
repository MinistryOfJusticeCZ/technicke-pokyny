---
category: Tvorba služby
expires: 2018-03-15
---

# Jak optimalizovat výkon frontendu

Při vývoji webových stránek vaší služby byste se měli zaměřit na [performance frontendu][]. Tím, že váš web bude reagovat rychleji a lépe fungovat na všech zařízeních, zlepšíte uživatelský prožitek (UX) z vaší služby.

## Upřednostněte úkoly spojené s výkonem

Prioritizujte věci, které musíte dělat (vysoká) před úkoly střední nebo nízké priority (nice-to-have).

Například:

|Priorita|Příklad|Akce
|---|---|---|
|Vysoká|Správné pozicování stylů|Dejte styly na začátek a skripty na konec stránky|
||Zmenšete počet HTTP požadavků|Pro zmenšení velikosti a doby načtení minimalizujte ikony, CSS a JavaScript soubory [HTTP/1.1 only]|
||Komprimujte statické assety|Pro kompresi CSS a JavaScript kódu použijte [minifikaci][] a [Gzip][]|
||Nastavte spravné Hlavičky|Pro optimální caching nastavte spravné [Cache-Control][] a [ETag][] hlavičky na assetech|
|Střední|Hledejte prázdné `src` image atributy|Vyhněte se použivání prázdných `src` image atributům, jelikož některé prohlížeče jim přesto pošlou požadavek|
||Redukujte DNS lookupy|Použijte co nejméně domén třetích stran, abyste tak redukovali počet DNS lookupů per stránku|
||Maximalizujte paralelizaci|Servujte statické assety z různých subdomén; prohlížeče tak mohou stáhnout více assetů paralelně [HTTP/1.1 only]|
||Prozkoumejte [lazy loading][]|Pro stránky se spoustou obrázků načítejte pouzy ty, které jsou ve viewportu prohlížeče|
||Prozkoumejte dopad načítání vícero [@font-face][] assetů|Prozkoumejte @font-face assety, pokud řešíte problémy s [FOUT, FOIT a FOFT][]|
|Nízká|Nastavte správně obrázky a sprity|Pro jednodušší parsování browseru, nastavte obrázky a sprity horizontálně|
||Redukujte velikost cookie|Protože každá cookie je posílána s každým HTTP požadavkem, zvažte použití cookie-free domény pro statické assety [HTTP/1.1 only]|
||[AJAX][] požadavky využívající JSON|Vyhněte se přidávání velkého množství dat JSON Objectu, jelikož to způsobuje výkonnostní chyby při parsování|
||Prozkoumejte použití [WebSockets][]|Zvažte použití WebSockets namísto [XMLHttpRequest][]; HTTP request packet má 1,684 bytů overhead, s porovnáním s 8 bytovým WebSocket packetem|
||Prozkoumejte použití service workera| Zvažte použití service workera pro cachování kritických assetů na strojích uživatelů, namísto jejich posílání přes síť

## Automatizujte optimalizaci

Optimalizaci výkonu můžete automatizovat za použití nástrojů jako:

- [Gulp][]
- [Grunt][]
- [Broccoli][]
- [npm-scripts][]

Tyto nástroje byste měli integrovat do vaší [workflow pro Continuous Delivery (CD) a Continuous Integration (CI)][], aby se automaticky před nasazením (deployment) spustily.

Zvažte automatizování běžných úkonů jako:

- CSS a JS linting a optimalizace
- CSS a JS minifikace
- optimalizace obrázků
- generování spritů a ikon
- optimalizace SVG obrazu

[Google PageSpeed][] může spoustu těchto úkonů udělat za vás.

## Automatizované testování

Testování výkonu frontendu můžete automatizovat za použití služeb třtích stran jako:

- WebPagetest
- Google PageSpeed Insights
- SpeedCurve

Měli byste si [stanovit performance budget][]. Jakmile máte performance budget nastaven, otestujte, zda vaše stránky zůstavají v tomto budgetu. Existují nástroje, která vám toto umožní jako:

- grunt-prefbudget
- grunt-pagespeed
- performance-budget
- PSI - PageSpeed Insights s reportingem
- Google Lighthouse

## Pokračovat ve čtení

You can find out more about improving your website’s frontend performance by reading:

- [Setting a performance budget][]
- [My performance audit workflow][]
- [Front-end performance for web designers and front-end developers][]
- [Improving web app performance with the Chrome DevTools Timeline and Profiles][]
- [Google Web Fundamentals: Optimizing Content Efficiency][]

Britský Service Manual má další doporučení [jak můžete testovat výkon frontendu][].

[performance frontendu]: https://www.gov.uk/service-manual/technology/how-to-test-frontend-performance
[minifikaci]: http://minifycode.com/
[Gzip]: https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer#text_compression_with_gzip
[Cache-Control]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control
[ETag]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag
[lazy loading]: https://developers.google.com/web/fundamentals/performance/lazy-loading-guidance/images-and-video/#what_is_lazy_loading
[@font-face]: https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face
[FOUT, FOIT a FOFT]: https://css-tricks.com/fout-foit-foft/
[AJAX]: https://developer.mozilla.org/en-US/docs/Web/Guide/AJAX/Getting_Started
[WebSockets]: https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API
[XMLHttpRequest]: https://xhr.spec.whatwg.org/
[Gulp]: https://gulpjs.com/
[Grunt]: https://gruntjs.com/
[Broccoli]: http://broccolijs.com/
[npm-scripts]: https://css-tricks.com/why-npm-scripts/
[workflow pro Continuous Delivery (CD) a Continuous Integration (CI)]: https://www.gov.uk/service-manual/technology/deploying-software-regularly
[Google PageSpeed]: https://developers.google.com/speed/pagespeed/module/
[stanovit performance budget]: https://www.gov.uk/service-manual/technology/how-to-test-frontend-performance#set-a-performance-budget
[Setting a performance budget]: https://timkadlec.com/2013/01/setting-a-performance-budget/
[My performance audit workflow]: https://aerotwist.com/blog/my-performance-audit-workflow/
[Front-end performance for web designers and front-end developers]: https://csswizardry.com/2013/01/front-end-performance-for-web-designers-and-front-end-developers/
[Improving web app performance with the Chrome DevTools Timeline and Profiles]: https://addyosmani.com/blog/performance-optimisation-with-timeline-profiles/
[jak můžete testovat výkon frontendu]: https://www.gov.uk/service-manual/technology/how-to-test-frontend-performance
[Google Web Fundamentals: Optimizing Content Efficiency]: https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/