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

On Windows, install MiKTeX and make sure `pdflatex` is available in PowerShell:

```powershell
pdflatex --version
```

The workspace settings use the portable command name `pdflatex`, so the same `.vscode/settings.json` can be shared through Git between macOS and Windows. Avoid committing absolute paths such as `/Library/TeX/texbin/pdflatex` or a Windows user-directory path.

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

## Multiple Choice Questions

Paper 2 multiple-choice questions use a different body layout from Paper 4 theory questions. Keep the existing cover page, then use the MCQ helpers for the question body.

Start each MCQ with:

```latex
\mcquestion{Which instrument is most suitable to determine the volume of a small irregularly shaped stone?}
```

Use `\mcqtext{...}` for continuation text under the same question:

```latex
\mcquestion{A steel ball is dropped from the top floor of a building. Air resistance can be ignored.}
\mcqtext{Which statement describes the motion of the ball?}
```

### Plain Statement Choices

Use this for ordinary A-D statement choices:

```latex
\mcqchoices
  {The ball falls with constant acceleration.}
  {The ball falls with constant speed.}
  {The ball falls with decreasing speed.}
  {The ball falls with increasing acceleration.}
```

This creates a vertical A-D list aligned like Paper 2.

### Short Horizontal Choices

Use this when all choices are short values or short phrases:

```latex
\mcqchoiceswide
  {14 m/s\textsuperscript{2}}
  {24 m/s\textsuperscript{2}}
  {28 m/s\textsuperscript{2}}
  {34 m/s\textsuperscript{2}}
```

This matches the compact one-line layout used for many numerical answers.

### Diagrams In The Question Stem

For MCQ diagrams copied from a real paper, do not redraw the picture in TikZ. Crop the picture from the source PDF, save it in `assets/mcq/`, and include it with `\mcqimage`.

```latex
\mcquestion{The diagram shows a velocity-time graph for an object which is accelerating.}
\mcqimage[82mm]{assets/mcq/q03_velocity_time_graph.png}
\mcqtext{What is the acceleration of the object?}
```

Use this for graphs, rays, circuits, apparatus diagrams, and any single figure that belongs to the question stem.

The optional width controls the printed size. If omitted, the image uses the full question-text width:

```latex
\mcqimage{assets/mcq/my_diagram.png}
```

### Cropping MCQ Images From A PDF

The current MCQ examples use images cropped from `0625_s25_qp_22.pdf`. The assets live in:

```text
assets/mcq/
```

Current examples:

```text
q03_velocity_time_graph.png
q04_compression_diagrams.png
q07_cricket_bat.png
q18_wave_amplitude.png
q21_refraction.png
q22_lens_image.png
q29_potential_divider.png
q32_magnetic_field_choices.png
```

On Windows or macOS, use Poppler's `pdftoppm` to crop a diagram from a PDF:

```powershell
pdftoppm -f 2 -l 2 -png -r 200 -x 450 -y 1005 -W 725 -H 610 -singlefile 0625_s25_qp_22.pdf assets\mcq\q03_velocity_time_graph
```

On macOS or Linux, use forward slashes in the output path:

```bash
pdftoppm -f 2 -l 2 -png -r 200 -x 450 -y 1005 -W 725 -H 610 -singlefile 0625_s25_qp_22.pdf assets/mcq/q03_velocity_time_graph
```

The important options are:

- `-f` and `-l`: first and last page to render
- `-r 200`: image resolution
- `-x` and `-y`: top-left corner of the crop, in pixels
- `-W` and `-H`: crop width and height, in pixels
- `-singlefile`: output one file without an added page number

After cropping, include the image:

```latex
\mcqimage[82mm]{assets/mcq/q03_velocity_time_graph.png}
```

Crop diagrams tightly enough to avoid surrounding question text, but leave a little white space so labels and arrows are not clipped.

### Future Work: Repeatable Crop Workflow

The current crop commands work, but they require trial and error because the crop box is written as raw pixel coordinates. A better future workflow for Codex is:

1. Render each source PDF page to a preview image.
2. Add a coordinate grid overlay to the preview.
3. Record each diagram crop in a manifest file, for example `assets/mcq/crops.json`.
4. Run one script to regenerate all cropped assets from the manifest.

Example manifest idea:

```json
{
  "q03_velocity_time_graph": {
    "pdf": "0625_s25_qp_22.pdf",
    "page": 2,
    "x": 450,
    "y": 1005,
    "w": 725,
    "h": 610,
    "latexWidth": "82mm"
  }
}
```

Prefer cropped PDF assets when possible, because vector diagrams stay sharper in the final LaTeX output:

```latex
\mcqimage[82mm]{assets/mcq/q03_velocity_time_graph.pdf}
```

Use PNG when the source crop/export workflow is simpler or the original figure is already raster-like.

### Equation Choices

Use vertical choices for fractions or equations unless they are very short:

```latex
\mcqchoices
  {$h=\dfrac{9.8}{2 \times 5.0^2}$}
  {$h=\dfrac{5.0^2 \times 2}{9.8}$}
  {$h=\dfrac{5.0^2}{2 \times 9.8}$}
  {$h=\dfrac{2 \times 9.8}{5.0^2}$}
```

For compact equation choices that fit comfortably on one row, use `\mcqchoiceswide`.

### Table Choices

