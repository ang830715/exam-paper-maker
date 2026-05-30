# Codex Notes For This Project

This project creates Cambridge-style IGCSE Physics practice papers in LaTeX.

## Project Shape

- `templates/igcsephysics.sty` contains the shared layout, packages, cover page, and helper commands.
- `templates/paper2_mcq_template.tex` is the blank Paper 2 multiple-choice starter.
- `templates/paper4_theory_template.tex` is the blank Paper 4 theory starter.
- `examples/` contains filled reference papers and reusable examples.
- Each real generated paper should live in its own project-root folder, e.g. `pre_ig_final_mock/`.
- `assets/` contains reusable diagrams and cropped source images.

## Rules For Codex

- Do not recreate the old single-file template structure.
- Do not add duplicate `igcsephysics.sty` loader files in subfolders.
- Edit `templates/igcsephysics.sty` only when changing shared layout or helper commands.
- For a new paper, create a dedicated project-root folder, copy a starter file from `templates/` into it, and edit the copy.
- Keep paper metadata and question content in the paper `.tex` file.
- The default front page is `\schoolpracticefrontpage`; use `\schoolexamfrontpage` only when a school exam needs the `For Examiner's Use` marking table.
- Keep `\caieofficialfrontpage` available for official-style covers.
- Keep shared visual design and macros in `templates/igcsephysics.sty`.
- When a paper lives outside `templates/`, load the style by relative path.
- When a paper lives in its own root-level folder, use:

```latex
\usepackage{../templates/igcsephysics}
\renewcommand{\assetpath}{../assets}
```

- Use the helper commands documented in `TEMPLATE_GUIDE.md`; do not manually type dotted answer lines or manual mark spacing.
- Build from the folder containing the `.tex` file.

## Build Command

Use:

```bash
pdflatex -synctex=1 -interaction=nonstopmode -file-line-error paper_name.tex
```

For cross-references and page counts, run the command twice.

## Starting A New Paper

Paper 2:

```bash
mkdir my_paper2
cp templates/paper2_mcq_template.tex my_paper2/my_paper2.tex
cd my_paper2
pdflatex -synctex=1 -interaction=nonstopmode -file-line-error my_paper2.tex
pdflatex -synctex=1 -interaction=nonstopmode -file-line-error my_paper2.tex
```

Paper 4:

```bash
mkdir my_paper4
cp templates/paper4_theory_template.tex my_paper4/my_paper4.tex
cd my_paper4
pdflatex -synctex=1 -interaction=nonstopmode -file-line-error my_paper4.tex
pdflatex -synctex=1 -interaction=nonstopmode -file-line-error my_paper4.tex
```

After copying, update the metadata near the top of the paper file:

```latex
\renewcommand{\examcode}{0625/22}
\renewcommand{\papername}{Paper 2 Multiple Choice (Extended)}
\renewcommand{\examtime}{45 minutes}
\renewcommand{\exammarks}{40}
\renewcommand{\examfooter}{0625/22/PRACTICE}
```

## Examples To Consult

- Use `examples/paper2_mcq_sample.tex` and `examples/paper2_mcq_sample_body.tex` for MCQ layout patterns.
- Use `examples/paper4_theory_sample.tex` for structured question, figure, calculation, answer-line, and graph patterns.
- Use `TEMPLATE_GUIDE.md` for command details and style rules.
