# L15 Multilingual LLMs 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。重点覆盖 multilingual LLM 的动机、语言资源不均衡、建模选择、tokenization、数据混合、scaling/transfer laws、评测、安全与语言社区。

## 高频核心题

### Q1. 为什么 multilingual LLM 重要？

A：世界上大量用户不以英语为主要语言，多语言模型关系到公平、可及性、知识覆盖和跨语言迁移。

### Q2. Multilingual LLM 的核心目标是什么？

A：让一个模型能理解、生成并推理多种语言，同时尽量在高资源和低资源语言上都保持质量。

### Q3. 为什么“只做英语模型”是不够的？

A：它会排除大量非英语用户，并把模型能力、知识和安全保障集中在少数高资源语言上。

### Q4. 多语言模型相比每种语言单独训练模型有什么优势？

A：共享参数可降低成本，并让高资源语言的知识和能力迁移到低资源语言。

### Q5. 多语言模型的主要挑战有哪些？

A：数据不均衡、tokenization 不公平、跨语言 transfer/interference、评测困难，以及文化和安全差异。

### Q6. Positive transfer 是什么？

A：一种语言的训练帮助另一种语言提升表现，常发生在相近语言、共享脚本或共享任务结构之间。

### Q7. Negative interference 是什么？

A：多语言共享模型时，一种语言或任务的学习损害另一种语言表现。

### Q8. Curse of multilinguality 是什么？

A：语言数量增加后，固定模型容量和训练预算被更多语言共享，单个语言可能得到更少资源，表现下降。

## 语言资源与数据不均衡

### Q9. 为什么语言资源分布是多语言 NLP 的根本问题？

A：大多数语言缺少足够的标注数据、未标注文本、工具链和评测集。

### Q10. 讲义中提到大量语言缺少什么？

A：大量语言没有足够 labelled 或 unlabelled resources，因此难以直接训练和评测。

### Q11. High-resource language 是什么？

A：拥有大量文本、标注数据、工具和 benchmark 的语言，如英语、西班牙语、中文等。

### Q12. Low-resource language 是什么？

A：可用数字文本和标注资源很少的语言，常在模型训练和评测中被边缘化。

### Q13. 为什么低资源语言不是“问题小”？

A：低资源通常反映数字化和资源建设不平等，不代表使用者少或语言价值低。

### Q14. 多语言数据不均衡如何影响模型？

A：模型会偏向高资源语言，低资源语言可能生成质量差、混合语言、事实错误或不安全回答。

### Q15. 为什么语言识别是多语言数据管线的重要步骤？

A：如果语料语言被识别错，模型会学习错误分布，评测和采样也会被污染。

### Q16. FineWeb2 的目标是什么？

A：构建大规模、多语言、可处理 Common Crawl 数据的高质量网络文本语料。

### Q17. FineWeb2 pipeline 包括哪些步骤？

A：语言识别、tokenisation、去重、质量过滤和 rehydration 等。

### Q18. Rehydration 在 FineWeb2 中是什么意思？

A：利用多个 Common Crawl snapshots 中重复出现的文档信息帮助判断质量和代表性。

### Q19. 为什么只出现一次的网页可能质量较低？

A：singleton 页面更可能是噪声、垃圾内容或偶然抓取结果。

### Q20. 为什么出现太多次的网页也可能有问题？

A：高度重复页面可能过度代表模板、镜像或低信息量内容，导致训练分布失衡。

## 建模选择

### Q21. 多语言模型设计要决定哪些组件是否共享？

A：词表、embedding、encoder/decoder、attention layers、任务头和语言特定参数都可能共享或拆分。

### Q22. 早期多语言模型常使用什么语言特定设计？

A：可能使用语言特定 embedding、encoder/decoder 或输出头，以减轻语言差异带来的冲突。

### Q23. 当前大模型通常采用什么共享策略？

A：多数现代 LLM 共享几乎全部参数，只通过数据、tokenizer 和 prompt 控制语言行为。

### Q24. 为什么在 prompt 中显式标记语言可能有帮助？

A：语言标签或明确指令能减少语言混淆，帮助模型选择目标语言和任务格式。

### Q25. Cross-lingual language model 的核心思想是什么？

A：通过共享表示和跨语言目标，使模型在多种语言之间迁移知识。

### Q26. XLM/TLM 这类方法为什么重要？

A：它们展示了跨语言预训练可以利用平行文本和共享目标提升多语言理解。

### Q27. English-first models 的问题是什么？

A：它们主要由英语数据驱动，多语言能力可能来自少量附带数据或迁移，低资源语言不稳定。

