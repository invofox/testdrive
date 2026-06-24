# Samples

The dataset. One folder per document type; **31 types**, **5 documents each**.

## Layout contract

```
samples/<DocumentType>/
├── originals/        # source documents — the input you feed to a model/SDK
│   └── <name>.pdf
└── ground_truth/     # expected output — one JSON per original
    └── <name>.json
```

Rules every type folder follows:

1. **Type name is `PascalCase`** — `Invoice`, `BankStatement`, `FormW2`, `IDCard`.
   It is the single source of truth for `documentType` in the JSON.
2. **Pairing by basename** — `originals/<name>.pdf` has exactly one
   `ground_truth/<name>.json`. The two sets of basenames are identical.
3. **Originals are binary** (PDF today; images may be added later) and are kept binary
   by the repo's [`.gitattributes`](../.gitattributes).
4. **Ground truth is UTF-8 JSON, LF-terminated.**

## Ground-truth schema

```json
{
  "documentType": "Invoice",
  "sourceFile": "document_01_tech_consultancy.pdf"
}
```

- `documentType` — equals the parent folder name.
- `sourceFile` — equals the file in `originals/`.

Expected extracted fields will be added to these JSONs over time.

## Validating the dataset

A quick integrity check (every original is paired, every `documentType` matches its folder):

```bash
python3 - <<'PY'
import json, os, glob
ok = True
for dt in sorted(d for d in os.listdir('.') if os.path.isdir(d)):
    pdfs  = {os.path.splitext(f)[0] for f in os.listdir(f'{dt}/originals')}
    jsons = {os.path.splitext(f)[0] for f in os.listdir(f'{dt}/ground_truth')}
    if pdfs != jsons:
        ok = False; print(f'[{dt}] unpaired:', pdfs ^ jsons)
    for j in glob.glob(f'{dt}/ground_truth/*.json'):
        if json.load(open(j)).get('documentType') != dt:
            ok = False; print(f'[{dt}] bad documentType in {j}')
print('OK' if ok else 'FAILED')
PY
```
