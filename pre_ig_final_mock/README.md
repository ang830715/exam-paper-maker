# Pre-IG Final Mock

This folder contains a combined mock paper generated from `../2025-2026 pre-IG Final Mock.docx`.

The source questions in the Word document are embedded as pictures. The LaTeX paper transcribes the question text and answer choices into the project template, and uses cropped source images only for diagrams and figure blocks.

The Word cover metadata states `Pre-IG Theory`, `June 2026`, and `40` marks. The embedded source image sequence contains 13 multiple-choice question blocks followed by 5 structured question blocks; all visible source question blocks are included in document order.

Build from this folder with XeLaTeX so the PDF embeds Arial:

```bash
xelatex -synctex=1 -interaction=nonstopmode -file-line-error pre_ig_final_mock.tex
xelatex -synctex=1 -interaction=nonstopmode -file-line-error pre_ig_final_mock.tex
```

## Manual Diagram Crop Workflow

The source `.docx` stores questions as screenshots. The text is transcribed into LaTeX, but diagrams should be cropped as separate images.

To avoid tedious file naming, manually crop diagrams into:

```text
assets/manual_crops/
```

Use simple numbered names:

```text
01.png
02.png
03.png
...
```

Crop only the diagram content. Do not include source question text, source `Fig. x.x` captions, answer lines, or surrounding whitespace. For MCQ diagram choices, include the A/B/C/D labels if they are part of the answer-choice diagram block.

Expected crop order:

```text
01 arrow direction diagram
02 solid blocks density diagram
03 wooden bar turning-effect choices
04 spring-and-loads apparatus
05 extension-against-load graph choices
06 stone in liquid diagram
07 voltmeter/ammeter circuit choices
08 equilibrium force choices
09 prism dispersion choices
10 mirror reflection choices
11 changes of state diagram
12 dolls' house lighting circuits
13 changes of state structured figure
14 vase forces figure
15 plastic rod and dry cloth figure
16 mirror reflection figure
17 converging lens figure
18 metre rule equilibrium figure
```

After the crops are saved, replace the current `source_questions/...` cropped includes in `pre_ig_final_mock.tex` with direct references to `assets/manual_crops/NN.png`.
