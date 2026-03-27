# Litigation Timeline Builder

**Scanned PDF claim file in, chronological Word timeline out.**

You have a 500-page scanned PDF from an insurance claim or litigation file. You need a timeline. Normally that's 40+ hours of a paralegal flipping through pages. This tool does it in minutes.

## What it does

1. **OCR** - Renders every page and extracts text via Tesseract (parallel, configurable DPI)
2. **Segment** - Detects document boundaries (where one letter/email/report ends and another begins)
3. **Classify** - Identifies document types: emails, letters, proof of loss, invoices, estimates, motions, sworn statements, etc.
4. **Extract dates** - Pulls all dates from each document, picks the primary one
5. **Describe** - Generates a concise description (sender, subject, key amounts, claim numbers)
6. **Output** - Builds a formatted `.docx` table sorted chronologically

Optional: pass `--llm-summarize` to use Claude for better document descriptions.

## Quick start

```bash
git clone https://github.com/IAmNotTheSource/litigation-timeline.git
cd litigation-timeline

python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# Make sure Tesseract is installed
# macOS: brew install tesseract
# Ubuntu: sudo apt install tesseract-ocr

python timeline_builder.py claim_file.pdf
```

Output: `claim_file_timeline.docx`

## Usage

```bash
# Basic
python timeline_builder.py input.pdf

# Custom output path
python timeline_builder.py input.pdf --output my_timeline.docx

# Higher OCR quality (slower)
python timeline_builder.py input.pdf --dpi 300

# More parallel workers
python timeline_builder.py input.pdf --workers 8

# Cache OCR results (skip OCR on re-runs)
python timeline_builder.py input.pdf --cache

# Print document summary to terminal
python timeline_builder.py input.pdf --summary

# Use Claude for better descriptions (needs ANTHROPIC_API_KEY)
pip install anthropic
export ANTHROPIC_API_KEY=your-key
python timeline_builder.py input.pdf --llm-summarize
```

## What it detects

**Document types:** Property loss notices, proof of loss, reservation of rights letters, sworn statements, examination under oath, demand letters, complaints, summons, motions, invoices, payment summaries, damage estimates, activity logs, adjuster notes, emails, general correspondence, and more.

**Date formats:** `MM/DD/YYYY`, `YYYY-MM-DD`, `Month DD, YYYY`, `DD Month YYYY`, day-of-week formats, and two-digit year formats.

**Smart segmentation:** Detects page 1 markers, continuation pages, estimate report blocks, and document-start patterns to correctly group multi-page documents together.

## Dependencies

- [PyMuPDF](https://pymupdf.readthedocs.io/) - PDF rendering
- [pytesseract](https://github.com/madmaze/pytesseract) + [Tesseract OCR](https://github.com/tesseract-ocr/tesseract) - OCR engine
- [python-docx](https://python-docx.readthedocs.io/) - Word document generation
- [Pillow](https://pillow.readthedocs.io/) - Image handling
- [dateparser](https://dateparser.readthedocs.io/) - Date extraction
- [anthropic](https://docs.anthropic.com/) - Optional, for LLM-enhanced descriptions

## Customizing

The document-start patterns, document type identifiers, and boilerplate filters are all defined as lists at the top of `timeline_builder.py`. Add your own insurance companies, document types, or date patterns directly.

## License

MIT. Use it however you want.
