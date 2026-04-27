# L20 Safety and Security 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。根据 `examguide.pdf`，Lecture 20 的 slide 19 到最后不考；本文件聚焦可考核心：safety vs security、LLM watermarking、watermark 检测与局限、security/safety threat types、illicit-content attacks、training data filtering，以及 adversarial suffix jailbreak。

## 高频核心题

### Q1. Safety 和 security 的区别是什么？

A：Safety 关注系统不会造成伤害；security 关注系统不被恶意利用、攻击或绕过。

### Q2. LLM safety 关注哪些问题？

A：有害内容、偏见、错误信息、隐私泄露、危险建议和不符合人类意图的行为。

### Q3. LLM security 关注哪些问题？

A：prompt injection、jailbreak、data exfiltration、模型滥用、水印规避和系统被攻击者操纵等。

### Q4. 为什么 safety 与 security 会重叠？

A：攻击者常通过 security 漏洞绕过 safety guardrails，从而诱导模型产生有害输出。

### Q5. 本讲可考部分主要关注哪些方向？

A：watermarking、illicit content prevention、adversarial suffix attacks 和 jailbreak transfer。

### Q6. 为什么 LLM 输出来源识别重要？

A：识别 AI-generated text 有助于检测滥用、学术诚信问题、垃圾内容和虚假信息传播。

### Q7. Watermarking 的核心思想是什么？

A：在生成文本时嵌入统计上可检测的模式，使检测器能判断文本是否可能由某模型生成。

### Q8. LLM watermark 为什么不能像图片水印一样直接嵌入不可见标记？

A：文本是离散符号，微小改动会改变含义或流畅度，因此通常只能嵌入统计偏好。

## Watermarking 算法

### Q9. Watermarking 在 decoding 时如何工作？

A：根据前文和密钥把词表划分为 green list 与 red list，并提高 green tokens 的采样概率。

### Q10. Green list 是什么？

A：在某一步生成中被 watermark 算法偏好的 tokens 集合。

### Q11. Red list 是什么？

A：相对不被 watermark 偏好的 tokens 集合。

### Q12. Watermark key 的作用是什么？

A：决定每个上下文下词表如何被划分，使外部观察者难以伪造或检测具体规则。

### Q13. Watermark logit bias 是什么？

A：在 sampling 前给 green tokens 增加额外 logit，使它们更可能被生成。

### Q14. 为什么 watermark 仍需保持文本质量？

A：如果 bias 太强，文本会变得不自然；如果太弱，检测信号又不明显。

### Q15. Watermark 的核心 tradeoff 是什么？

A：文本质量、检测强度和鲁棒性之间的平衡。

### Q16. 为什么 watermark 是 probabilistic？

A：它不是强制每个 token 都来自 green list，而是提高 green token 的概率，因此检测基于统计显著性。

### Q17. Watermarked text 的检测直觉是什么？

A：统计文本中 green tokens 的比例是否显著高于无水印文本的期望。

### Q18. 为什么长文本更容易检测 watermark？

A：样本 token 更多，统计检验更有力，green token 比例的偏差更稳定。

### Q19. 为什么短文本难检测？

A：token 数太少，随机波动大，难以区分水印信号和偶然偏差。

### Q20. Watermark detector 通常输出什么？

A：一个统计分数或显著性判断，表示文本是否可能含有水印。

## Watermark 识别与局限

### Q21. Watermark detection 的 false positive 是什么？

A：人类或无水印文本被误判为 watermarked。

### Q22. Watermark detection 的 false negative 是什么？

A：有水印文本没有被检测出来。

### Q23. 为什么 paraphrasing 会削弱 watermark？

A：改写会替换 tokens，破坏原生成过程嵌入的 green token 统计模式。

### Q24. 为什么编辑、翻译和摘要会影响 watermark？

A：这些操作改变 token 序列，可能删除或稀释水印信号。

### Q25. Watermarking 能否证明文本一定由某模型生成？

A：不能。它只能提供统计证据，且受攻击、文本长度和检测阈值影响。

### Q26. 为什么 watermark key 需要保密？

A：如果 key 泄露，攻击者可以伪造水印或专门生成绕过检测的文本。

### Q27. Watermarking 对开源模型有什么挑战？

A：攻击者可修改 decoding 过程、移除水印或访问模型实现细节。

### Q28. Watermarking 对 closed models 有什么优势？

A：服务方可控制 decoding 和 key，更容易统一嵌入与检测水印。

### Q29. 为什么 watermark 不是完整 safety solution？

A：它只帮助追踪或识别生成文本，不能阻止模型产生有害内容。

### Q30. Watermarking 和 content moderation 的区别是什么？

A：watermark 判断来源；content moderation 判断内容是否违规或有害。

## Threat Types 与 Illicit Content

### Q31. LLM security concerns 可以包括哪些？

A：恶意提示、越狱、隐私泄露、自动化滥用、模型复制、输出伪造和检测规避。

### Q32. Illicit content attack 是什么？

A：攻击者诱导模型生成违法、危险或平台禁止的内容。

### Q33. 为什么 illicit content prevention 很难？

A：同一知识可有良性和恶意用途，攻击者还能用改写、角色扮演或编码绕过规则。

### Q34. Direct prevention by filtering training data 是什么？

A：在训练前从数据中移除或降低有害内容，以减少模型学习危险行为。

### Q35. 训练数据过滤的优点是什么？

A：从源头减少模型接触明显有害数据，降低部分危险能力或有害风格学习。

### Q36. 训练数据过滤的局限是什么？

A：很难定义和检测所有有害内容，过滤可能损害合法知识，并不能阻止推理组合出危险信息。

