# L29 Agents Lecture 2 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。重点覆盖 vision-enabled agents、VLM-based agents、embodied AI、Computer Use Agents、OpenCUA trajectory modelling、AgentNet data collection/curation、long-form CoT、训练 recipe、CUA benchmarks/evaluation，以及未来挑战。

## 高频核心题

### Q1. L29 的核心主题是什么？

A：从 text-only agents 扩展到 multimodal 和 embodied environments，重点研究 Computer Use Agents。

### Q2. Text-only LLM agent 的输入输出是什么？

A：主要通过文本感知任务、表示状态和生成输出。

### Q3. VLM-based AI agent 的输入输出是什么？

A：输入可包含 vision+text，输出通常仍可用 text/code/action 表示。

### Q4. 为什么需要 VLM-based agents？

A：现实世界包含图像、视频、图表、表格和 GUI，仅靠文本会丢失大量状态信息。

### Q5. VLM-based agent 的优势有哪些？

A：利用 Web 预训练知识，用文本或代码表示输出，并直接使用视觉信息避免把所有状态转成文本。

### Q6. 为什么视觉能避免 context saturation？

A：截图或图像可直接作为视觉输入，而不需要把整个界面、DOM 或图像细节全部转成超长文本。

### Q7. Embodied AI 是什么？

A：能通过视觉或其他感知流与模拟或真实环境交互的 AI agents。

### Q8. Computer Use Agent 是什么？

A：能控制电脑和应用程序，根据语言指令规划并执行点击、键盘输入等操作完成任务的 agent。

## Computer Use Agents

### Q9. CUA 为什么比普通搜索 agent 更复杂？

A：它需要操作 OS 应用、跨平台执行复杂 workflow，并根据界面状态连续决策。

### Q10. CUA 的长期目标是什么？

A：实现能代表用户自主完成电脑任务的 agents。

### Q11. CUA 中 agent 的状态通常是什么？

A：对于 vision-based agents，状态通常是当前屏幕截图，而不是 DOM 或 HTML。

### Q12. 为什么“不使用 DOM/HTML”是一个重要设定？

A：它让任务更接近真实视觉操作，但也更难，因为 agent 必须从截图中视觉 grounding。

### Q13. CUA 决策过程如何表示？

A：表示为轨迹 `I, s0, a0, s1, a1, ..., sT, aT`。

### Q14. 轨迹中的 `I` 表示什么？

A：语言 instruction 或 command，例如“在 Amazon 找一条蓝色牛仔裤”。

### Q15. 轨迹中的 `s_t` 表示什么？

A：第 `t` 步环境状态，vision-based CUA 中通常是屏幕截图。

### Q16. 轨迹中的 `a_t` 表示什么？

A：第 `t` 步 agent 选择的动作，例如点击、输入、滚动或终止。

### Q17. Goal state 是什么？

A：任务成功完成时的环境状态。

### Q18. Termination action 为什么重要？

A：agent 需要知道何时停止，否则可能循环或继续执行多余动作。

### Q19. Action representation 通常如何建模？

A：把动作建模为函数或结构化文本，例如 click(x,y)、type(text)、scroll、press key 等。

### Q20. 为什么动作表示要结构化？

A：结构化动作才能被系统可靠执行和评估。

## AgentNet 数据收集与整理

### Q21. AgentNet Tool 的作用是什么？

A：自动收集人类在电脑上完成任务的交互数据，用于训练 CUA。

### Q22. AgentNet Tool 收集哪些数据？

A：OBS screen recording、鼠标键盘输入、accessibility trees 等。

### Q23. AgentNet 涵盖哪些操作系统？

A：Ubuntu、Windows 和 macOS。

### Q24. 为什么要收集多 OS 数据？

A：不同系统 UI、快捷键、应用生态不同，模型需要跨环境泛化。

### Q25. AgentNet 中 annotators 做什么？

A：在约 200 个应用和网站上完成多样化任务，以提高任务复杂度和覆盖。

### Q26. AgentNet 任务如何人工验证？

A：根据 goal clarity、diversity 和 complexity 标为 rejected、ok、good 或 excellent。