### Q28. Bilingual models 的特点是什么？

A：重点支持两种或少数语言，通常能在目标语言上更强，但覆盖范围较窄。

### Q29. Massively multilingual models 的特点是什么？

A：覆盖几十到上百种语言，追求广覆盖和跨语言迁移，但更容易遇到容量和数据分配问题。

### Q30. BLOOM、Aya、Glot500、EuroLLM 这类项目共同体现什么？

A：它们体现了面向多语言覆盖、开放训练和非英语能力建设的努力。

## Tokenization

### Q31. Tokenization 为什么会影响多语言公平性？

A：同样一句话在不同语言中可能被切成不同数量 tokens，影响训练成本、上下文长度和生成质量。

### Q32. Subword tokenizer 的基本思想是什么？

A：把词拆成常见子词单位，在词级和字符级之间折中，既能处理常见词也能处理新词。

### Q33. 多语言 tokenizer 的困难是什么？

A：优化一种语言的切分可能损害另一种语言；不同脚本、形态和空格习惯差异很大。

### Q34. Subword fertility 是什么？

A：每个真实词平均被拆成多少 subword tokens；数值越高通常表示该语言 tokenization 越低效。

### Q35. Proportion of continued words 是什么？

A：被拆成多个 token 的词所占比例，用于衡量 tokenizer 对某语言是否过度切分。

### Q36. 高 subword fertility 会带来什么问题？

A：同样内容占用更多 context window，训练和推理成本更高，并可能让模型更难学习词级规律。

### Q37. Byte-level tokenizer 的优点是什么？

A：理论上可以表示任何文本，避免 unknown tokens。

### Q38. Byte-level tokenizer 的代价是什么？

A：某些语言可能被拆得更碎，token 序列更长，训练效率较低。

### Q39. Gemma tokenizer 的讲义要点有哪些？

A：它使用 SentencePiece/BPE，大词表、byte fallback、数字切分和保留空格等设计，以增强覆盖和稳定性。

### Q40. 为什么 tokenizer 不是中立技术细节？

A：tokenizer 决定不同语言在模型预算中的“价格”，会直接影响语言间性能差距。

## 数据质量与数据混合

### Q41. 多语言语料常见噪声有哪些？

A：错误语言、机器翻译噪声、乱码、模板文本、重复内容、错配句对和非自然语言内容。

### Q42. 数据质量标签中 correct 表示什么？

A：文本语言和内容符合预期，可作为有效训练样本。

### Q43. Wrong language 表示什么？

A：样本被归到某语言，但实际是另一种语言。

### Q44. Incorrect translation 表示什么？

A：平行或翻译数据中，源文本与目标文本语义不匹配。

### Q45. Not language 表示什么？

A：样本不是自然语言文本，例如乱码、代码片段或无意义符号。

### Q46. 为什么低资源语言更容易受数据质量影响？

A：可用数据本来就少，少量噪声会占更大比例，且自动过滤工具也常对低资源语言更差。

### Q47. Data mix 是什么？

A：训练时不同语言数据的采样比例。

### Q48. Natural/proportional sampling 是什么？

A：按语料规模采样，数据多的语言被采样更多。

### Q49. Uniform sampling 是什么？

A：每种语言采样概率相近，能提升低资源语言曝光，但可能过采样小语料。

### Q50. Temperature sampling 的公式直觉是什么？

A：用 `p_i = n_i^alpha / sum_j n_j^alpha` 调节语言采样，`alpha` 越小越接近均匀采样。

### Q51. Temperature sampling 想解决什么 tradeoff？

A：在高资源语言数据量优势和低资源语言曝光之间折中。

### Q52. 为什么过度上采样低资源语言可能有害？

A：会重复训练少量样本，增加过拟合和噪声放大。

## Scaling Laws 与 Transfer

### Q53. ATLAS 研究关注什么？

A：关注 multilingual scaling laws，研究模型规模、数据、语言数量和跨语言迁移如何共同影响表现。

### Q54. Adaptive transfer law 的核心思想是什么？

A：目标语言表现不仅取决于本语言数据，还取决于相似语言和其他语言带来的正负迁移。

### Q55. Transfer matrix 表示什么？

A：表示一种语言的训练数据对另一种语言表现的影响，可包含 positive transfer 和 negative interference。

### Q56. 哪些因素常预测跨语言 transfer？

A：脚本、语言家族、词汇相似性、数据质量、任务类型和训练比例。

### Q57. 为什么相同脚本可能帮助 transfer？

A：共享字符、子词和拼写规律能让模型更容易共享表示。

