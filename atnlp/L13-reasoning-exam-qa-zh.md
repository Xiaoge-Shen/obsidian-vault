# L13 Reasoning in LLMs 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。根据 `examguide.pdf`，Lecture 13 的 slides 25-29 不考；本文件聚焦可考核心：reasoning 定义、inference-time reasoning、self-consistency、tree of thoughts、self-reflection、feedback 质量，以及 external verifiers 的基本思想，不展开被排除的 Lean mini-course 细节。

## 高频核心题

### Q1. 讲义中如何理解 reasoning？

A：Reasoning 是使用事实或假设进行结构化思考，以得出结论、做决策或解释为什么某事成立。

### Q2. 本讲关注的 reasoning 是什么？

A：关注使用文本形式的 reasoning，例如 chain-of-thought，来解决人类通常需要推理的问题。

### Q3. LLM reasoning 可以从哪两个阶段改进？

A：可以从 training 阶段和 inference/decoding 阶段改进。

### Q4. 本讲主要关注哪类方法？

A：主要关注 inference-time 或 decoding-time 方法，因为概念简单且预算较低时更容易使用。

### Q5. inference-time reasoning 方法有哪些？

A：包括 chain-of-thought、self-consistency、tree-of-thoughts、self-reflection 和 external solvers/verifiers。

### Q6. 为什么 reasoning 方法是 moving target？

A：因为该领域发展很快，新方法和经验发现不断出现。

### Q7. 本讲选择方法的标准是什么？

A：概念和实现相对简单，并且主要作用在 decoding/inference 阶段。

### Q8. 为什么 inference-time 方法有吸引力？

A：它们不需要重新训练模型，能通过采样、搜索、反馈或工具调用提升推理表现。

## Chain-of-Thought 与 Self-Consistency

### Q9. Chain-of-Thought 是什么？

A：CoT 是让模型逐步生成中间推理过程，再给出答案。

### Q10. CoT 的基本局限是什么？

A：CoT 通常是线性的，一旦走错路径，模型可能继续沿错误方向推理。

### Q11. Self-consistency 的核心思想是什么？

A：同一问题可以有多条推理路径；如果多条路径导向同一答案，就选择出现最多的答案。

### Q12. Self-consistency 如何操作？

A：采样多个 reasoning chains `r1,...,rn` 和对应答案 `a1,...,an`，选择出现次数最多的答案。

### Q13. Self-consistency 为什么可能提升推理？

A：它用多样化采样降低单条 CoT 偶然错误的影响，相当于对 reasoning paths 做投票。

### Q14. Self-consistency 中如何控制答案多样性？

A：最简单的方法是调整 decoding temperature。

### Q15. temperature 在 self-consistency 中的作用是什么？

A：较高 temperature 增加采样多样性，可能探索更多推理路径；太高也会增加噪声。

### Q16. Self-consistency 的优点是什么？

A：简单、通用、不需要训练，常能提升数学和推理任务表现。

### Q17. Self-consistency 的代价是什么？

A：需要多次采样，推理成本随采样条数增加。

### Q18. Self-consistency 的一个风险是什么？

A：如果多数路径共享同一错误偏差，投票仍会选错。

### Q19. Self-consistency 和普通 CoT 的区别是什么？

A：普通 CoT 只生成一条推理链；self-consistency 生成多条并对最终答案投票。

### Q20. Self-consistency 适合什么任务？

A：适合答案可聚合或可投票的任务，例如数学题、常识推理和多选题。

## Tree of Thoughts

### Q21. Tree of Thoughts 是什么？

A：ToT 让模型探索多个 branching reasoning paths，类似搜索算法，而不是只走一条线性 CoT。

### Q22. ToT 为什么比 CoT 更接近真实 problem solving？

A：真实解决问题常需要尝试、评估、回退和探索多个候选路径。

### Q23. ToT 的核心优势是什么？

A：允许 deliberate decision making、self-evaluation 和 backtracking。

### Q24. ToT 需要定义哪些组件？

A：thought decomposition、thought generator、state evaluator 和 search algorithm。

### Q25. thought decomposition 是什么？

A：定义什么算一个 thought；它要足够小以产生多样性，又足够大以评估进展。

### Q26. thought generator 的作用是什么？

A：给定当前状态，生成下一步可能的 thought。