Use `mcqtable` for answer choices that are rows in a table:

```latex
\begin{mcqtable}{|c|c|c|}
\hline
 & mass & weight \\ \hline
\textbf{A} & increases & increases \\ \hline
\textbf{B} & increases & no change \\ \hline
\textbf{C} & no change & increases \\ \hline
\textbf{D} & no change & no change \\ \hline
\end{mcqtable}
```

For wider tables, choose suitable `p{...}` columns:

```latex
\begin{mcqtable}{|c|p{32mm}|p{78mm}|}
\hline
 & type of wave & direction of vibration \\ \hline
\textbf{A} & longitudinal & parallel to the direction of travel of the wavefront \\ \hline
\textbf{B} & longitudinal & perpendicular to the direction of travel of the wavefront \\ \hline
\textbf{C} & transverse & parallel to the direction of travel of the wavefront \\ \hline
\textbf{D} & transverse & perpendicular to the direction of travel of the wavefront \\ \hline
\end{mcqtable}
```

### Diagram Choices

When the four options are diagrams from a past paper, crop the whole answer-choice diagram block as one image. This keeps the spacing, labels, key, and linework faithful to the original paper:

```latex
\mcquestion{When there is an electric current in a long straight wire, a magnetic field is created around the wire.}
\mcqtext{Which diagram shows the correct pattern and direction of magnetic field lines around a long straight wire carrying current into the page?}
\mcqimage[118mm]{assets/mcq/q32_magnetic_field_choices.png}
```

Use `\mcqchoicegrid` only when you are creating original diagram choices yourself.

### Examples Based On 0625_s25_qp_22

These examples show how to express common Paper 2 layouts using the template helpers. They are based on the real May/June 2025 Paper 2 layouts, but are kept as short formatting examples.

Plain statement choices:

```latex
\mcquestion{A ball is dropped from a building. Air resistance is negligible.}
\mcqtext{Which statement describes the motion of the ball?}
\mcqchoices
  {It has uniform acceleration.}
  {It falls at constant speed.}
  {Its speed decreases.}
  {Its acceleration increases.}
```

Short numerical choices on one row:

```latex
\mcquestion{A mass is pulled down on a vertical spring and then released.}
\mcqtext{What is the magnitude of the initial acceleration?}
\mcqchoiceswide
  {14 m/s\textsuperscript{2}}
  {24 m/s\textsuperscript{2}}
  {28 m/s\textsuperscript{2}}
  {34 m/s\textsuperscript{2}}
```

Table choices:

```latex
\mcquestion{A flexible material containing pockets of air is compressed.}
\mcqtext{What happens to its mass and weight?}
\begin{mcqtable}{|c|c|c|}
\hline
 & mass & weight \\ \hline
\textbf{A} & increases & increases \\ \hline
\textbf{B} & increases & no change \\ \hline
\textbf{C} & no change & increases \\ \hline
\textbf{D} & no change & no change \\ \hline
\end{mcqtable}
```

Equation choices:

```latex
\mcquestion{A stone is thrown vertically upwards at 5.0 m/s.}
\mcqtext{Which equation gives the maximum height $h$ reached by the stone?}
\mcqchoices
  {$h=\dfrac{9.8}{2 \times 5.0^2}$}
  {$h=\dfrac{5.0^2 \times 2}{9.8}$}
  {$h=\dfrac{5.0^2}{2 \times 9.8}$}
  {$h=\dfrac{2 \times 9.8}{5.0^2}$}
```

Diagram in the question stem:

```latex
\mcquestion{Which arrow on the graph shows the amplitude of the wave?}
\mcqimage[110mm]{assets/mcq/q18_wave_amplitude.png}
```

For this style, the answer letters are already on the diagram, so no separate `\mcqchoices` block is needed.

Diagram choices:

```latex
\mcquestion{A current flows into the page through a long straight wire.}
\mcqtext{Which diagram shows the magnetic field pattern?}
\mcqimage[118mm]{assets/mcq/q32_magnetic_field_choices.png}
```

This is preferred for past-paper-based examples because it uses the original figure block.

### Current Template Body

The current `igcse_physics_template.tex` body shows the Paper 2 MCQ examples first. The Paper 4 structured-question examples are still kept in the same file, but they are temporarily disabled while the MCQ workflow is being refined:

```latex
% Structured question examples are kept below, but temporarily disabled while
% the Paper 2 multiple-choice workflow is being refined.
\iffalse
...
\fi
```

To show the structured examples again, remove or comment out the `\iffalse` and matching `\fi`.

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

## Right-Aligned Result Answer Groups

For grouped result answers that match the Cambridge style, use right-aligned answer lines and one combined mark below the group:

```latex
\rightanswerline[80mm]{scale}
\rightanswerline[80mm]{magnitude of resultant velocity}
\rightanswerline[80mm]{direction of resultant velocity (angle from the river bank)}
\rightanswermark{4}
```

This produces a right-aligned group like:

```text
                                 scale ............................
        magnitude of resultant velocity ............................
direction of resultant velocity (angle from the river bank) ........
                                                           [4]
```

Use `\rightanswerline[length]{label}` when several related answer prompts share one mark allocation. Use `\rightanswer[label length]{label}{marks}` only for a single right-aligned answer with its own mark.

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
- Use `\rightanswerline` with `\rightanswermark` for grouped right-aligned result answers.
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
