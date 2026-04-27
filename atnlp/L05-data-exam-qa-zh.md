# L05 Data 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。重点覆盖数据的重要性、预训练数据来源、数据筛选、去重、FineWeb/FineWeb-Edu、post-training data、synthetic instruction data，以及数据伦理与法律。

## 高频核心题

### Q1. 为什么说 data is the key？

A：因为模型只能从训练数据中学习知识、语言模式、推理模式和偏差；数据的质量、覆盖面和筛选方式直接决定模型能力。

### Q2. 数据为什么是现代 AI 的竞争优势？

A：模型架构和工程细节容易被论文和系统复现，但高质量、专有、持续维护的数据集更难获得，因此能带来长期性能优势。

### Q3. 为什么大型模型的数据集常常不公开？

A：主要原因是竞争优势和版权责任：公开数据来源可能削弱商业壁垒，也可能暴露版权风险。

### Q4. 训练 LLM 的数据大致分为哪几类？

A：主要包括 pre-training data、annealing 或 mid-training data，以及 post-training data。

### Q5. 什么是 pre-training data？

A：Pre-training data 是用于训练 base model 的大规模原始文本或代码数据，目标是让模型学习通用语言、知识和模式。

### Q6. 什么是 annealing 或 mid-training？

A：它是在预训练后期使用更高质量或更面向特定能力的数据继续训练，以强化某些能力。

### Q7. 什么是 post-training data？

A：Post-training data 用于让模型学习期望行为，包括 instruction training、RL、alignment 和 task fine-tuning。

### Q8. base model 和 instruct model 的区别是什么？

A：Base model 是预训练和 mid-training 后的模型；instruct model 是进一步经过 post-training 后，更会遵循人类指令的模型。

### Q9. 数据质量为什么需要大量人工努力？

A：因为高质量数据需要专家判断、持续清洗、领域覆盖、边界案例处理和人工制定筛选标准。

### Q10. 数据问题中的 long tail 指什么？

A：指大量低频但重要的问题，例如少数领域、边缘案例、低资源语言、罕见格式和质量异常。

## 预训练数据来源

### Q11. Wikipedia 为什么常被用于 LLM 训练？

A：因为它质量较高、结构清晰、多语言覆盖广、许可证较开放，并且有丰富元数据。

### Q12. Wikipedia 有哪些优点？

A：优点包括社区维护、质量控制机制、300 多种语言覆盖、Creative Commons license 和较高知识密度。

### Q13. Wikipedia 有哪些局限？

A：它存在地理和人口统计偏差，例如女性编辑比例较低，某些地区和群体代表性不足。

### Q14. 为什么 web data 是最重要的预训练来源之一？

A：因为 web 数据规模巨大，覆盖多领域、多文体、多语言，包含 LLM 所需的大量世界知识和语言知识。

### Q15. web data 的主要问题是什么？

A：它噪声大、质量参差不齐、偏向年轻用户和发达国家，尤其可能过度代表美国和英语互联网。

### Q16. Common Crawl 是什么？

A：Common Crawl 是一个非营利组织提供的开放网络爬取数据，包含 petabytes 级网页数据。

### Q17. Common Crawl 为什么重要？

A：它让研究者和小机构也能访问大规模 web 数据，而不只是大型公司能爬取和使用互联网数据。

### Q18. Common Crawl 的数据有什么特点？

A：规模极大、每月新增大量网页、存在重复和噪声，需要大量清洗和筛选。

### Q19. GitHub 数据为什么重要？

A：代码数据对编程任务很重要，也可能帮助模型学习结构化推理、工具使用和问题分解模式。

### Q20. GitHub 数据有什么有用的元数据？

A：包括 stars、项目成熟度、活跃度、issues、pull requests 和 license 信息。

### Q21. The Stack 数据集是什么？

A：The Stack 是一个大规模代码数据集，包含多种编程语言的 permissively licensed code。

### Q22. 为什么代码数据要特别关注 license？

A：因为代码通常有明确许可证，训练数据筛选需要避免使用不允许再分发或商业使用的代码。

## 数据选择目标

### Q23. data selection 的总体目标是什么？

A：选择最适合训练目标的数据，同时提高性能、减少成本、保证评价完整性，并减少有害行为。

### Q24. 什么是 optimal dataset？

A：Optimal dataset 是尽可能接近模型测试或部署时分布的数据集。

