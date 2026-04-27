# L28 Agents Lecture 1 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。根据 `examguide.pdf`，Lecture 28 的 slides 27-34 不考；本文件聚焦可考核心：AI agent 定义、LLM-based agent anatomy、agentic problem-solving process、agent 设计 pitfalls、single-call execution、prompt chaining、routing、parallelisation、tool use、MCP、agentic frameworks 与总结。

## 高频核心题

### Q1. Russell 和 Norvig 如何定义 agent？

A：Agent 是能通过 sensors 感知环境，并通过 actuators 作用于环境的东西。

### Q2. 为什么现代 AI agent 仍可回到传统定义？

A：无论 LLM agent、tool agent 还是 multimodal agent，本质仍是感知、决策和行动。

### Q3. McKinsey/Gov.UK 这类定义强调什么？

A：强调 agent 可代表用户或系统执行任务、协调流程、做决策，并能合作或独立达成目标。

### Q4. LLM-based AI agent 的核心组成是什么？

A：LLM、输入输出文本表示、wrapper/framework、可选工具、记忆和工作流控制。

### Q5. LLM-based agent 的主要优势是什么？

A：利用预训练知识，并用自然语言表示输入、目标、计划和输出。

### Q6. 为什么 agent 话题容易有 hype？

A：应用空间很大，但定义混乱，很多系统只是包装 LLM，需要清楚认识其 strengths and weaknesses。

### Q7. Agentic AI problem-solving process 包括哪些步骤？

A：Get the Mission、Scan the Scene、Think it Through、Take Action、Learn & Get Better。

### Q8. 为什么 agents 需要课程中多个主题的知识？

A：agent 结合 LLM、prompting、RAG、long-context modelling 和 multimodal modelling。

## Agent Anatomy 与 Pitfalls

### Q9. Get the Mission 是什么？

A：用户用自然语言指定目标和约束。

### Q10. Scan the Scene 是什么？

A：agent 自主确定需要哪些信息来源，例如检索文档或调用 RAG。

### Q11. Think it Through 是什么？

A：agent 使用推理步骤制定计划。

### Q12. Take Action 是什么？

A：agent 生成可执行动作，使系统更接近目标。

### Q13. Learn & Get Better 是什么？

A：理想情况下，agent 从环境反馈和交互经验中改进行为。

### Q14. “Framework != LLM” 是什么意思？

A：用户通常与包装 LLM 的 agent framework 交互，而不是直接与裸 LLM 交互。

### Q15. Modality gap 是什么？

A：非文本数据必须转成 LLM 可用的表示，纯文本 LLM 无法直接处理原始图像、声音或 GUI。

### Q16. Context length limit 对 agent 有什么影响？

A：长任务历史、工具结果和文档会占用上下文，导致遗漏、截断或成本增加。

### Q17. Lack of memory 是什么问题？

A：LLM 本身没有真正长期记忆，长期状态通常需要 RAG、数据库或外部 memory 模拟。

### Q18. Action space 是什么？

A：agent 可选择和执行的动作集合，例如调用工具、点击按钮、运行代码或发送消息。

### Q19. 为什么 action space 设计很关键？

A：动作集合决定 agent 能做什么，也影响安全、可靠性和错误恢复。

### Q20. 为什么设计 patterns 对 agents 重要？

A：agent 行为灵活但复杂，patterns 提供可复用蓝图，提升结构、可靠性、效率和可维护性。

## Single-call 与 Prompt Chaining

### Q21. Single-call execution 是什么？

A：把任务写成一个 prompt，调用一次 LLM 得到输出。

### Q22. Single-call execution 适合什么任务？

A：简单、一阶段、容易测试的任务。

### Q23. Single-call execution 的局限是什么？

A：复杂多步任务难以一次完成，难调试、难约束、容易遗漏中间步骤。

### Q24. Prompt chaining 是什么？

A：把复杂任务分解成一系列较小 prompts，前一步输出作为后一步输入。

### Q25. Prompt chaining 的核心思想是什么？

A：divide-and-conquer，将大任务拆为可管理的子问题。

### Q26. Prompt chaining 的例子是什么？

A：先总结市场报告，再抽取趋势和数据点，最后写给营销团队的邮件。

### Q27. Prompt chaining 的优点是什么？

A：模块化、清晰、易调试、通常更可靠。

### Q28. Prompt chaining 的缺点是什么？

A：低效、上下文跟踪困难、错误传播，并要求模块输入输出格式严格匹配。

### Q29. Error propagation 在 chaining 中是什么意思？

A：早期步骤的错误会作为输入传给后续步骤，导致最终输出继续偏离。

## Routing

### Q30. Routing pattern 是什么？

A：引入条件逻辑，根据用户意图、约束或输入类型选择合适流程或组件。

### Q31. Routing 相比 prompt chaining 的改进是什么？

A：不再固定顺序执行，而是能根据情况动态选择路径。

### Q32. Routing 如何帮助 safety？

A：可加入 safety router，检测危险请求或约束违规，阻止执行 unsafe behaviours。

### Q33. Rule-based routing 是什么？

A：用关键词、模式或结构化字段等预定义规则决定 route。

### Q34. ML model-based routing 是什么？

A：用训练好的分类器做 intent classification。

### Q35. Embeddings-based routing 是什么？

A：把输入转成向量，与各 route/capability 的 embedding 比较相似度。

### Q36. LLM-based routing 是什么？

A：让语言模型读取输入并输出意图标签或路由决策。

### Q37. Routing 的优点是什么？

A：系统更动态、可处理多种输入，并能把任务导向最合适组件。

### Q38. Routing 的缺点是什么？

A：router 本身成为 failure point，误分类会把请求送到错误流程。