### Q27. thought generator 可以如何实现？

A：可以从 LLM 采样，或使用 propose prompt 让模型提出候选 thought。

### Q28. state evaluator 的作用是什么？

A：评估 frontier states 离解决问题有多近，类似搜索中的 heuristic evaluation。

### Q29. state evaluator 可以如何实现？

A：可以用 prompt 给状态打分，或让模型在多个候选状态中投票选最好。

### Q30. ToT 中常用什么 search algorithm？

A：可以使用 BFS 或 DFS。

### Q31. BFS 和 DFS 在 ToT 中有什么区别？

A：BFS 同层扩展多个候选，覆盖更广；DFS 深入单一路径，可能更省内存但更容易走偏。

### Q32. ToT 为什么 problem-specific？

A：不同任务需要不同 thought 粒度、生成方式、评价标准和搜索策略。

### Q33. ToT 的代价是什么？

A：比普通 CoT 更贵，因为需要生成和评价多个中间状态。

### Q34. Game of 24 在讲义中用于说明什么？

A：用于说明 ToT 如何通过搜索不同算术组合来解决需要探索路径的问题。

### Q35. ToT 和 self-consistency 的区别是什么？

A：Self-consistency 独立采样多条完整 reasoning chains；ToT 显式构造树并逐步搜索和评价中间状态。

### Q36. ToT 适合什么类型任务？

A：适合需要规划、搜索、回溯或局部选择的任务。

## Self-Reflection 与 Self-Refinement

### Q37. Self-reflection 的核心思想是什么？

A：让模型对自己的输出给出反馈，并根据反馈逐步修改答案。

### Q38. Self-refinement 是什么？

A：Self-refinement 是模型生成初始答案后，生成反馈并迭代改进答案的过程。

### Q39. Self-reflection 为什么可看作 reasoning？

A：因为模型显式评估当前答案、发现问题并提出修正步骤。

### Q40. Self-reflection 的基本流程是什么？

A：生成初始输出，生成反馈，根据反馈改写输出，可重复多轮。

### Q41. Self-reflection 什么时候停止？

A：可以在固定步数 `t` 停止，也可以从反馈中提取 stop score 或 stopping indicator。

### Q42. Self-reflection 的优点是什么？

A：不需要外部标注即可改进部分输出，尤其适合写作、代码和开放式任务。

### Q43. Self-reflection 的风险是什么？

A：模型可能无法发现自己的错误，或生成看似合理但无效的反馈。

### Q44. Self-reflection 是否总能提升结果？

A：不一定。如果反馈不准确或不具体，反复修改可能让答案变差。

### Q45. Self-reflection 和 self-consistency 的区别是什么？

A：Self-consistency 对多个答案投票；self-reflection 对一个答案进行反馈和迭代修改。

### Q46. Self-reflection 和 ToT 的区别是什么？

A：ToT 搜索多个状态路径；self-reflection 更像对当前输出进行批改和修订。

## Feedback 质量

### Q47. Madaan et al. 强调什么样的 feedback？

A：Feedback 应该 actionable 和 specific。

### Q48. actionable feedback 是什么？

A：能指出具体改进动作的反馈，例如“避免在 for loop 中重复计算”。

### Q49. specific feedback 是什么？

A：能定位具体需要修改的短语、代码片段或推理步骤。

### Q50. “Improve the efficiency of the code” 为什么不是好 feedback？

A：它太笼统，没有说明具体哪里低效或如何改。

### Q51. “Avoid repeated calculations in the for loop” 为什么是好 feedback？

A：它具体指出问题位置和可执行修改方向。

### Q52. 为什么 feedback prompt 很重要？

A：模型需要被提示生成具体、可操作的反馈，否则可能只给空泛评价。

### Q53. Feedback improvements 的核心目标是什么？

A：让 feedback 更准确、更具体、更能指导下一轮输出改进。

### Q54. 为什么 feedback 对代码 reasoning 特别有用？

A：代码有较清晰的错误、效率和可执行性标准，反馈可以直接指导修复。

## External Verifiers/Solvers

### Q55. external verifiers/solvers 的核心思想是什么？

A：使用外部工具验证或解决问题，而不是完全依赖 LLM 自己生成推理。

### Q56. external solver 为什么仍属于 inference-time 方法？

