# Pre-IG Final Mock

This folder contains a combined mock paper generated from `../2025-2026 pre-IG Final Mock.docx`.

The source questions in the Word document are embedded as pictures. The LaTeX paper transcribes the question text and answer choices into the project template, and uses cropped source images only for diagrams and figure blocks.

The Word cover metadata states `Pre-IG Theory`, `June 2026`, and `40` marks. The embedded source image sequence contains 13 multiple-choice question blocks followed by 5 structured question blocks; all visible source question blocks are included in document order.

This mock uses the default school practice front page, so it does not show the `For Examiner's Use` marking table. Use `\schoolexamfrontpage` only for school exam papers that need that table.

Build from this folder with XeLaTeX so the PDF embeds Arial:

```bash
xelatex -synctex=1 -interaction=nonstopmode -file-line-error pre_ig_final_mock.tex
xelatex -synctex=1 -interaction=nonstopmode -file-line-error pre_ig_final_mock.tex
```

## Manual Diagram Crops

The source `.docx` stores questions as screenshots. The text is transcribed into LaTeX, and clean diagram crops live in:

```text
assets/manual_crops/
```

Crop only the diagram content. Do not include source question text, source `Fig. x.x` captions, answer lines, or surrounding whitespace. For MCQ diagram choices, include the A/B/C/D labels if they are part of the answer-choice diagram block.

The current manual crops are named by question and content:

```text
q04_density_blocks.png
q05_turning_effect_choices.png
q06_spring_apparatus.png
q06_extension_graph_choices.png
q07_stone_liquid.png
q08_meter_circuit_choices.png
q09_equilibrium_choices.png
q10_prism_dispersion_choices.png
q11_mirror_reflection_choices.png
q12_changes_state_mcq.png
q13_dolls_house_circuits.png
q14_changes_state_structured.png
q15_vase_forces.png
q16_plastic_rod_cloth.png
q18_mirror_reflection.png
q18_lens_image.png
q19_metre_rule.png
```

Question 3 still uses a cropped source-question screenshot because no separate manual crop for the arrow direction diagram is currently present.