### Q37. 为什么不能简单删除所有敏感知识？

A：许多知识有 dual-use 性质，在医学、安全研究、教育等场景中可能是合法必要的。

### Q38. Safety classifier 或 refusal policy 的作用是什么？

A：在训练或推理阶段识别有害请求，并促使模型拒绝或给出安全替代答案。

### Q39. 为什么 refusal policy 需要平衡？

A：过松会放出有害内容，过严会拒绝合法请求，降低可用性。

### Q40. Jailbreak 是什么？

A：通过特殊提示或策略绕过模型安全机制，让模型输出本应拒绝的内容。

## Adversarial Suffix Attacks

### Q41. Zou et al. adversarial suffix attack 的目标是什么？

A：寻找一段可附加到恶意请求后的 suffix，使模型更可能服从并生成有害内容。

### Q42. 为什么 adversarial suffix attack 很危险？

A：suffix 可能通用、自动化搜索得到，并能迁移到多个模型。

### Q43. Positive affirmation objective 是什么？

A：优化 suffix 使模型输出以肯定服从的开头开始，例如倾向于回答“Sure, here is...”而不是拒绝。

### Q44. 为什么让模型先“答应”很关键？

A：一旦模型进入服从模式，后续生成更可能继续给出违规内容。

### Q45. Coordinate gradient search 的直觉是什么？

A：逐个位置搜索或替换 suffix tokens，根据梯度选择更能提高攻击目标概率的 token。

### Q46. 为什么 discrete token attack 需要特殊搜索方法？

A：tokens 是离散的，不能像连续向量一样直接用普通梯度下降更新。

### Q47. Universal adversarial suffix 是什么？

A：同一 suffix 可对多个不同恶意请求起作用，而不是只针对单个 prompt。

### Q48. Transferability 指什么？

A：在一个或多个模型上找到的攻击 suffix 可迁移到其他模型，包括 closed models。

### Q49. 为什么 transfer to closed models 特别严重？

A：攻击者即使不能访问 closed model 权重，也可能用开源模型优化 suffix 后迁移攻击。

### Q50. Adversarial suffix attack 暴露了什么问题？

A：当前 safety alignment 可能依赖脆弱的表面模式，面对优化过的提示会失效。

### Q51. 这种攻击与普通 prompt engineering 有何不同？

A：普通 prompt engineering 多靠人工设计；adversarial suffix 用优化算法系统搜索触发安全失败的 token 序列。

### Q52. 为什么 suffix 常看起来像乱码？

A：优化目标是改变模型概率分布，不一定追求人类可读性。

### Q53. 为什么乱码 suffix 仍能有效？

A：模型对 token 序列的内部表示可能被这些 token 强烈影响，即使人类觉得无意义。

### Q54. 防御 adversarial suffix 的思路有哪些？

A：更强 adversarial training、输入检测、robust refusal、解码约束、多模型审核和持续红队测试。

### Q55. 为什么简单 blacklist suffix 不够？

A：攻击者可以重新优化出新 suffix，且可能存在大量功能相似的 token 序列。

## Safety/Security Tradeoffs

### Q56. 为什么 safety 不能只靠训练数据过滤？

A：模型可组合知识，攻击者可绕过过滤，且许多风险出现在部署交互中。

### Q57. 为什么 safety 不能只靠 inference-time filter？

A：filter 可能漏检、误检，且模型内部仍可能具备危险能力。

### Q58. 多层防御是什么意思？

A：结合数据治理、训练对齐、系统提示、分类器、工具权限、监控、水印和红队测试。

### Q59. Red teaming 在 LLM safety 中是什么？

A：系统性寻找模型或应用的安全漏洞，包括诱导有害输出和绕过 guardrails。

### Q60. 为什么公开 attack 方法仍有价值？

A：它们帮助研究者发现脆弱点、构建更强 benchmark 和改进防御。

### Q61. Safety evaluation 为什么需要 adaptive attacks？

A：静态测试容易被模型记住或过拟合，adaptive attacks 更能暴露真实部署风险。

### Q62. 为什么安全和可用性存在 tension？

A：更严格拒绝可降低风险但可能拒绝合法请求；更宽松回答可提高可用性但增加滥用风险。

### Q63. 为什么安全策略需要 context awareness？

A：同一内容在教育、研究、防御或恶意场景下风险不同，需要根据意图和上下文判断。

### Q64. 本讲非考部分从哪里开始？

A：根据 exam guide，slide 19 到最后不考；因此本复习集不覆盖后续 fairness/SVD/SAL 等细节。

## 考前作答模板

### Q65. 如果考“safety vs security”，应该怎么答？

A：safety 是避免伤害，security 是防止被攻击或滥用；二者相互影响，jailbreak 就是 security failure 导致 safety failure。

### Q66. 如果考“LLM watermarking”，应该怎么答？

A：生成时用密钥把词表分成 green/red list，提高 green tokens 概率；检测时统计 green token 比例是否显著偏高。

### Q67. 如果考“watermark 的局限”，应该怎么答？

A：短文本统计弱，改写/翻译/编辑会破坏信号，key 泄露会被伪造或规避，且 watermark 不能阻止有害内容。

### Q68. 如果考“adversarial suffix attack”，应该怎么答？

A：攻击者用优化搜索一段 suffix，使模型从拒绝转为服从；可通过 positive affirmation objective 和 coordinate gradient search 找到，并可能迁移到 closed models。

### Q69. 如果考“训练数据过滤的利弊”，应该怎么答？

A：过滤能减少明显有害样本，但难覆盖 dual-use 知识和部署期组合风险，也可能误删合法内容。