### Q27. 为什么原始 screen recordings 不能直接训练 agent？

A：视频帧率高、动作密集且低层噪声多，模型难以从全部帧直接学习高层决策。

### Q28. Data curation pipeline 的两个关键步骤是什么？

A：action reduction 和 state-action matching。

### Q29. Action reduction 是什么？

A：降低动作密度，只保留高层有用信号，例如 scroll 只记录起点和终点。

### Q30. State-action matching 是什么？

A：从视频中抽取能预测当前动作的关键帧，与对应动作配对。

### Q31. 为什么要 append final termination frame？

A：让数据包含任务完成后的最终状态，帮助模型学习何时终止。

### Q32. AgentNet dataset 的规模是多少？

A：讲义中提到 22,625 条 human-annotated computer use trajectories。

### Q33. AgentNet OS 分布大致是什么？

A：约 12K Windows、5K macOS、5K Ubuntu trajectories。

### Q34. 每条轨迹平均多少步？

A：讲义中提到平均 18.6 steps。

### Q35. 为什么 trajectory history 对 CUA 重要？

A：agent 需要理解前一步动作、状态变化和可能错误，才能决定下一步。

## OpenCUA 训练

### Q36. 能否直接用 `(I, s_t, a_t)` finetune VLM？

A：可以尝试，但 OpenCUA 发现效果不好，因为模型难以 grounding 和 reasoning over trajectory。

### Q37. 直接 finetune 失败的核心原因是什么？

A：模型无法充分理解任务目标、屏幕状态和历史动作之间的关系。

### Q38. Long-form CoT for CUA 的目的是什么？

A：鼓励模型对任务、屏幕内容和下一步动作进行显式推理。

### Q39. OpenCUA 的 long-form CoT 有哪三层？

A：Level 3 关注 salient textual/visual elements；Level 2 关注 planning 和 reflection；Level 1 预测 action。

### Q40. L1 在训练格式中表示什么？

A：action only，即只输出动作。

### Q41. L2 表示什么？

A：thought + action，即先给推理/计划，再输出动作。

### Q42. L3 表示什么？

A：observation + thought + action，即先描述观察，再推理，最后动作。

### Q43. OpenCUA 如何生成 long-form CoT 数据？

A：使用 Claude-3.7 Sonnet 等模型合成 reasoning traces。

### Q44. 为什么 synthetic CoT 对 CUA 有用？

A：它把视觉元素、历史、计划和动作联系起来，为模型提供中间监督。

### Q45. OpenCUA 还加入哪些额外输入？

A：action history 和 visual history，例如前三张 screenshots。

### Q46. Action history 的作用是什么？

A：帮助模型知道已做过哪些操作，避免重复和理解当前状态。

### Q47. Visual history 的作用是什么？

A：帮助模型追踪界面变化和任务进展。

### Q48. 使用 history 的 tradeoff 是什么？

A：提高准确率但增加输入长度和推理成本。

### Q49. OpenCUA 使用什么 base model 系列？

A：讲义提到从 QwenVL-2.5 开始，finetune 7B、32B、72B 等规模。

### Q50. OpenCUA 的两个训练阶段是什么？

A：Stage 1 为 GUI screen visual grounding；Stage 2 为 OS environment 中的 planning + reasoning。

### Q51. 为什么 joint training 可能更好？

A：同时学习 grounding 和 trajectory reasoning，能力相互支撑，但计算成本更高。

### Q52. Stage 2 only 为什么适合低计算场景？

A：直接用 trajectory data finetune pretrained VLM，成本较低，尤其当 VLM 已具备 GUI grounding 能力时。

## Benchmarks 与 Evaluation

### Q53. CUA evaluation 的核心问题是什么？

A：agent 是否能根据语言指令在真实或模拟电脑环境中达到最终目标。

### Q54. Success rate 衡量什么？

A：任务成功完成的比例。

### Q55. 为什么 CUA 评测不只看最终 success？

A：长任务可能中间卡住、循环、走错路径或无法恢复，需要分析 intermediate goals、backtracking 和 failure原因。

### Q56. Visual grounding benchmark 测什么？

