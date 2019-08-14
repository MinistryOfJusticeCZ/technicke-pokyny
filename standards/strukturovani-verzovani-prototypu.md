---
category: Design služby
---

# Strukturování a verzování prototypů

pozn. prozkoumat možnosti glitch.com

Jak udržovat a organizovat verze prototypů pro desingéry a výzkumníky.

## Konvence pro názvy

Pokud pro hostování prototypů používáte Heroku, ujistěte se, že vaše appka začíná ``msp-``. Toto pomáhá identifikovat prototyp jako službu Ministerstva spravedlnosti, a příspívá tak k lepší spolupráci napříč úřady.

Vyhněte se zkratkám a zkratkovým slovům. Designéři tak vědějí, o jakou službu se jedná, předtím než otevřou odkaz. Za tím samým účelem, dávejte prototypu (a repositáři s kódem) popisné názvy.

Pojmenovávejte verze Heroku stejným jménem, novým verzím jen přidávejte příponu ``-1`` nebo ``-2``.

## Verzování

Verzování pomáhá udržovat přehled o minulých verzí prototypu.

Pomocí git tagů, můžete udělat snapshoty prototypů, které jste vytvořily. Přesto ale můžete udržovat minulé verze i bez tohoto tagování. Ve chvíli, kdy vytvoříte nový prototyp, vytvořte novou Heroku appku s číslem verze (msp-project-v2). Poté vypněte automatické aktualizace z GitHubu.

Heroku umožňuje mít až 100 appek (pokud přidáte platební inforamce - nebudete platit poplatek).

Příklad stránky s tabulkou, která udržuje předchozí verze a changelog.

## Udržování logu změn, které jste udělali

Když pracujete rychle a děláte spoustu změn v návrhu (designu), snadno ztratíte přehled, jaké rozhodnutí jste udělali. V seznamu verzí prototypu, můžete ke každé verzi přidat "changelog", který by popisoval změny v návrhu a jejich opodstanění.

## Vlastnictví a převod vlastnictví

Převod vlastnictví prototypů by mělo být součástí jakéhokoliv předávání výstupů mezi designéry.

Můžete převést, jak GitHub repositáře, tak Heroku appkyYou can transfer both GitHub repos and Heroku apps. [Převod GitHub repositářů](https://help.github.com/en/articles/transferring-a-repository)

[Převod Heroku appek](https://devcenter.heroku.com/articles/transferring-apps)

## Best practice pro výzkum

Výzkumnící chtějí vědět, jakými iteracemi prototyp prošel. K tomu může pomoc používání "changelogu", jako nástroj k uchovávání důležitých informací zjištěných z výzkumu.

Zajistěte, aby výzkumníci mohli prototypy jednoduše procházet. Jasně značte verze, co se změnilo a proč.

## Pokračovat ve čtení

Další informace o [Dělání prototypů](https://www.gov.uk/service-manual/design/making-prototypes) můžete najít na na stránkách GOV.UK.