### Q58. 为什么语言家族可能帮助 transfer？

A：相近语言常有类似语法、词形和词汇来源，更容易迁移。

### Q59. Curse of multilinguality 是否意味着多语言一定更差？

A：不是。多语言会带来容量竞争，但 positive transfer 可部分抵消甚至超过损失。

### Q60. 讲义中关于“增加语言数量”的 scaling 直觉是什么？

A：如果存在正迁移，加入更多语言所需的数据和模型增长可能低于线性增加。

### Q61. Pretraining from scratch 与 finetuning 的比较是什么？

A：低预算时 finetuning 多语言模型常更划算；足够计算和数据时，从头预训练可能获得更强目标语言能力。

### Q62. 为什么不能只看平均分评价多语言模型？

A：平均分会掩盖低资源语言失败、语言间方差和特定社区的风险。

## 评测

### Q63. 多语言评测为什么困难？

A：不同语言数据质量、文化背景、任务难度和翻译等价性都可能不同。

### Q64. 机器翻译 benchmark 的问题是什么？

A：翻译可能不自然、不等价或泄露英语中心内容，不能代表真实语言使用。

### Q65. 为什么需要专家验证多语言 benchmark？

A：只有熟悉语言和文化的人才能判断题目是否自然、正确、公平。

### Q66. 自动指标在多语言评测中有什么风险？

A：它们可能与人类判断相关性弱，尤其在开放生成、低资源语言和文化语境中不可靠。

### Q67. culturally adapted benchmark 是什么？

A：不是简单翻译题目，而是根据目标语言和文化重写，使任务对当地使用者自然且有意义。

### Q68. 报告多语言结果时应注意什么？

A：应报告每种语言或语言组表现、方差和失败案例，而不是只给 overall mean。

### Q69. 为什么英语 benchmark 不能直接代表 multilingual ability？

A：模型可能英语很强但非英语弱，尤其在安全、事实性和推理任务上差距明显。

## 安全、责任与语言社区

### Q70. 多语言安全为什么重要？

A：安全机制常在英语上更强，非英语尤其低资源语言中更容易出现有害或不可靠输出。

### Q71. Safety transfer 是什么？

A：在一种语言学到的安全对齐能否迁移到其他语言。

### Q72. 为什么 safety transfer 可能脆弱？

A：有害请求、文化规范和表达方式在语言间不同，训练数据也高度不均衡。

### Q73. XSafety 这类数据集关注什么？

A：关注多语言和跨文化安全风险，包括不同语言中的有害请求和模型响应。

### Q74. “Think in English then respond in target language” 为什么可能影响安全？

A：模型的英语安全能力更强，让中间推理转到英语可能减少某些非英语 unsafe responses。

### Q75. 为什么这种英语中转策略不是根本解决方案？

A：它仍以英语为中心，可能丢失本地文化语境，也不能替代目标语言安全数据和社区参与。

### Q76. Global harms 和 local harms 的区别是什么？

A：global harms 跨地区普遍存在；local harms 与特定语言、文化、政治或社会语境相关。

### Q77. 为什么语言是文化身份的一部分？

A：语言承载知识、价值、历史和社群关系，模型支持不足会造成文化和知识边缘化。

### Q78. Epistemic injustice 在多语言 NLP 中是什么意思？

A：某些语言和社区的知识被忽视、误表示或无法进入模型系统。

### Q79. Data sovereignty 是什么？

A：语言社区应对自身数据如何收集、使用、共享和商业化拥有控制权。

### Q80. Participatory design 为什么重要？

A：它让受影响社区参与目标、数据、评价和部署决策，减少外部研究者单方面定义问题。

### Q81. Masakhane 体现了什么理念？

A：以非洲语言社区为中心、开放协作、社区参与和能力建设的 NLP 研究模式。

## 考前作答模板

### Q82. 如果考“多语言 LLM 的挑战”，应该怎么答？

A：答数据不均衡、tokenization 不公平、transfer/interference、评测和安全文化差异，并说明低资源语言尤其脆弱。

### Q83. 如果考“temperature sampling”，应该怎么答？

A：写出 `p_i = n_i^alpha / sum_j n_j^alpha`，说明它在 proportional 和 uniform sampling 间调节语言采样。

### Q84. 如果考“tokenizer 对多语言的影响”，应该怎么答？

A：说明不同语言 token 数不同，subword fertility 和 continued words 会影响成本、context 长度和模型表现。

### Q85. 如果考“多语言评测原则”，应该怎么答？

A：不要只看平均分；避免未经验证的机器翻译 benchmark；报告分语言结果，并重视文化适配和专家验证。

