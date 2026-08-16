# Personal site

Osobista strona + blog, hostowana na GitHub Pages (Jekyll, budowany automatycznie przez GitHub).

## Jak dodać nowy post

Utwórz plik w katalogu `_posts/` o nazwie `RRRR-MM-DD-tytul-posta.md`, np. `_posts/2026-08-16-pierwszy-post.md`:

```markdown
---
layout: post
title: "Tytuł posta"
---

Treść posta w Markdownie...
```

Po `git push` GitHub Pages przebuduje stronę automatycznie (zwykle w ciągu ~1 minuty).
Post pojawi się na liście na stronie głównej pod adresem `/blog/RRRR/tytul-posta/`.

## Struktura

- `index.md` — strona główna (landing page + lista postów)
- `_posts/` — posty bloga
- `_layouts/` — szablony HTML (default, post)
- `assets/css/style.css` — style
- `_config.yml` — konfiguracja Jekylla