### Q25. 为什么 data selection 能降低成本？

A：通过去掉低质量、重复、无关或有害数据，可以减少训练 tokens、计算资源和存储成本。

### Q26. 为什么 data selection 和 evaluation integrity 有关？

A：如果测试集内容进入训练数据，模型分数可能来自记忆而不是泛化，评价就失真。

### Q27. 数据选择的常见目标有哪些？

A：提高模型性能、提高数据效率、提高选择效率、保护评价完整性、减少 bias 和 toxicity。

### Q28. 为什么不同 learning stage 的数据选择目标不同？

A：预训练更关注规模、质量和效率；instruction tuning 更关注任务多样性和指令格式；alignment 更关注行为、安全和偏好。

### Q29. 预训练数据过滤 pipeline 通常包括哪些环节？

A：包括 language filtering、heuristics、data quality filtering、domain-specific filtering、deduplication、toxic/explicit content filtering 和 data mixing。

### Q30. data mixing 是什么？

A：Data mixing 是决定不同来源、语言、领域或质量等级数据在最终训练语料中的比例。

## Language Filtering

### Q31. 为什么需要 language filtering？

A：因为训练者可能希望控制语言比例、聚焦某些语言，或过滤掉不符合目标语言的数据。

### Q32. 为什么不总是直接做 multilingual training？

A：多语言数据质量更难保证、筛选成本更高，而且不同语言在数据量和质量上差异很大。

### Q33. URL-based language filtering 是什么？

A：通过域名或 URL 线索判断语言，例如 `.fr` 可能对应法语、`.es` 可能对应西班牙语。

### Q34. URL-based filtering 有什么局限？

A：它对低资源语言、跨语言网站和 code-switching 情况不可靠。

### Q35. fastText 在 language filtering 中的作用是什么？

A：fastText 是常用语言识别器，可以快速识别多种语言，适合大规模数据过滤。

### Q36. language identification 的难点有哪些？

A：短文本难判断，相似语言易混淆，方言和 code-switching 也可能被误判。

### Q37. 为什么 language filtering 可能伤害方言？

A：如果阈值设置过严，模型可能把非标准英语或方言误判为低质量或错误语言，从而过滤掉边缘群体语言。

## Heuristic Filtering

### Q38. heuristic filtering 的目标是什么？

A：快速去除 boilerplate、错误信息、重复片段、乱码、低质量格式和明显不适合训练的文本。

### Q39. heuristic filtering 为什么必须快？

A：预训练语料规模极大，过滤函数需要能在海量文档上低成本运行。

### Q40. heuristic filtering 的局限是什么？

A：它只能看表面统计，不能真正判断文档内容是否有深度或教育价值。

### Q41. 常见 heuristic filtering 特征有哪些？

A：包括字符数、词数、行数、重复次数、是否包含特定词、标点是否完整、字母比例、数字比例和平均行长。

### Q42. 为什么要过滤 boilerplate？

A：Boilerplate 如导航栏、登录提示和模板文本会占用大量 token，却提供很少有效语言或知识信号。

### Q43. 为什么重复文本会影响训练？

A：重复文本会浪费计算资源，并可能让模型过拟合某些低质量或频繁重复的模式。

## Data Quality

### Q44. 数据质量为什么难以定义？

A：因为“高质量”最终取决于训练后模型在什么测试集或应用场景上表现更好。

### Q45. 讲义中给出的高质量数据例子有哪些？

A：Wikipedia、books、patents 和 peer-reviewed journal articles。

### Q46. classifier-based quality filtering 是什么？

A：训练一个分类器判断数据点是否接近某个已知高质量参考语料的分布。

### Q47. 为什么质量过滤比简单 heuristic 更贵？

A：因为它通常需要更复杂的表示、模糊匹配或分类模型，而不是简单统计规则。

### Q48. 为什么数据质量和 benchmark performance 不能简单等同？

A：因为一个筛选策略可能提升某些 benchmark，却降低其他能力或损害多样性。

## Deduplication 与 MinHash

### Q49. deduplication 是什么？

A：Deduplication 是去除训练语料中的重复或近重复文档。

### Q50. 为什么预训练数据需要 deduplication？

A：它能减少数据规模和训练成本，提高泛化，并降低模型记忆低质量重复内容的风险。

### Q51. exact matching deduplication 是什么？

A：它通过完全相同的内容或 URL 来删除重复文档。

