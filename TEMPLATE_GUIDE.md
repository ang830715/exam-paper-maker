# IGCSE Physics LaTeX Template Guide

This guide explains how to use `igcse_physics_template.tex` to create Cambridge-style IGCSE Physics practice papers.

The template is designed so most users only need to edit the metadata at the top and replace the sample questions in the document body.

## Build Setup

Use VS Code with the LaTeX Workshop extension.

This project already has `.vscode/settings.json` configured to use `pdflatex` with SyncTeX:

```json
"-synctex=1",
"-interaction=nonstopmode",
"-file-line-error"
```

Useful LaTeX Workshop actions:

- Build PDF: `Ctrl+Alt+B`
- Sync from `.tex` cursor to PDF: `Ctrl+Alt+J`
- Sync from PDF to `.tex`: `Ctrl+click` in the PDF preview

On macOS, install MacTeX or BasicTeX, then make sure `pdflatex` is available in the terminal:

```bash
pdflatex --version
```

For this Mac workspace, `.vscode/settings.json` is pinned to the standard MacTeX executable:

```bash
/Library/TeX/texbin
```

If you move the project back to Windows, change the VS Code tool command from `/Library/TeX/texbin/pdflatex` to `pdflatex`, or to the full Windows path for your TeX installation.

The build uses `-synctex=1`, so source/PDF syncing works after a successful build.

## Paper Metadata

Edit these commands near the top of the `.tex` file:

```latex
\newcommand{\examtitle}{PHYSICS}
\newcommand{\examcode}{0625/42}
\newcommand{\papername}{Paper 4 Theory (Extended)}
\newcommand{\examseason}{Practice Paper}
\newcommand{\examtime}{1 hour 15 minutes}
\newcommand{\exammarks}{80}
\newcommand{\examyear}{2026}
\newcommand{\examfooter}{0625/42/PRACTICE}
```

Do not edit the cover-page layout helpers unless changing the visual design of the front page.

## Layout Notes

The template uses `11pt` text, matching the real Cambridge papers we checked. Do not reduce the class size to make text fit; if a line wraps too early, check the question layout widths first.

Question text widths are calculated from `\textwidth`, not `\linewidth`, so part labels and question stems can use the full available line. This matters for long starts such as:

```latex
\questionpart{a}{Fig. \figref{fig:river-canoe} shows water in a river moving parallel to the river bank at 4.0m/s and a canoe travelling in the river.}{8}
```

Question and part labels use fixed 8 mm label columns. This keeps the spacing close to the Cambridge layout while allowing references and question text to update automatically when questions are reordered.

## Cover Page Helpers

The cover page uses normal LaTeX layout, with small TikZ drawings only for the candidate-number boxes and logo approximation. This is more stable than drawing the whole first page as an overlay.

Important helper commands:

```latex
\nameentry
\candidateentry{CENTRE}{\candidateboxes{5}}
\candidateentry{CANDIDATE}{\candidateboxes{4}}
```

The candidate-number boxes have a custom midpoint baseline so the labels are vertically centered against the boxes. If the front-page labels look misaligned after editing, check `\candidateboxes` before changing random vertical spaces.

## Basic Question Structure

Start each main question with:

```latex
\question{Question stem text here.}{8}
```

The second argument is the total mark for that question. End the question with:

```latex
\totalmarks
```

If the question starts immediately with a part label, such as `3 (a) Fig. 3.1 shows...`, use:

```latex
\questionpart{a}{Fig. \figref{fig:river-canoe} shows water in a river.}{8}
```

Then use `\parttext{...}` for continuation text under the same part before starting `(b)`.

Example:

```latex
\question{A trolley is released from rest and moves down a slope.}{9}

\partquestion{a}{Define acceleration.}
\writtenline{ }
\writtenline{ }
\marksright{2}

\totalmarks
```

## Text After Figures

Use `\qtext{...}` for normal text under the main question, especially after a figure:

```latex
\question{Fig. \figref{fig:electric-bicycle} shows an electric bicycle.}{8}
\figplaceholder[fig:electric-bicycle]{10}{4.5}

\qtext{When fully charged, the battery can deliver a power of 600 W for 60 min.}
```

This keeps the text aligned with the main question text column. Figure numbers are generated from the current question number, so this example becomes `Fig. 1.1` if it is in question 1 and `Fig. 3.1` if the question is moved to question 3.

## Parts: `(a)`, `(b)`, `(c)`

Use:

```latex
\partquestion{a}{State the form of useful energy gained by the load.}
```

If a part has a second sentence on a new line, use `\parttext{...}`:

```latex
\partquestion{b}{The input energy supplied to the motor is 12 J.}
\parttext{Calculate the efficiency of the motor.}
```

## Nested Parts: `(a)(i)`, `(a)(ii)`

For the first nested part, use:

```latex
\partsubquestion{a}{i}{Calculate the energy stored in the battery.}
```

For later nested parts under the same letter, use:

