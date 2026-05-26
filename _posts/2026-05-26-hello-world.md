---
title: "Hello World – pierwszy post"
date: 2026-05-26 12:00:00 +0200
categories: [Blog]
tags: [test, code, markdown]
---

To jest testowy post sprawdzający wygląd czcionek, bloków kodu i tabel. Jeśli wszystko gra – San Francisco na macOS wygląda świetnie bez Google Fonts w tle.

## Nagłówek H2

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Chirpy renderuje treść postów przez standardowy pipeline Jekyllowy – Markdown → HTML, z podświetlaniem składni przez Rouge.

### Przykład kodu – Python

```python
import sys

def greet(name: str) -> str:
    """Zwraca powitanie dla podanej nazwy."""
    return f"Hello, {name}!"

if __name__ == "__main__":
    names = ["Alice", "Bob", sys.argv[1] if len(sys.argv) > 1 else "World"]
    for n in names:
        print(greet(n))
```

### Przykład kodu – Shell

```bash
# Sprawdź aktualną wersję gema Chirpy
bundle info --path jekyll-theme-chirpy

# Zbuduj stronę lokalnie
JEKYLL_ENV=production bundle exec jekyll build
```

### Przykład kodu – YAML (config snippet)

```yaml
# _config.yml
title: "My Blog"
theme: jekyll-theme-chirpy

assets:
  self_host:
    enabled: false
```

## Tabela

| Urządzenie     | Rok  | CPU              | RAM    |
|:---------------|:----:|:-----------------|-------:|
| iPhone 5c      | 2013 | Apple A6         | 1 GB   |
| Xbox 360       | 2005 | IBM Xenon        | 512 MB |
| Dell Optiplex  | 2016 | Intel i5-6500    | 8 GB   |

## Kod inline

W terminalu możesz użyć `bundle exec jekyll serve --livereload` żeby oglądać zmiany na żywo. Plik konfiguracyjny to `_config.yml`, a posty lądują w katalogu `_posts/`.

## Blockquote

> Dobry blog techniczny to nie kwestia platformy – to kwestia regularności i jakości treści.

To tyle na ten moment. Reszta postów pojawi się wkrótce.