### Q52. approximate matching deduplication 是什么？

A：它检测近似重复文档，例如模板相同但细节略有变化的网页。

### Q53. MinHash 用来解决什么问题？

A：MinHash 用来高效估计大量文档之间的 Jaccard similarity，从而发现近重复文档。

### Q54. 为什么不能直接两两比较所有文档？

A：因为比较数量随文档数平方增长，百万文档就约有 5000 亿次比较，成本不可接受。

### Q55. MinHash 的基本步骤是什么？

A：先把文档转成 token 或 n-gram 集合，再用多个 hash 函数生成签名，最后比较签名相似度估计集合重叠。

### Q56. MinHash 签名中的匹配比例表示什么？

A：它近似表示两个文档集合的 Jaccard similarity。

### Q57. 为什么 hash 函数越多，MinHash 越准确？

A：更多 hash 函数生成更长签名，可以更稳定地估计真实相似度。

## FineWeb 与 FineWeb-Edu

### Q58. FineWeb 是什么？

A：FineWeb 是一个从 Common Crawl 构建的大规模高质量 web text 数据集，规模约 15T tokens。

### Q59. FineWeb 的核心思想是什么？

A：Quality trumps quantity，即经过精心筛选的高质量数据比单纯更多数据更有价值。

### Q60. FineWeb 使用了多少 Common Crawl snapshots？

A：讲义中提到 FineWeb 使用了 96 个 Common Crawl snapshots。

### Q61. FineWeb 如何选择过滤策略？

A：它系统评估大量候选 heuristics，选择少量有效过滤器，并比较不同去重策略对性能的影响。

### Q62. FineWeb 如何评估过滤效果？

A：可以人工检查过滤前后数据，也可以用过滤后的数据训练模型并在 benchmark 上评估。

### Q63. 为什么评估过滤策略很慢很贵？

A：因为最可靠的方法往往要实际训练模型，再观察 downstream benchmark performance。

### Q64. FineWeb 中 deduplication 的重要发现是什么？

A：不同去重粒度会影响性能；去除跨 crawl 的大重复簇可能是性能提升的重要来源。

### Q65. FineWeb-Edu 是什么？

A：FineWeb-Edu 是 FineWeb 中经过教育价值筛选后的高质量 educational content 子集。

### Q66. FineWeb-Edu 如何获得教育价值标签？

A：先用强 LLM 对样本文档打教育价值分数，再用这些 synthetic labels 训练轻量分类器。

### Q67. FineWeb-Edu 为什么不直接用 LLM 给全部 15T tokens 打分？

A：成本过高；讲义提到直接用 LLM 过滤会非常昂贵，因此使用轻量分类器扩展到大规模数据。

### Q68. FineWeb-Edu 使用了什么模型打初始标签？

A：讲义中提到使用 Llama-3-70B-Instruct 给 460,000 个 FineWeb 样本文档评分。

### Q69. FineWeb-Edu 的轻量分类器如何工作？

A：在 Snowflake-arctic-embed-m embeddings 上训练 linear regression classifier 来预测教育价值分数。

### Q70. FineWeb-Edu 的包含阈值是什么？

A：讲义中提到使用 educational score `>=3` 作为纳入阈值。

### Q71. FineWeb-Edu 最终保留了多少 tokens？

A：从 15T token FineWeb corpus 中保留了约 1.3T tokens 的高质量教育内容。

### Q72. FineWeb-Edu 的图表说明了什么？

A：高质量教育数据能以更少训练 tokens 达到更高 benchmark performance，说明数据质量比单纯数量更关键。

### Q73. FineWeb-Edu 在 MMLU 图中有什么关键信息？

A：它在较少 tokens 下快速提升 MMLU accuracy，例如约 38B tokens 就达到 0.336 左右。

### Q74. 为什么 FineWeb-Edu 是 synthetic data 的例子？

A：因为它用 LLM 生成教育价值标签，再训练分类器过滤数据，这是一种用 synthetic labels 改进真实数据选择的方法。

## Toxic、Personal Data 与 Ethics

### Q75. 为什么要移除 toxic 或 explicit content？

A：为了降低模型生成有害、冒犯性或不适当内容的概率。

### Q76. 移除 toxic content 有什么副作用？

A：模型可能变得不擅长识别或讨论这类文本，过滤也可能误伤某些群体语言。

### Q77. toxic content filtering 有哪些方法？