```latex
\subpartquestion{ii}{State the form of energy stored by the battery.}
```

If a nested part needs continuation text, use:

```latex
\subparttext{Calculate the useful energy transferred to the load.}
```

Example:

```latex
\partsubquestion{a}{i}{State the form of useful energy gained by the load.}
\subpartwrittenanswer{ }{1}

\subpartquestion{ii}{The load has a mass of 0.45 kg and is raised through a vertical height of 1.8 m.}
\subparttext{Calculate the useful energy transferred to the load.}
\working[24mm]
\answer{energy}{J}{3}
```

## Answer Lines

### Full Written Answer Lines

For normal full-width dotted answer lines, use:

```latex
\writtenanswer{ }{1}
```

For nested `(a)(i)` answer lines, use:

```latex
\subpartwrittenanswer{ }{1}
```

These commands make the dotted line extend toward the right side, like the real paper.

### Lines Without Marks

For a line without a mark at the end:

```latex
\writtenline{ }
```

For nested parts:

```latex
\subpartwrittenline{ }
```

### Short Answer Lines

Only use short lines when the real answer is intentionally short, such as one word:

```latex
\shortwrittenanswer{ }{1}
\subpartshortwrittenanswer{ }{1}
```

Avoid using short lines for normal written responses.

### Two Numbered Lines

For "State two..." style answers:

```latex
\twowrittenlines
\marksright{2}
```

## Calculation Answers

Use:

```latex
\answer{quantity}{unit}{marks}
```

Example:

```latex
\working[24mm]
\answer{energy}{J}{3}
```

This produces a right-aligned Cambridge-style calculation answer:

```text
energy = ................. J [3]
```

If the answer line needs a different length:

```latex
\answer[80mm]{time}{s}{2}
```

## Working Space

Use vertical space for calculations:

```latex
\working[24mm]
```

The default is:

```latex
\working
```

Adjust the size depending on the amount of working expected.

## Answer Boxes

Use an answer box for longer explanations:

```latex
\answerbox[34mm]
\marksright{4}
```

The optional argument controls the box height.

## Figures

Use the placeholder command while drafting:

```latex
\figplaceholder[fig:setup]{10}{4.5}
```

Arguments:

1. Optional label for cross-references
2. Width in cm
3. Height in cm

Refer to the figure with:

```latex
Fig. \figref{fig:setup}
```

Figure numbers reset inside each main question. The first figure in question 2 is numbered `Fig. 2.1`, the second is `Fig. 2.2`, and so on. If questions are reordered, the figure references update after rebuilding twice.

Replace the placeholder with a real `tikzpicture` or `\includegraphics` later if needed:

```latex
\examfigure{fig:vase}{
  \includegraphics[width=75mm]{assets/diagrams/vase.png}
}
```

For cropped diagrams from a PDF, use `\includegraphics` options:

```latex
\examfigure{fig:original-setup}{
  \includegraphics[
    page=3,
    trim=40mm 80mm 60mm 120mm,
    clip,
    width=80mm
  ]{0625_s22_qp_42.pdf}
}
```

## Tables And Graphs

Plain LaTeX tables and TikZ graphs can be inserted directly. Keep them centered unless the real paper layout suggests otherwise.

Example graph grid:

```latex
\begin{center}
\begin{tikzpicture}[x=0.5cm,y=0.5cm]
  \draw[step=0.5cm,gray!55,very thin] (0,0) grid (18,10);
  \draw[thick,->] (0,0) -- (18.5,0) node[right] {length};
  \draw[thick,->] (0,0) -- (0,10.5) node[above] {resistance};
\end{tikzpicture}
\end{center}
```

## Page Breaks

Use:

```latex
\newpage
```

Use:

```latex
\blankpage
```

only when you deliberately need a page labelled `BLANK PAGE`.

## Important Style Rules

When generating new questions, follow these rules:

- Use `\partsubquestion` and `\subpartquestion` for `(a)(i)`, `(a)(ii)` layouts.
- Use subpart answer helpers for nested subparts.
- Use full written answer lines for ordinary written responses.
- Use short answer lines only for short one-word or phrase answers.
- Put marks at the right using helper commands, not manual spaces.
- Do not manually type many dots. Always use the answer-line commands.
- Use `\working[...]` before calculation answer lines.
- End every main question with `\totalmarks`.
- Keep question text generic and original; do not copy full past-paper questions unless permission allows it.

## Common Mistakes

Wrong for nested subparts:

```latex
\partsubquestion{a}{i}{State the form of energy.}
\writtenanswer{ }{1}
```

Correct:

```latex
\partsubquestion{a}{i}{State the form of energy.}
\subpartwrittenanswer{ }{1}
```

Wrong for calculation marks:

```latex
\answer{energy}{J [3]}
```

Correct:

```latex
\answer{energy}{J}{3}
```

Wrong for normal full written responses:

```latex
\shortwrittenanswer{ }{2}
```

Correct:

```latex
\writtenanswer{ }{2}
```
