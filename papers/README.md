# Papers

Put real generated papers in this folder.

Start from a template:

```bash
cp ../templates/paper2_mcq_template.tex my_paper2.tex
cp ../templates/paper4_theory_template.tex my_paper4.tex
```

Files in this folder should load the shared style and assets with:

```latex
\usepackage{../templates/igcsephysics}
\renewcommand{\assetpath}{../assets}
```

Build from this folder:

```bash
pdflatex -synctex=1 -interaction=nonstopmode -file-line-error my_paper2.tex
```

Run `pdflatex` twice when the paper has figure references or page counts.

