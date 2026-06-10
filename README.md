# Research Paper Translation

`research-paper-translation` is a Codex/Claude skill for close-reading research papers in Chinese.

It is designed for cases where a normal "translate this paper" prompt is not enough:

- the user cares about the exact PDF version
- the translation must stay paragraph-aligned
- numbers, model names, datasets, and metrics must match the source
- summaries must not silently replace full translation

This repository packages the skill in a layout that is ready for local installation today and compatible with a future submission to `openai/skills` `.experimental`.

## What makes it different

- Version-aware: treat the user's actual PDF as the source of truth
- Extraction-aware: prefer real PDF text extraction before translating
- Paragraph-aligned: translate paragraph by paragraph for close reading
- Fact-preserving: recheck numbers, metrics, and section references
- Chinese-reader focused: optimized for faithful Chinese translation, not generic multilingual summarization

## Repository Layout

```text
research-paper-translation/
├── LICENSE.txt
├── README.md
└── skills/
    └── research-paper-translation/
        ├── LICENSE.txt
        ├── SKILL.md
        ├── agents/
        │   └── openai.yaml
        ├── references/
        │   └── pdf-extraction.md
        └── scripts/
            └── extract_pdf_text.py
```

## Install

### Codex local install

Copy the skill directory into your Codex skills folder:

```bash
mkdir -p ~/.codex/skills
cp -R skills/research-paper-translation ~/.codex/skills/
```

Restart Codex after installing.

### Claude local install

```bash
mkdir -p ~/.claude/skills
cp -R skills/research-paper-translation ~/.claude/skills/
```

### Codex install from GitHub

After this repository is published to GitHub, install it with `$skill-installer` or the helper script. Example URL shape:

```text
https://github.com/<your-github-handle>/research-paper-translation/tree/main/skills/research-paper-translation
```

## Usage Prompts

Use prompts like:

- `用 $research-paper-translation 逐段翻译这篇论文，不要漏句子。`
- `Use $research-paper-translation to read this PDF, lock the exact version, and produce a paragraph-aligned Chinese translation.`
- `用 $research-paper-translation 检查这版中文译文是不是和原论文逐段对得上。`

## PDF Extraction Helper

The bundled script tries extraction engines in this order:

1. `PyMuPDF` (`fitz`)
2. `pdftotext`
3. `pdfplumber`

Example:

```bash
python3 skills/research-paper-translation/scripts/extract_pdf_text.py paper.pdf
```

If no `--output` path is given, the script writes a `.txt` file into the system temp directory and prints the chosen engine and output path.

## Scope

Best for:

- arXiv papers
- conference and journal PDFs
- paragraph-by-paragraph Chinese close reading
- translation review against the original paper

Not optimized for:

- scanned image PDFs that require OCR-heavy recovery
- generic contracts, manuals, or web articles
- summary-only reading workflows

## License

MIT. See [LICENSE.txt](LICENSE.txt).
