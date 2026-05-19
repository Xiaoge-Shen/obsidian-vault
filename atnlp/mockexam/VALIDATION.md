# Mock Exam Validation

Validation date: 2026-05-19

## Source Basis

Used local course materials:

- `examguide.pdf`
- `public/2026/misc/examguide.pdf`
- selected NLU past papers in `public/2026/pastpaper`
- lecture revision files in `public/2026/lectures/*-exam-qa-zh.md`
- tutorials `public/2026/tutorials/tutorial1.pdf` to `tutorial4.pdf`
- labs under `public/2026/labs`
- coursework specification in `public/2026/coursework/coursework.pdf`

Subagent division:

- Coverage agent: extracted examinable topics, exclusions, coursework/lab/tutorial relevance.
- Style agent: extracted NLU-style constraints, mark conventions, subquestion patterns, and MCQ style.
- Blueprint agent: drafted a three-paper distribution across the exam guide.
- Main integration: wrote original papers, mark schemes, compiled PDFs, and performed validation.

## Exam Guide Compliance

- [x] Three mock papers generated.
- [x] Each mock paper is 50 marks.
- [x] Each mock paper has three compulsory main questions.
- [x] Each paper includes scenario-based applied questions.
- [x] Each paper includes a focused multiple-choice section with A-D choices.
- [x] Every multiple-choice answer in the mark schemes includes a concept explanation.
- [x] Questions use concise-answer instructions similar to the NLU past papers.
- [x] Mark schemes are separate from student-facing papers.
- [x] Coverage and difficulty guide is included.
- [x] Guest lectures were not used as primary content.
- [x] Non-examinable slide ranges in the exam guide were avoided as primary question targets.

## Mark Totals

Automatic LaTeX source check using `\pts{...}` totals:

- `mock-paper-1.tex`: 50 marks
- `mock-paper-2.tex`: 50 marks
- `mock-paper-3.tex`: 50 marks

## Topic Coverage

Paper 1:

- Machine translation
- Low-resource translation
- Data extraction and selection
- Synthetic/monolingual data use
- BPE and multilingual transfer
- QA and open-domain QA
- RAG
- Multilingual retrieval
- Evaluation, contamination, benchmark saturation
- Ethics and bias

Paper 2:

- Summarisation
- Instruction fine-tuning
- RLHF and verifiable rewards
- PEFT and LoRA
- Distillation
- Long-context modelling
- Sinusoidal PE, RoPE, positional interpolation, YaRN
- Efficient attention
- Decoding-time reasoning and self-consistency
- Safety-relevant evaluation

Paper 3:

- Perceptually grounded NLP
- VLM architecture
- Vision encoder and connector/projector design
- VLM data and evaluation
- AI agents
- Computer-use agents and trajectories
- Reliability and safety of agents
- Mechanistic interpretability and sparse autoencoders
- Parsing
- LLM watermarking
- Ethics and harms
- LLMs and automata

## Build Validation

Build command used:

```bash
latexmk -pdf -interaction=nonstopmode -halt-on-error <file>.tex
```

Generated PDFs:

- `mock-paper-1.pdf`: 5 pages
- `mock-paper-1-mark-scheme.pdf`: 2 pages
- `mock-paper-2.pdf`: 5 pages
- `mock-paper-2-mark-scheme.pdf`: 2 pages
- `mock-paper-3.pdf`: 5 pages
- `mock-paper-3-mark-scheme.pdf`: 2 pages

LaTeX diagnostics:

- [x] No fatal LaTeX errors.
- [x] No overfull hbox warnings after final rebuild.
- [x] No underfull hbox warnings after final rebuild.

## Manual Acceptance Notes

- [x] Papers are original rather than copied from past papers.
- [x] NLU-style scenario framing is used throughout.
- [x] MCQs are narrow and concept-focused, with one best answer.
- [x] Mark schemes provide indicative answer outlines rather than over-specific memorisation answers.
- [x] MCQ explanations clarify the concept being tested, not just the correct letter.
- [x] The set functions as a broad revision compass across the exam guide, rather than a prediction of the real exam.
