```
import zipfile
from pathlib import Path

ZIP = "upsell_all.zip"      # the zip file's name
OUT = "."                   # where to put the files ("." = this folder)

with zipfile.ZipFile(ZIP) as z:
    z.extractall(OUT)
    print(len(z.namelist()), "files extracted to", Path(OUT).resolve())

```
