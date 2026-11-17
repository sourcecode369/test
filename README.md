```
python

import zipfile
from pathlib import Path

KEEP = {".ipynb", ".py", ".md", ".csv", ".json"}      # code, notebooks, results tables, settings
with zipfile.ZipFile("upsell_all.zip", "w", zipfile.ZIP_DEFLATED) as z:
    for p in Path(".").rglob("*"):
        if p.is_file() and p.suffix in KEEP and "data" not in p.parts and ".ipynb_checkpoints" not in p.parts:
            z.write(p)
print(Path("upsell_all.zip").stat().st_size / 1e6, "MB")

```
