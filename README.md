# testto-pay-docs

Documentação pública do Testto Pay (MkDocs Material), publicada em https://wmreis-labs.github.io/testto-pay-docs/.

```bash
python3 -m venv .venv && .venv/bin/pip install -r requirements.txt
.venv/bin/mkdocs serve
```

Antes de publicar: `mkdocs build --strict`. O deploy é o workflow `pages.yml`, a cada push em `main`.
