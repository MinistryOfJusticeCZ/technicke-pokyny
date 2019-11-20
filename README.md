# Technické pokyny

Jak budujeme a spravujeme služby na Ministerstvu spravedlnosti. Toto repo je inspirována, a půjčuje si ze stránky [Technické pokyny GDS][gds-way].

Stránka je vytvořena za použití [Jekyllu][Jekyll], a hostována za použití [GitHub Pages][GitHub Pages]. Používá HTML, SCSS, JavaScript, a obrázky z [GDS's Tech Docs
Template][tech-docs-template]. Stránka je přetvořena, aby fungovala s Jekyllem, namísto [Middlemana][Middleman].

[gds-way]: https://github.com/alphagov/gds-way
[Jekyll]: https://jekyllrb.com
[GitHub Pages]: https://pages.github.com
[tech-docs-template]: https://github.com/alphagov/tech-docs-template
[Middleman]: https://middlemanapp.com

## Jak začít

Nainstalujte Ruby a [Bundler][bundler], nejlépe přes [Ruby version
manager][rvm].

[rvm]: https://www.ruby-lang.org/en/documentation/installation/#managers
[bundler]: http://bundler.io/

Jakmile máte Ruby a Bundler nastaveny, nainstalujte do této složky dependence pomocí:

```bash
bundle install
```

## Spuštění

Pro spuštění spusťte tento příkaz:

```bash
bundle exec jekyll serve --watch
```

To vytvoří lokální web server, pravděpodobně na http://127.0.0.1:4000
(sledujte řádek s `Server address:`). Při změně v Markdown souboru se stránky updatují automaticky (díky `--watch`).

## Big Thank you
To all these amazing people that did the hard work to make it easier:
* [Government Digital Service](https://github.com/alphagov)
* [18F](https://github.com/18F)
* [Government of Ontario](https://github.com/ongov)
* [NHS](https://github.com/nhsuk)
* [Co-op](https://github.com/coopdigital)
* [UK Home Office](https://github.com/UKHomeOffice)
* [U.S. Digital Service](https://github.com/usds)
* [República Argentina](https://github.com/argob)
