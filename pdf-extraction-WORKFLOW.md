# Field Ledger — start here

This is a self-initiated, AI-assisted fictional portfolio demonstration. The owner has not yet personally practiced the workflow. It is not a client project, proof of paid demand or skill mastery, or a measured time-saving result. The MP4 is a silent explanatory film, not a live tool recording.

Extract `field-ledger-motion-source.zip`. Its root has this `WORKFLOW.md`; the executable sample is in `pdf-extraction/toolproject/`. The 27 PDFs in `demo/inputs/` contain invented people and service records. The tool reads stored text values from known fillable AcroForms, using the explicit aliases in `schema.json`. It does not infer missing values, OCR scans, or edit input PDFs.

## Run the fictional batch

Requires Python 3.10 or newer. From the extracted ZIP root on Linux or macOS:

```sh
cd pdf-extraction/toolproject
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python -m src.extractor demo/inputs outputs/my-demo-run-01
printf 'Exit code: %s\n' "$?"
```

Dependency setup may download the pinned packages; the extractor itself runs locally and makes no network requests. Use a **new** output directory each time. Existing destinations are refused with exit code 1. This example completes with exit code **2**, meaning it created a receipt but found records requiring review: **23 extracted + 1 needs review + 3 quarantined = 27**.

For Windows PowerShell, after extracting the ZIP, use:

```powershell
cd pdf-extraction/toolproject
py -3 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m src.extractor demo\inputs outputs\my-demo-run-01
$LASTEXITCODE
```

Open `outputs/my-demo-run-01/COMPLETE.json` first. Then inspect `extracted-records.xlsx`: `RunSummary` for counts, `Validation` for issues, and `Records` plus `RawFields` against the source PDF. `audit.json` records decoded values, field states, source hashes, and quarantine reasons; `schema-used.json` records the mapping. The `quarantine/` folder holds unchanged copies of three rejected PDFs. In `Records`, 27 **data** rows include those three, apart from title and header rows. Intake 03 retains `SRV-0003`, `José Müller`, and the text serial `000000003`; intake 23 has an empty required company and `needs_review` status. A workbook edit does not update the PDF or audit.

## Try the simulated revision

The supplied `revisions/required-contact.schema.json` makes `contact_name` required. From `pdf-extraction/toolproject/`:

```sh
.venv/bin/python -m src.extractor demo/inputs outputs/my-contact-revision-01 --schema revisions/required-contact.schema.json
printf 'Exit code: %s\n' "$?"
```

This also exits **2**, with **21 extracted + 3 needs review + 3 quarantined = 27**. Intake 04 and intake 16 change to review status because their names are blank. Mapped values and source PDFs remain unchanged. Choose another fresh output folder for any subsequent run.

On Windows PowerShell, from `pdf-extraction/toolproject/` after the baseline setup:

```powershell
.\.venv\Scripts\python.exe -m src.extractor demo\inputs outputs\my-contact-revision-01 --schema revisions\required-contact.schema.json
$LASTEXITCODE
```

## Run the tests

From `pdf-extraction/toolproject/` after setup:

```sh
.venv/bin/python -m pip install -r requirements-dev.txt
.venv/bin/python -m unittest discover -s tests -v
```

The test suite uses ReportLab to make synthetic PDFs. It checks values, Unicode, leading zeros, formula-like text, unsupported inputs, collisions, and output rollback. This package was freshly extracted and checked on Linux with already installed Python dependencies; no new package installation was needed for that check. Windows and macOS commands are guidance, not tested claims.

On Windows PowerShell, from `pdf-extraction/toolproject/`:

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements-dev.txt
.\.venv\Scripts\python.exe -m unittest discover -s tests -v
```

The ZIP also contains editable motion files: `render.py`, `art.py`, `pdf-extraction/scenes.py`, and its two image assets. Rendering is separate from running the extractor and requires Python with pycairo plus `ffmpeg` with libx264. From the ZIP root, `python3 render.py pdf-extraction --output field-ledger-local-render.mp4` makes a new film. Renderer font selection and layout may differ across systems; the included approved MP4 remains the reference export.
