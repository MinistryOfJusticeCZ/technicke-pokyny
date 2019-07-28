# Technical guidance

Jak budujeme a spravujeme služby na Ministerstvu spravedlnosti. Toto repo je inspirována, a půjčeno ze stránky [Technické pokyny GDS][gds-way].

Stránka je vytvořena za použití [Jekyllu][Jekyll], a hostována za použití [GitHub Pages][]. Používá HTML, SCSS, JavaScript, a obrázky z [GDS's Tech Docs
Template][]. Stránka je přetvořena, aby fungovala s Jekyllem, namísto [Middlemana][Middleman].

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
