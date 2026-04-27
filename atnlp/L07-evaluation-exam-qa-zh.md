# L07 Evaluation 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。重点覆盖 evaluation 的目的、frameworks、perplexity、cloze tasks、standardized benchmarks、benchmark saturation、generation evaluation、LM Arena、ELO、LLM-as-a-Judge 和 contamination。

## 高频核心题

### Q1. 为什么 evaluation 对 NLP 和 LLM 很关键？

A：Evaluation 决定我们如何判断系统是否变好、是否适合部署，以及研究是否真的取得进展。

### Q2. 为什么评价 LLM 比评价传统程序更难？

A：传统程序常有明确正确答案；LLM 输出开放、主观、上下文相关，常常没有唯一 ground truth。

### Q3. evaluation 的三个常见目标是什么？

A：比较两个或多个系统、评估系统改动是否有效、判断系统是否适合某个 use case。

### Q4. 为什么不同目标需要不同 evaluation？

A：因为“哪个系统整体更强”“某个改动是否有效”和“是否适合安全关键应用”衡量的风险和质量维度不同。

### Q5. 评价框架需要考虑哪些维度？

A：答案类型、是否有 ground truth、使用什么 metric、prompt formulation，以及 deployment criteria。

### Q6. 为什么没有 one true evaluation？

A：不同任务和部署场景重视不同能力，例如事实性、推理、安全、用户偏好或成本，单一指标无法覆盖。

### Q7. 为什么要看 individual instances 和 predictions？

A：总体分数可能隐藏错误模式，逐例分析能发现模型具体失败在哪里。

### Q8. 为什么 LLM 的开放式回答难以评价？

A：因为常无单一 ground truth，成功标准主观，人类评价者也可能对 originality、style 或 depth 有分歧。

## Evaluation Frameworks

### Q9. answer type 对评价有什么影响？

A：Multiple choice 容易自动评分；short answer 可用 exact match 或 F1；long/open answer 通常需要人工或 LLM 判断。

### Q10. 有 ground truth 和无 ground truth 的评价有什么区别？

A：有 ground truth 时可用 accuracy、F1 等自动指标；无 ground truth 时通常需要 rubrics、pairwise preference 或 judge。

### Q11. metric 的选择为什么重要？

A：metric 决定模型被奖励什么行为，错误 metric 会推动模型优化错误目标。

### Q12. prompt formulation 为什么会影响评价？

A：同一模型在 zero-shot、few-shot、chain-of-thought 或不同提示词下表现可能不同。

### Q13. deployment criteria 是什么？

A：它指任务的实际风险和使用要求，例如娱乐聊天、医疗建议和自动驾驶的评价标准应完全不同。

### Q14. 为什么 safety-critical decision making 不能只看排行榜？

A：排行榜通常反映平均偏好或 benchmark 表现，不足以评估极端风险、可靠性和责任要求。

### Q15. 为什么 evaluation 要和 use case 对齐？

A：模型在 benchmark 上高分不代表适合实际场景；评价应测量部署中真正重要的能力和风险。

## Intrinsic Evaluation 与 Perplexity

### Q16. 什么是 intrinsic evaluation？

A：Intrinsic evaluation 直接评价模型本身的内部任务或语言建模能力，而不是某个下游应用效果。

### Q17. perplexity 是什么？

A：Perplexity 是评价语言模型的标准指标，可直观理解为模型在每个词位置平均认为有多少个可能选择。

### Q18. perplexity 越低表示什么？

A：表示模型对下一个词越确定，语言建模损失越低。

### Q19. perplexity=1 代表什么？

A：代表模型完全确定每个下一个词。

### Q20. perplexity=10 如何直观理解？

A：可以理解为模型平均觉得每一步大约有 10 个同样可能的词。

### Q21. perplexity 适合用来做什么？

A：适合比较同一数据集上的语言建模质量、跟踪训练进度、检测 train/validation overfitting。

### Q22. 为什么比较 perplexity 必须在同一数据集上？

A：不同数据集难度、分布和 tokenization 可能不同，perplexity 不可直接横向比较。

### Q23. perplexity 的主要局限是什么？

A：它不能可靠衡量事实正确性、推理能力、alignment 或长程连贯性。

### Q24. 为什么低 perplexity 不一定代表更好的模型？