A：agent 是否能定位和选择指定按钮、文本框或屏幕元素。

### Q57. OSWorld 是什么？

A：用于在真实电脑环境中评估 multimodal agents open-ended tasks 的 benchmark。

### Q58. OSWorld-Verified 为什么出现？

A：原 OSWorld 存在网站变化、指令歧义和评估函数脆弱等问题，需要更可靠可复现版本。

### Q59. OSWorld-Verified 改进了什么？

A：云端并行基础设施、修复 300+ 任务问题、公开评测平台和更可靠任务质量。

### Q60. OSWorld 任务为什么会失效？

A：网站结构和内容变化、未来日期任务过期、指令模糊或 evaluation functions 过严。

### Q61. WindowsAgentArena 是什么？

A：类似 OSWorld 的 Windows 环境评测平台，利用 Azure 并行虚拟环境支持大规模评估和 RL 训练。

### Q62. ScienceBoard 想评估什么？

A：在现实科学 workflow 中评估 multimodal autonomous agents 的能力。

### Q63. Online-Mind2Web 是什么？

A：基于 live websites 的网页任务 benchmark，过滤不清晰、失效和 CAPTCHA 阻碍任务。

### Q64. 为什么使用 live websites 更难？

A：真实网站会变化，任务环境不稳定，但更接近真实用户场景。

### Q65. ComputerUseArena 是什么？

A：human-centric、open-ended 的 CUA 评测环境，用户可提出不预定义的任务。

### Q66. 为什么 open-ended evaluation 很重要？

A：静态 benchmark 可能不能反映真实鲁棒性，开放任务能暴露模型在未知请求中的表现。

### Q67. ComputerUseArena 的发现说明什么？

A：静态 benchmark 排名可能与开放评测差异很大，说明 CUA 鲁棒性不足。

## Future Work 与挑战

### Q68. CUA 的四个未来挑战是什么？

A：visual grounding gap、long-horizon modelling、credit-assignment problem、security & reliability。

### Q69. Visual grounding gap 是什么？

A：agent 难以准确理解屏幕元素、按钮、图标和可操作区域之间的关系。

### Q70. Long-horizon modelling 为什么难？

A：CUA 任务步骤多，错误会累积，模型需要记忆历史、规划和恢复。

### Q71. Credit assignment problem 在 CUA 中是什么？

A：任务失败时很难判断是哪一步动作、观察或计划导致最终失败。

### Q72. Security & reliability 为什么特别重要？

A：CUA 能真实操作电脑，错误或攻击可能造成数据泄露、误操作或安全事故。

### Q73. 为什么 language 是 planning 和 action execution 的桥梁？

A：高层用户目标和低层动作都可用语言/结构化文本表示，使 VLM 能连接意图、观察和动作。

### Q74. CUA 与 L28 text-only agents 的核心区别是什么？

A：L28 主要关注文本和工具工作流；L29 让 agent 直接看屏幕并在 OS 中执行动作。

## 考前作答模板

### Q75. 如果考“CUA 是什么”，应该怎么答？

A：CUA 是能根据语言指令观察屏幕、规划并执行电脑操作的视觉 agent，状态通常是截图，动作是点击/输入/滚动/终止等结构化操作。

### Q76. 如果考“OpenCUA 训练管线”，应该怎么答？

A：先用 AgentNet 收集和清理 trajectories，再用 long-form CoT 构造 observation/thought/action 监督，最后对 VLM 做 GUI grounding 与 OS planning/reasoning 训练。

### Q77. 如果考“为什么 CUA 评测困难”，应该怎么答？

A：任务长、多步、环境变化大，成功率不足以解释失败，还要评估 grounding、循环、backtracking、中间目标和安全性。

### Q78. 如果考“OSWorld-Verified 的意义”，应该怎么答？

A：它修复原 OSWorld 中网站变化、指令歧义、评估函数脆弱等问题，使 CUA 评测更可复现、更公平。

### Q79. 如果考“CUA 未来挑战”，应该怎么答？

A：答 visual grounding、long-horizon modelling、credit assignment、security/reliability，并说明 CUA 因能真实操作电脑所以安全风险更高。