A：通常在解码或回答过程中调用工具，不需要重新训练模型。

### Q57. 使用 external solvers 的优点是什么？

A：可以引入符号验证、计算器、 theorem prover 等可靠工具，减少纯文本推理错误。

### Q58. 使用 external solvers 的代价是什么？

A：需要把自然语言问题转成工具可处理形式，并处理工具反馈和接口错误。

### Q59. external verifier 的典型例子是什么？

A：形式化证明器、符号求解器、代码执行器或其他可验证工具都可作为外部 verifier/solver。

### Q60. 形式化证明器在 reasoning 中的作用是什么？

A：它可以检查模型给出的形式化步骤是否符合逻辑规则，从而提供比模型自评更强的正确性信号。

### Q61. 为什么不应把 external verifier 理解成“模型自动变聪明”？

A：verifier 只是提供检查或求解接口；模型仍需要把问题正确表示出来，并把工具反馈转化成下一步推理。

### Q62. 使用形式化 verifier 的主要难点是什么？

A：自然语言到形式语言的转换困难，搜索空间大，工具接口复杂，而且错误 formalisation 会让验证结果失去意义。

### Q63. 为什么 external verifier 能突破 LLM-only modelling？

A：因为推理正确性部分交给外部系统检查，而不是只依赖模型参数和文本生成。

## 综合比较题

### Q64. CoT、self-consistency、ToT 的关系是什么？

A：CoT 是单条线性推理；self-consistency 是多条 CoT 投票；ToT 是显式树状搜索和状态评价。

### Q65. Self-reflection 和 external verifier 的区别是什么？

A：Self-reflection 由模型自己评价自己；external verifier 使用外部工具提供更独立的检查。

### Q66. Inference-time reasoning 和 training-time reasoning 的区别是什么？

A：Inference-time 方法不改参数，通过解码策略、搜索、反馈或工具提升表现；training-time 方法通过数据和目标函数改变模型能力。

### Q67. 为什么 inference-time reasoning 可能更适合 limited-budget settings？

A：它不需要大规模训练或模型更新，只增加推理时计算和提示设计。

### Q68. 为什么多数投票不等于证明正确？

A：多个 sampled chains 可能共享模型偏差，重复答案不保证逻辑有效。

### Q69. 为什么 ToT 更接近搜索算法？

A：它维护状态、扩展候选 thought、评价 frontier，并用 BFS/DFS 等策略探索空间。

### Q70. 为什么 self-reflection 需要高质量 feedback？

A：反馈如果不具体或错误，就无法指导有效修改，甚至会把输出改坏。

## 公式与术语速记

### Q71. `r1,...,rn` 在 self-consistency 中表示什么？

A：表示采样得到的多个 reasoning chains。

### Q72. `a1,...,an` 在 self-consistency 中表示什么？

A：表示每条 reasoning chain 得到的最终答案。

### Q73. ToT 中 frontier states 是什么？

A：当前搜索边界上的候选中间状态。

### Q74. stop score 是什么？

A：用于判断 self-refinement 是否应停止的标量或停止信号。

### Q75. value prompt 在 ToT 中做什么？

A：让模型评价某个状态离解决问题有多近。

### Q76. vote prompt 在 ToT 中做什么？

A：让模型在多个候选状态中选出较好的一个。

## 考前作答模板

### Q77. 如果考“self-consistency”，应该怎么答？

A：说明采样多条 CoT reasoning chains，对最终答案投票；优点是简单有效，缺点是成本高且多数可能仍错。

### Q78. 如果考“Tree of Thoughts”，应该怎么答？

A：列出 thought decomposition、generator、evaluator、search algorithm，并说明它允许搜索、评价和回溯。

### Q79. 如果考“self-reflection”，应该怎么答？

A：说明模型先生成答案，再生成 actionable/specific feedback，随后迭代修改，直到固定步数或 stop signal。

### Q80. 如果考“feedback 为什么重要”，应该怎么答？

A：强调反馈必须具体可执行；笼统反馈不能指导改进，具体反馈能定位问题并给出修复方向。

### Q81. 如果考“external verifiers”，应该怎么答？

A：说明它们在推理时调用外部工具验证或求解，能提高可靠性，但需要正确 formalisation 和工具接口。