A：模型可以通过记忆常见模式降低 perplexity，但仍可能事实错误、推理弱或不符合用户目标。

### Q25. perplexity 和 memorization 有什么关系？

A：模型可能因为记住训练数据中的常见片段而降低 perplexity，这不等于真正泛化。

### Q26. perplexity 为什么不能很好评价 long-range coherence？

A：它通常逐 token 累积局部预测概率，不能充分检查整段文本的全局结构和一致性。

## Cloze Tasks

### Q27. cloze task 是什么？

A：Cloze task 是给定上下文，要求模型填入缺失词或短语的任务。

### Q28. LAMBADA 测试什么能力？

A：LAMBADA 测试需要较宽 discourse context 才能预测目标词的语言理解能力。

### Q29. perplexity 和 cloze task 的关系是什么？

A：二者测量相近的语言建模能力；perplexity 是连续概率评价，cloze 是离散对错评价。

### Q30. 为什么 cloze task 比普通下一个词预测更有挑战？

A：因为正确词常依赖较长上下文和篇章级线索，而不是局部 n-gram。

### Q31. cloze task 的局限是什么？

A：它仍偏语言建模，不一定评价事实性、复杂推理、工具使用或安全性。

## Task-based Benchmarks

### Q32. 什么是 task-based benchmark？

A：它用一组标准化任务评价模型在不同能力维度上的表现，例如知识、代码、数学、推理和多模态。

### Q33. 为什么 foundation models 需要 multidimensional evaluation？

A：因为一个模型可能数学强但事实性弱，或代码强但安全性差，单一任务不能代表整体能力。

### Q34. Gemma 3 技术报告中的 benchmark 表格说明什么？

A：现代模型评估通常跨多个任务报告结果，如 MMLU-Pro、LiveCodeBench、Bird-SQL、GPQA、SimpleQA、MATH、MMMU 等。

### Q35. 为什么 benchmark 表格不能只看平均分？

A：平均分会掩盖能力差异，不同任务对部署风险和用户需求的重要性也不同。

### Q36. MMLU 是什么？

A：MMLU 是 Massive Multitask Language Understanding，覆盖多个学科的多选题 benchmark。

### Q37. MMLU 主要测什么？

A：主要测世界知识、问题解决能力，以及跨 57 个 subjects 的多任务理解能力。

### Q38. MMLU 为什么被广泛使用？

A：它覆盖面广、格式标准化、易自动评分，曾是通用 LLM 能力的重要指标。

### Q39. MMLU 的局限是什么？

A：它可能出现 saturation、数据污染、题目噪声，并且多选题不能覆盖开放式创造和真实交互能力。

### Q40. MMLU-Pro 相比 MMLU 有什么变化？

A：MMLU-Pro 有 10 个选项而不是 4 个，更难，更多 college-level reasoning，且经过专家复核减少噪声。

### Q41. MMLU-Pro 为什么更稳定？

A：讲义提到它在不同 prompt styles 下分数波动更小，prompt sensitivity 从约 4-5% 降到约 2%。

### Q42. MMLU-Pro 为什么更适合测试 reasoning？

A：它的题目更复杂，直接回答更难，Chain-of-Thought 在其上更有帮助。

### Q43. GPQA 是什么？

A：GPQA 是 graduate-level Google-proof Q&A benchmark，面向高难度专业问题。

### Q44. GPQA Diamond 的特点是什么？

A：题目由专家编写和验证，非专家即使用网络搜索也很难答对，旨在测试真正专业知识和推理。

### Q45. GPQA 中为什么要让非专家验证？

A：为了确认题目不是简单搜索可解，从而更接近 Google-proof。

### Q46. GPQA 为什么比普通知识题更难？

A：它需要专业背景和细致推理，而不是简单记忆或检索。

### Q47. DeepSeek-R1 图表在评价讲义中说明什么？

A：它展示模型在推理 benchmark 上的表现，用于说明 standardized tasks 能比较特定能力。

### Q48. 为什么 benchmark 需要不断更新？

A：模型进步会导致旧 benchmark saturation，分数接近人类或满分后难以区分模型。

## Saturation 与 Humanity's Last Exam

### Q49. benchmark saturation 是什么？