A：可以用 URL 或文本词表过滤，也可以用训练好的分类器或商业工具。

### Q78. 什么是 Personally Identifiable Information？

A：PII 包括姓名、地址、电话号码、信用卡号、电子邮件等能识别个人的信息。

### Q79. 为什么要处理 PII？

A：为了保护隐私，减少模型记忆和泄露个人信息的风险。

### Q80. PII 处理的理想方式是什么？

A：不是简单删除所有内容，而是在需要时替换成连贯的 synthetic information。

### Q81. 是否应该从 Wikipedia 中移除所有人名？

A：不一定。Wikipedia 中的公开人物信息可能是知识内容的一部分，不能机械套用 PII 规则。

### Q82. representation 问题指什么？

A：训练数据决定模型反映哪些声音、文化和观点；代表性不足会造成偏见、刻板印象和排斥。

### Q83. underrepresentation 有什么后果？

A：模型可能对边缘群体表现差，误判其语言或文化表达，甚至强化偏见。

### Q84. overrepresentation 有什么后果？

A：主流或强势群体观点被过度强化，可能进一步放大现有权力不平衡。

### Q85. 讲义中的 African American dialect 例子说明什么？

A：如果训练和评价只承认标准英语，模型可能把合法方言误判为错误英语。

## Post-Training Data 与 Instructions

### Q86. 预训练模型的一个核心限制是什么？

A：它只是学会预测文本，没有明确学会“用户现在要求我完成什么任务”。

### Q87. instruction tuning 解决什么问题？

A：它提供监督信号，让模型学习如何遵循命令、回答问题并以有帮助的方式行动。

### Q88. instruction tuning 的数据形式是什么？

A：通常是 instruction-response pairs，例如“翻译这句话”和对应翻译答案。

### Q89. instruction tuning 如何提升泛化？

A：通过暴露给许多任务类型和共享指令格式，模型能更好处理未见过的新任务。

### Q90. Super-Natural Instructions 是什么？

A：它是一个大规模 instruction-tuning 数据集，覆盖 1600 多个多样 NLP 任务。

### Q91. Super-Natural Instructions 的关键洞见是什么？

A：接触大量清晰自然语言写成的任务说明，可以提升模型 zero-shot 和 cross-task generalization。

### Q92. 为什么 instruction tuning 更符合人类使用模型的方式？

A：因为用户通常直接给模型自然语言任务说明，而不是只让模型续写文本。

## Synthetic Instruction Data

### Q93. synthetic instruction data 是什么？

A：它是由模型、程序或其他自动化方式生成的 instruction-response 数据，用于训练或调优模型。

### Q94. 生成 synthetic data 的主要方法有哪些？

A：包括 self-generation + verification、distillation from stronger models、evolutionary methods 和 procedural generation。

### Q95. self-generation + verification 是什么？

A：模型自己生成候选数据，再通过规则、验证器或另一个模型检查正确性。

### Q96. stronger model distillation 是什么？

A：用更强模型生成 instruction-response pairs，再用这些数据训练较小或开源模型。

### Q97. evolutionary methods 是什么？

A：通过不断改写、复杂化或变换已有 instruction 来生成更丰富的数据，例如 Evol-Instruct。

### Q98. procedural generation 适合哪些任务？

A：适合代码、数学、结构化任务等可以通过程序自动生成和验证的数据。

### Q99. synthetic instruction data 的主要挑战是什么？

A：质量控制、维持多样性、避免模式坍缩，以及判断何时有可靠 ground truth。

### Q100. 为什么有 ground truth 的任务更适合 synthetic data？

A：如果能自动验证正确性，例如数学和代码，synthetic data 的可靠性和价值会更高。

### Q101. 为什么 synthetic data 需要混入 human data？

A：只用模型生成数据可能导致多样性下降或模型偏差循环放大，human data 可以帮助保持分布多样。

### Q102. Alpaca 数据集说明了什么？

A：它用较低成本生成 52K instruction-response pairs，展示了开源模型可以通过 synthetic instruction data 获得较强指令跟随能力。

### Q103. Alpaca 为什么影响很大？

A：它证明了用更强 LLM 生成数据可以低成本调优 LLaMA，并推动了开源 instruction tuning 数据集的发展。

## Copyright 与 License

### Q104. copyright 保护什么？

A：保护原创并固定在物质形式中的文学、戏剧、音乐和艺术作品，不保护单纯 ideas 或 facts。

