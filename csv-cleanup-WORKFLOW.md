# Reproduce the CSV cleanup example

Self-initiated, AI-assisted portfolio demonstration using fictional data. This
is a local Python command-line utility, not a hosted app or a CRM integration.
The owner has not yet personally completed this practice workflow. No client
engagement, paid demand, time savings, or skill mastery is claimed.

This public companion ZIP contains editable utility source, fictional inputs,
recorded outputs, and these instructions. It excludes internal motion-production
records. Its filename matches the approved video's getting-started card.

## Start here

Extract `redline-cleanup-editable-source.zip`. Open a terminal in the extracted
folder containing this WORKFLOW.md, then use the commands below. Python 3.10 or
newer must already be installed. Initial dependency setup needs an internet
connection to download the pinned openpyxl package and its dependency. The
cleanup itself runs locally without a service account or paid tool.

Windows PowerShell:

```powershell
cd utility-demo
py -3 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe cleanup.py --input input/batch_a.csv input/batch_b.xlsx --output-dir output/my-baseline --dedupe-key customer_id --dedupe-policy exact-only
```

macOS or Linux:

```sh
cd utility-demo
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python cleanup.py --input input/batch_a.csv input/batch_b.xlsx --output-dir output/my-baseline --dedupe-key customer_id --dedupe-policy exact-only
```

If Python is unavailable, install it through your usual approved software route.
If an output folder already contains files, choose a new folder name rather
than overwriting earlier results. Keep all input column headers unchanged.

## Inspect the result

Open `utility-demo/output/my-baseline/summary.txt`: 18 input records become
15 output records after 3 exact duplicates are removed. There are 6 review
rows, 5 missing cells, and 1 retained conflicting customer-ID group.

`cleaned.csv` contains all 15 retained rows. `review.csv` copies the 6 flagged
rows from those 15; it is not 6 additional records. `duplicates.csv` records
removed duplicates. `summary.txt` and `summary.json` explain the counts.
The recorded reference files are in `output/baseline/`.

The utility trims outer spaces, preserves inner spaces, and removes exact
whole-row repeats only when the customer ID is present. It retains conflicting
rows and blanks. It does not guess missing information, validate email
addresses, fuzzy-match people, or choose the winning conflicting record.
Formula-like CSV values are protected with a leading apostrophe.

## Simulated human revision

The supplied `input-corrected/batch_a_corrected.csv` changes one cell: fictional
customer 00103, Mila Shah, receives `mila.shah@example.test`. This is invented
sample information supplied for the demonstration, not inferred by the tool.
You can reproduce this edit by copying `input/batch_a.csv` to that corrected
filename and changing only the empty email cell for customer 00103 in a text
or spreadsheet editor. Preserve IDs as text (including leading zeros), the
headers, and CSV format. In real work, confirm the value with the data owner.

From the same `utility-demo` terminal folder, run:

Windows PowerShell:

```powershell
.\.venv\Scripts\python.exe cleanup.py --input input-corrected/batch_a_corrected.csv input/batch_b.xlsx --output-dir output/my-corrected --dedupe-key customer_id --dedupe-policy exact-only
```

macOS or Linux:

```sh
.venv/bin/python cleanup.py --input input-corrected/batch_a_corrected.csv input/batch_b.xlsx --output-dir output/my-corrected --dedupe-key customer_id --dedupe-policy exact-only
```

Compare `output/my-corrected/summary.txt` with the baseline: review rows fall
from 6 to 5 and missing cells from 5 to 4. The 15 output records, 3 removed
duplicates, and 1 conflict group are unchanged. Reference outputs are in
`output/corrected/`. Review the remaining records before any separate import.
No CRM connection or import is included or performed.

## Demonstration and verification boundaries

The 94-second video combines explanatory animation with browser-rendered views
of actual saved command/output evidence. Its illustrated edit is not a live
application screen recording. A text alternative accompanies the video.
This packaged utility and sample results were checked on Linux with Python
3.12 and openpyxl 3.1.5. The Windows commands are provided for reproduction;
this publication package has not been newly tested on a physical Windows PC.