A：当模型在 benchmark 上接近满分或人类水平，benchmark 不再能有效区分模型能力时，就发生 saturation。

### Q50. saturation 有什么问题？

A：它会让排行榜失去诊断价值，模型之间看似都很好，但真实能力差异可能被掩盖。

### Q51. AI Index 图中的 saturation 说明什么？

A：许多经典 benchmark 上模型已经接近或超过人类 baseline，说明需要更难、更真实的评价。

### Q52. Humanity's Last Exam 试图解决什么问题？

A：它试图通过更难的专家级、闭合式、自动可评分题目缓解 benchmark saturation。

### Q53. HLE 使用了哪些反 saturation 设计？

A：使用人类知识前沿的 subject-matter experts、multiple choice/short answer 自动评分、更难的 multimodal questions，以及多阶段题目过滤。

### Q54. HLE 的局限是什么？

A：它主要测试结构化学术问题，不适合评价开放式、创造性或现实交互式问题解决能力。

### Q55. 为什么 HLE 也可能 eventually saturate？

A：只要 benchmark 固定并公开，模型进步和训练数据污染最终都可能让它失去区分度。

### Q56. 为什么“最后一次考试”这个说法要谨慎？

A：因为任何固定 benchmark 都可能被过拟合或污染，而且模型能力不只体现在学术题上。

## Open-ended Generation Evaluation

### Q57. 为什么开放式生成问题难评价？

A：因为没有唯一正确答案，评价标准主观，甚至人类也会对质量有分歧。

### Q58. “写一首避免陈词滥调的悲伤短诗”为什么难评分？

A：因为 originality、emotional depth 和 cliché avoidance 都是主观维度，没有简单 ground truth。

### Q59. 哪些任务可能有参考答案但仍难评价？

A：翻译和摘要可能有参考答案，但仍有多个合理输出，自动指标不一定认可所有好答案。

### Q60. 为什么真实 LLM 使用和 benchmark 任务不同？

A：真实用户请求常是开放式、长上下文、多目标和交互式，而 benchmark 多是固定题目和标准答案。

### Q61. Clio/Claude.ai 使用分析说明什么？

A：真实 LLM 使用包含大量写作、代码、分析、创意和个人化任务，不只是 MCQ 或 accuracy-style benchmark。

### Q62. 为什么真实使用数据对 evaluation 有价值？

A：它能告诉我们用户真正如何使用模型，从而设计更贴近部署场景的评价。

## LM Arena 与 ELO

### Q63. LM Arena 是什么？

A：LM Arena 是一个开放、众包的大模型评价平台，用户比较两个匿名模型的回答并选择更好者。

### Q64. LM Arena 的基本流程是什么？

A：用户看到两个匿名模型并排回答同一 prompt，选择 A 更好、B 更好或 tie。

### Q65. LM Arena 主要评价什么？

A：它主要评价真实用户对开放式回答的偏好和 instruction-following 体验。

### Q66. LM Arena 为什么比固定 test set 更贴近真实使用？

A：因为 prompts 来自真实用户交互，模型回答开放且用户直接表达偏好。

### Q67. LM Arena 的结果如何聚合？

A：通常使用 Elo-style rating system 汇总 pairwise comparison 结果。

### Q68. LM Arena 的优点是什么？

A：它适合 general-purpose alignment evaluation，能持续加入新模型，并捕捉用户偏好。

### Q69. LM Arena 的局限是什么？

A：它昂贵、依赖用户群体偏好，不适合 safety-critical decision making，也可能偏好表面流畅。

### Q70. Elo rating 中所有模型初始分数通常如何设置？

A：所有模型从相同 rating 开始，例如 1000。

### Q71. Elo 中 `O_A` 表示什么？

A：表示模型 A 的比赛结果，赢为 1，平为 0.5，输为 0。

### Q72. Elo 中 `E_A` 表示什么？

A：表示根据当前 rating 预测模型 A 的 expected win probability。

### Q73. Elo 更新公式的直观含义是什么？

A：实际结果比预期好则升分，比预期差则降分，变化大小由 `K` 和意外程度决定。

### Q74. 为什么击败强对手涨分更多？

A：因为预期胜率低却实际获胜，说明模型表现显著超出预期。

### Q75. 为什么输给弱对手掉分更多？

