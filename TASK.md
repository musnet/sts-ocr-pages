# Task: transcribe textbook page images to Markdown + LaTeX

`images/` holds 613 scanned pages of a maths textbook (`page-0007.png` … `page-0619.png`, 1448×1890 px).
For **every** image, write `pages/page-XXXX.md` (same number) containing a faithful transcription.
The output will be used as AI training data and to rebuild a real-text PDF, so accuracy matters more than speed.

## How to work
- Look at each image yourself (Read tool) and transcribe it. Do not use OCR libraries.
- Parallelise with subagents: each subagent handles a contiguous batch of ~20 pages
  (e.g. 0007–0026, 0027–0046, …) and writes those `.md` files. Run several at once.
- Skip pages whose `.md` already exists (so the job can resume).
- Commit and push to `main` after every finished batch (`git add pages && git commit -m "pages XXXX-YYYY" && git push`).
- When all 613 files exist, check for any missing numbers, fill them, and push a final commit.

## Transcription rules
- Transcribe **all** text in reading order, verbatim — including running headers ("Real Numbers  3"),
  page numbers such as `(viii)`, figure captions, and "Ans." markers. Do not summarise, correct, or skip anything.
- Markdown structure: `#`/`##`/`###` for headings (UNIT, chapter, section titles, boxed titles like
  "SOLVED EXAMPLES"), `**bold**` and `*italic*` as they appear, numbered/lettered lists, Markdown tables for tables.
- **All mathematics in LaTeX**: inline `$...$`, display `$$...$$`.
  e.g. `$2 \times 3^{2} \times 13$`, `$\sqrt{2}$`, `$\frac{p}{q}$`, `$\sin^{2}A + \cos^{2}A = 1$`,
  `$30^{\circ}$`, `$\therefore$`, `$\angle ABC$`, `$\triangle PQR$`, `$\overline{AB}$`, `$\pi r^{2}$`.
  Multi-line working (aligned `=` signs) → `$$\begin{aligned} ... \end{aligned}$$`.
- **Figures / diagrams / graphs** (anything that is a drawing, not text): insert a placeholder line
  `![figure](bbox:x0,y0,x1,y1)` where the numbers are the approximate pixel box of the drawing in the
  1448×1890 image (top-left origin). Include text *inside* the drawing (labels, numbers in a factor tree)
  in the image, not in the transcription. Put its caption (e.g. `**Fig. 1.2.**`) as normal text after it.
- Blank page → file containing just `<!-- blank page -->`.
- Illegible text → `[illegible]`. Never invent content.