### Q39. Flight booking 例子说明什么？

A：agent 可先检查安全约束、抽取 booking parameters，再规划预订并生成确认邮件。

## Parallelisation

### Q40. Parallelisation pattern 是什么？

A：把相互独立的子任务并行执行，再汇总结果。

### Q41. Parallelisation 适合什么任务？

A：可拆成多个 independent sub-problems 的复杂任务，例如多文档搜索和总结。

### Q42. Parallelisation 的优点是什么？

A：减少等待时间，避免不必要的 sequential calls。

### Q43. Parallelisation 的代价是什么？

A：fork 时计算负载更高，需要 join/merge 结果，并可能处理卡住或冲突的子 agent。

### Q44. Parallelisation 与 prompt chaining 的区别是什么？

A：chaining 顺序依赖前一步输出；parallelisation 让独立步骤同时运行。

### Q45. Multi-document summarisation 为什么适合 parallelisation？

A：可分别搜索和总结 DocA、DocB，再合并差异和共同事实。

### Q46. Join step 为什么重要？

A：并行结果需要整合、去重、解决冲突，并形成最终输出。

## Tool Use Pattern

### Q47. Tool use pattern 是什么？

A：让 LLM 根据当前状态决定何时、如何调用外部函数、数据库或 API。

### Q48. 为什么现实 agent 需要 tool use？

A：真实应用常需要访问最新数据、执行操作、查询数据库或调用专门服务。

### Q49. Tool use 依赖 LLM 的什么能力？

A：理解 tool definitions，并生成结构化输出，例如 JSON 或 Python 函数调用。

### Q50. Tool use 的基本流程是什么？

A：定义工具、LLM 决策、生成函数调用、执行工具、处理 observation/result、可选地让 LLM 生成最终答案。

### Q51. Tool definition 包含什么？

A：工具名称、功能描述、参数 schema 和调用约束。

### Q52. Function call generation 是什么？

A：LLM 输出结构化调用，指定工具名和参数。

### Q53. Observation/result processing 是什么？

A：把工具执行结果整理成 LLM 可理解的上下文。

### Q54. Tool use 的优点是什么？

A：让 LLM 访问最新信息、扩展功能，并与其他 agentic design patterns 结合。

### Q55. Tool use 的缺点是什么？

A：增加输入 tokens，要求模型能可靠生成可执行结构化代码，并引入工具执行错误风险。

### Q56. τ-Bench 想评估什么？

A：评估真实领域中 tool-agent-user interaction 的能力和可靠性。

## MCP 与 Frameworks

### Q57. MCP 是什么？

A：Model Context Protocol，是标准化 LLM 与外部应用、数据源和工具通信的开放协议。

### Q58. MCP 与 tool use 的区别是什么？

A：tool use 是 agent 调用工具的模式；MCP 是标准化这种交互和上下文流动的协议。

### Q59. 为什么 MCP 对真实应用重要？

A：复杂企业系统包含异构、分布式服务，需要统一协议让 agents 获取上下文、执行动作和协调系统。

### Q60. MCP 使用什么模型组织系统？

A：client-server model，用于标准化信息流。

### Q61. MCP 支持定义哪些组件？

A：prompts、resources 和 tools。

### Q62. 什么时候适合使用 MCP？

A：构建复杂、可扩展、企业级、需要多个外部系统协同的 agentic systems。

### Q63. PydanticAI 是什么？

A：一个 GenAI agentic framework，强调清晰 API 和数据验证，便于实现 agentic LLM solutions。

### Q64. 为什么数据验证对 agent framework 重要？

A：工具参数和结构化输出必须可靠，否则执行外部动作会产生错误或安全风险。

### Q65. 选择 agent framework 时要考虑什么 tradeoff？

A：功能丰富度、易用性、可验证性、可维护性和系统复杂度之间的权衡。

## 非考范围提醒与综合比较

### Q66. L28 哪部分不纳入本复习集？

A：根据 exam guide，slides 27-34 不考，因此 reflection、ReAct、producer-critique 和 Reflexion 细节不展开。

### Q67. Prompt chaining、routing、parallelisation 的核心区别是什么？

A：chaining 顺序分解任务；routing 条件选择路径；parallelisation 并行执行独立子任务。

### Q68. Tool use 和 RAG 的关系是什么？

A：RAG 是检索外部信息的特定模式；tool use 更广，可调用数据库、API、代码、搜索或 RAG 工具。

### Q69. Agent reliability 为什么难？

A：agent 需要多步规划、工具调用、上下文管理和环境交互，任何环节错误都可能级联。

### Q70. 为什么 agent safety 需要 action space 控制？

A：agent 能执行真实动作，必须限制权限、验证参数并防止有害或不可逆操作。

## 考前作答模板

### Q71. 如果考“AI agent 定义”，应该怎么答？

A：用 Russell & Norvig 定义：agent 通过 sensors 感知环境，通过 actuators 作用环境；LLM agent 是这个定义在语言/工具环境中的实现。

### Q72. 如果考“agentic problem-solving process”，应该怎么答？

A：列 Get Mission、Scan Scene、Think Through、Take Action、Learn & Get Better，并说明分别对应目标、信息、计划、动作和反馈学习。

### Q73. 如果考“设计模式比较”，应该怎么答？

A：single-call 简单但不适合复杂任务；chaining 模块化但会错误传播；routing 动态但可能误分类；parallelisation 快但需要合并结果；tool use 扩展能力但增加执行风险。

### Q74. 如果考“MCP vs tool use”，应该怎么答？

A：tool use 是调用外部函数的 agent pattern；MCP 是让 LLM 与外部应用、resources 和 tools 标准化通信的协议。