A：因为预期本应获胜却失败，说明当前 rating 可能过高。

### Q76. Elo-style 评价和 accuracy 评价的区别是什么？

A：Elo 基于 pairwise preferences，适合开放式生成；accuracy 基于标准答案，适合封闭题。

## LLM-as-a-Judge

### Q77. 什么是 LLM-as-a-Judge？

A：LLM-as-a-Judge 是让强 LLM 对其他模型输出进行评分、排序或解释性评价。

### Q78. LLM-as-a-Judge 的两个主要优点是什么？

A：Scalability 和 explainability，即比人工更可扩展，并能给出评价理由。

### Q79. 为什么 LLM-as-a-Judge 能降低评价成本？

A：它可以自动评估大量开放式回答，减少人工逐条标注需求。

### Q80. LLM-as-a-Judge 为什么有 explainability？

A：judge model 可以输出选择某个回答的理由，帮助分析模型优缺点。

### Q81. Zheng et al. (2023) 对 LLM-as-a-Judge 有什么贡献？

A：他们展示 LLM 判断与人类判断有较强相关性，并提出 pairwise comparison、prompt design 和 calibration 等实践。

### Q82. LLM-as-a-Judge 为什么要 randomize answer order？

A：因为 judge model 可能偏向第一个答案，随机化顺序可以降低位置偏差。

### Q83. LLM-as-a-Judge 可能偏向什么样的答案？

A：可能偏向更流畅、更长、更自信或风格更像自己的答案，即使事实不一定正确。

### Q84. 什么是 self-referential bias？

A：judge model 更偏好与自己风格或家族相似的模型输出。

### Q85. LLM-as-a-Judge 的 prompt sensitivity 是什么？

A：评价结果可能因评判提示词的细微变化而改变，说明鲁棒性不足。

### Q86. 为什么 LLM judge 可能误判事实性？

A：因为它也可能被流畅表达迷惑，或者对问题本身知识不足却给出自信判断。

### Q87. LLM-as-a-Judge 适合什么场景？

A：适合大规模初筛、开放式回答比较和辅助分析，不适合单独承担高风险最终决策。

### Q88. LLM-as-a-Judge 和 human evaluation 的关系是什么？

A：LLM judge 可以降低人工成本，但仍应通过人类评价校准和验证，尤其在高风险场景。

## Contamination

### Q89. 什么是 benchmark contamination？

A：测试集或其近似内容进入训练数据，导致模型在评价时可能靠记忆而不是泛化。

### Q90. 为什么 foundation model 更容易发生 contamination？

A：因为它们在互联网规模数据上训练，而许多 benchmark 题目也公开在互联网。

### Q91. contamination 为什么会破坏评价？

A：它把 evaluation 变成 memorization 测试，夸大模型真实能力，误导研究进展。

### Q92. pre-foundation model 时代为什么 contamination 较容易控制？

A：传统 supervised setting 中 train/test split 通常清晰定义，数据范围较可控。

### Q93. 解决 contamination 的方法 1 是什么？

A：模型提供者报告 train-test overlap，但这需要信任自报，且 fuzzy overlap 很难检测。

### Q94. 解决 contamination 的方法 2 是什么？

A：使用动态和私有 held-out test sets，减少训练数据提前包含测试题的概率。

### Q95. 解决 contamination 的方法 3 是什么？

A：从模型行为推断 train-test overlap，例如比较 public/private benchmark 表现、测试记忆能力或利用数据点 exchangeability。

### Q96. 如何用 public vs. private benchmark 检测 contamination？

A：如果模型在公开版本异常高、私有版本明显下降，可能存在对公开题的记忆或过拟合。

### Q97. prefix completion 如何检测 memorization？

A：给模型测试题前缀，看它是否能补全原题或答案，若能则可能记住了测试数据。

### Q98. contamination 和 saturation 有什么关系？

A：公开 benchmark 不断被训练和调优会加速 saturation，使分数越来越难代表真实泛化。

## 综合比较题

### Q99. Intrinsic evaluation 和 task-based evaluation 的区别是什么？

A：Intrinsic evaluation 直接测语言建模等内部能力；task-based evaluation 测模型在具体任务或 benchmark 上的表现。

### Q100. Perplexity 和 accuracy 的区别是什么？