### Q105. 英国 copyright 是否需要注册？

A：不需要。版权自动产生，保护门槛很低。

### Q106. copyright 通常持续多久？

A：在英国通常持续作者去世后 70 年。

### Q107. 为什么互联网内容通常不能默认自由使用？

A：大多数互联网内容即使没有明确标注，也默认受版权保护。

### Q108. 使用 copyrighted material 的两条法律路径是什么？

A：获得版权持有者授权，或适用 UK fair dealing 中的特定例外。

### Q109. UK fair dealing 和 US fair use 有什么区别？

A：UK fair dealing 更窄，只限于研究和私人学习、评论、新闻报道、引用、讽刺、文本和数据挖掘等特定目的。

### Q110. license 是什么？

A：License 是 licensor 授予 licensee 使用作品的许可。

### Q111. Creative Commons license 的作用是什么？

A：它允许在特定条件下自由分发和使用版权作品。

### Q112. Terms of Use 为什么也重要？

A：即使作品有开放许可证，平台服务条款也可能额外限制下载或使用方式。

### Q113. YouTube 的例子说明什么？

A：即使视频使用 Creative Commons，YouTube terms of service 也可能禁止下载，因此许可证和平台条款都要看。

## 综合比较题

### Q114. pre-training data 和 post-training data 的区别是什么？

A：Pre-training data 用于学习通用语言和知识；post-training data 用于学习指令跟随、偏好、安全和具体任务行为。

### Q115. heuristic filtering 和 classifier-based filtering 的区别是什么？

A：Heuristic filtering 快速依赖表面规则；classifier-based filtering 更灵活，能学习质量分布，但成本更高。

### Q116. exact deduplication 和 approximate deduplication 的区别是什么？

A：Exact deduplication 删除完全重复内容；approximate deduplication 删除近重复内容，例如模板网页。

### Q117. FineWeb 和 FineWeb-Edu 的区别是什么？

A：FineWeb 是大规模清洗 web corpus；FineWeb-Edu 是从 FineWeb 中进一步筛选出的高教育价值子集。

### Q118. 为什么“质量胜过数量”是 L05 的核心主题？

A：因为低质量大规模数据会浪费训练成本并引入噪声，而筛选后的高质量数据能以更少 tokens 获得更好能力。

### Q119. 为什么数据选择本身可能造成偏见？

A：过滤器和阈值可能系统性排除某些语言、方言、地区或文化内容，从而改变模型学到的世界。

### Q120. 为什么 synthetic data 既有用又危险？

A：它能低成本扩展训练数据，但可能继承 teacher model 错误、降低多样性，并放大已有偏见。

## 考前作答模板

### Q121. 如果考“为什么数据重要”，应该怎么答？

A：先说模型只能学习数据中的知识、语言和偏差；再说数据质量影响能力、成本、泛化和安全；最后提到高质量专有数据是竞争优势。

### Q122. 如果考“data selection pipeline”，应该怎么答？

A：按 language filtering、heuristics、quality filtering、domain filtering、deduplication、toxicity/PII filtering 和 data mixing 说明。

### Q123. 如果考“为什么 deduplication 重要”，应该怎么答？

A：答三点：减少训练成本、防止过拟合或记忆重复内容、提升泛化和评价可靠性。

### Q124. 如果考“FineWeb 如何体现数据选择”，应该怎么答？

A：说明它从 Common Crawl 出发，通过系统调试 filtering heuristics、deduplication 和 educational classifier，证明高质量筛选比盲目扩大数据更重要。

### Q125. 如果考“FineWeb-Edu 如何使用 synthetic labels”，应该怎么答？

A：说明先用强 LLM 给样本文档教育价值打分，再训练轻量分类器大规模过滤，低成本筛出高教育价值文本。

### Q126. 如果考“instruction tuning 加了什么”，应该怎么答？

A：它加了遵循指令的监督信号，让模型知道用户任务和期望行为，而不只是继续预测下一个词。

### Q127. 如果考“synthetic instruction data 的挑战”，应该怎么答？

A：答质量控制、多样性维护、ground truth 可验证性、teacher bias 和 synthetic data collapse。

### Q128. 如果考“数据伦理”，应该怎么答？

A：从 representation、toxicity、PII、copyright 和 license 五个角度说明数据选择不仅影响性能，也影响公平、安全、隐私和合法性。

