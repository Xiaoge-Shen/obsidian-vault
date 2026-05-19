# Mock Exam Coverage and Difficulty

## Overview

The three mock papers are designed as a revision compass rather than predictions. Together they cover the major examinable areas in `examguide.pdf`, while each individual paper has a different centre of gravity.

Difficulty scale:

- Easy: mostly definition, recognition, or direct application.
- Medium: requires comparison, diagnosis, or system design.
- Hard: requires synthesis across topics, tradeoff analysis, or deployment judgement.

## Mock Paper 1

Theme: Retrieval, Question Answering, Data, Translation, and Evaluation

### Knowledge Coverage

Main areas:

- Machine translation
- Low-resource MT
- Parallel data extraction from web crawls
- Backtranslation and monolingual data use
- Data selection: quality, diversity, importance
- BPE and multilingual transfer
- QA and open-domain QA
- RAG and retriever-reader systems
- Sparse vs dense retrieval
- Multilingual QA and cross-lingual retrieval
- Evaluation: BLEU, chrF, COMET, retrieval metrics, answer metrics
- Benchmark contamination and benchmark saturation
- Ethics of multilingual and health-related deployment
- WEAT and bias measurement
- Natural Questions, Fusion-in-Decoder, LM Arena

### Exam Form

- Q1: Scenario-based low-resource MT and data pipeline design.
- Q2: Scenario-based multilingual RAG/open-domain QA design.
- Q3: 10 multiple-choice questions, each testing a narrow concept.

### Difficulty

Overall: Medium

Why:

- Q1 is medium-hard because it requires combining low-resource MT, web data extraction, synthetic data, BPE, and evaluation.
- Q2 is medium-hard because it requires RAG design plus multilingual ethical judgement.
- Q3 is easy-medium if definitions are solid, but it probes many isolated concepts quickly.

Best for practising:

- Translation/data/RAG integrated answers.
- Explaining tradeoffs concisely.
- Spotting evaluation pitfalls.

## Mock Paper 2

Theme: Adaptation, Reasoning, Long Context, Summarisation, and Safety

### Knowledge Coverage

Main areas:

- Summarisation: extractive vs abstractive, legal faithfulness
- ROUGE and pointer-generator networks
- Instruction fine-tuning
- RLHF and reward optimisation
- Coursework-related reasoning and verifiable rewards
- PEFT and LoRA equations
- Full fine-tuning vs parameter-efficient adaptation
- Knowledge distillation
- Long-context modelling
- Sinusoidal positional encoding
- RoPE
- Positional interpolation and YaRN
- Efficient attention
- Long-context evaluation and lost-in-the-middle style failures
- Chain-of-thought and self-consistency
- Shapley value intuition for reasoning-step contribution
- BPE-dropout
- Safety-relevant deployment evaluation

### Exam Form

- Q1: Scenario-based adaptation/design question for legal summarisation.
- Q2: Technical and conceptual long-context/reasoning question.
- Q3: 10 multiple-choice questions on summarisation, PEFT, long context, reasoning, distillation, and RLHF.

### Difficulty

Overall: Medium-hard

Why:

- Q1 requires comparing multiple adaptation strategies and knowing when automatic metrics are insufficient.
- Q2 is hard because it combines positional encoding mechanics, computational complexity, long-context evaluation, and decoding-time reasoning.
- Q3 includes formula-facing concepts such as LoRA, Shapley values, RoPE, and BPE-dropout.

Best for practising:

- Technical explanations with equations.
- Differentiating long context, RAG, and reasoning.
- Answering adaptation questions without writing too much.

## Mock Paper 3

Theme: Multimodal Models, Agents, Interpretability, Parsing, and Ethics

### Knowledge Coverage

Main areas:

- Perceptually grounded NLP
- Symbol grounding problem
- Visual grounding
- Vision-language model architecture
- Vision encoder, LLM backbone, connector/projector
- Connector architecture tradeoffs
- VLM training data
- Intrinsic vs extrinsic VLM evaluation
- Accessibility and ethical deployment of VLMs
- AI agent definitions
- Text-only tool agents vs vision-based computer-use agents
- Computer-use agent trajectories
- Action representation, trajectory history, and termination
- Agent reliability, safety, and evaluation beyond success rate
- Watermarking
- Sparse autoencoders
- Superposition and mechanistic interpretability
- Dependency vs constituency parsing
- Linearised tree validity
- Allocational vs representational harm
- Finite-state machines and formal-language limitations

### Exam Form

- Q1: Scenario-based VLM system design.
- Q2: Scenario-based AI agent/computer-use agent design and evaluation.
- Q3: 10 multiple-choice questions across VLMs, agents, safety, interpretability, parsing, ethics, and automata.

### Difficulty

Overall: Medium-hard

Why:

- Q1 is medium because it asks for architecture and evaluation, but the connector tradeoff requires precise explanation.
- Q2 is hard because agents require integrating perception, action, memory/history, action-space risk, and evaluation.
- Q3 is broad: each MCQ is short, but the concepts are spread across late-course material.

Best for practising:

- Later-course material.
- System-design questions involving VLMs and agents.
- Reliability and safety reasoning.

## Relative Difficulty Ranking

From easiest to hardest:

1. Mock Paper 1: broad but closest to the NLU past-paper themes.
2. Mock Paper 3: broad and later-course heavy; harder if VLM/agent material is weak.
3. Mock Paper 2: most technical; requires equations, long-context mechanics, and adaptation tradeoffs.

## Suggested Practice Order

Recommended order:

1. Paper 1 first, to warm up on familiar NLU-style topics: MT, QA, RAG, data, evaluation.
2. Paper 2 second, under timed conditions, because it is the most technical.
3. Paper 3 last, to test whether late-course topics are connected well enough for scenario answers.

After each attempt:

- Mark Q1/Q2 with the mark scheme first.
- Then check MCQs.
- Finally, write a 5-line reflection: which topics caused hesitation, which answers were too long, and which definitions need revision.