A：Perplexity 是概率式语言建模指标；accuracy 是离散任务上预测是否正确的比例。

### Q101. MMLU 和 MMLU-Pro 的区别是什么？

A：MMLU-Pro 更难，选项更多，题目经专家复核，prompt sensitivity 更低，更强调复杂推理。

### Q102. GPQA 和 MMLU 的区别是什么？

A：MMLU 覆盖广泛学科，多为标准知识多选题；GPQA 是专家级、Google-proof 的高难专业问题。

### Q103. HLE 和 LM Arena 的评价目标有什么不同？

A：HLE 测结构化、专家级、可自动评分的学术能力；LM Arena 测真实用户对开放式回答的偏好。

### Q104. LLM-as-a-Judge 和 LM Arena 的区别是什么？

A：LLM-as-a-Judge 用模型自动评价；LM Arena 用人类用户 pairwise preference 聚合评价。

### Q105. 为什么 benchmark 分数和 real-world usefulness 可能不一致？

A：benchmark 可能固定、窄、被污染或已饱和，而真实使用需要开放式交互、安全、成本和可靠性。

### Q106. 为什么 open-ended generation evaluation 更依赖偏好评价？

A：因为没有唯一标准答案，pairwise preference 或 rubric-based judgement 更能处理多种合理输出。

### Q107. 为什么 evaluation 应该是 multi-dimensional？

A：模型质量涉及 helpfulness、safety、reasoning、factuality、style 和 cost，不可能由一个分数完整表示。

### Q108. 为什么“看单个 leaderboard”是不够的？

A：leaderboard 只反映特定任务和指标，可能忽略安全、部署约束、成本和具体失败模式。

## 公式与术语速记

### Q109. Elo 中 A 的 expected win probability 公式直观表达什么？

A：它根据 A 和 B 的当前 rating 估计 A 应该赢的概率，rating 越高，预期胜率越高。

### Q110. Elo 更新公式 `R_A' = R_A + K(O_A - E_A)` 如何理解？

A：`O_A - E_A` 是实际结果与预期结果的差，差值越大，rating 改变越大。

### Q111. `K` 在 Elo 中表示什么？

A：`K` 是更新率，控制每场比较对 rating 的影响大小。

### Q112. perplexity 高代表什么？

A：代表模型对下一个词更不确定，语言建模困惑度更高。

### Q113. closed-ended benchmark 是什么？

A：有明确选项或短答案、可自动评分的 benchmark，例如 MCQ 或 short answer。

### Q114. open-ended evaluation 是什么？

A：评价没有唯一答案的生成任务，通常需要人类或 LLM 根据 rubric 或偏好判断。

### Q115. dynamic benchmark 是什么？

A：随时间更新或保持私有的测试集，用来降低 saturation 和 contamination 风险。

## 考前作答模板

### Q116. 如果考“为什么评价 LLM 很难”，应该怎么答？

A：从开放式输出、无唯一 ground truth、主观标准、多维能力、prompt sensitivity 和 real-world deployment 差异来回答。

### Q117. 如果考“perplexity 的优缺点”，应该怎么答？

A：优点是标准、便宜、适合追踪语言建模；缺点是不能评价事实、推理、alignment 和开放式质量。

### Q118. 如果考“benchmark saturation”，应该怎么答？

A：定义为模型接近满分或人类水平导致 benchmark 失去区分力；解决可用更难题、动态私有测试和多维评价。

### Q119. 如果考“LM Arena 如何工作”，应该怎么答？

A：用户比较两个匿名模型回答，选择更好或平局，结果用 Elo-style rating 聚合，适合开放式用户偏好评价。

### Q120. 如果考“LLM-as-a-Judge 的利弊”，应该怎么答？

A：优点是可扩展、能解释、成本低；缺点是 prompt sensitive、位置偏差、自我偏好、可能偏向流畅但错误的答案。

### Q121. 如果考“contamination 为什么危险”，应该怎么答？

A：它让测试集进入训练，模型可能靠记忆得高分，导致评价不再衡量真实泛化和推理能力。

### Q122. 如果考“如何选择 evaluation”，应该怎么答？

A：先确定 use case 和风险，再选答案类型、ground truth 设置、metric、prompt formulation，并结合逐例分析。

