# L22 Ethics and Bias 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。重点覆盖 AI ethics 的基本问题、LLM 风险分类、bias harms、WEAT/BBQ/counterfactual testing、bias mitigation，以及为什么技术修复必须配合制度和部署治理。

## 高频核心题

### Q1. AI ethics 研究什么？

A：AI ethics 研究 AI 系统设计、开发和部署中产生的道德问题，例如谁受益、谁受害、谁决策、谁负责。

### Q2. 为什么 AI systems 不是中立工具？

A：它们会编码创建者、训练数据和部署组织的价值、优先级和盲点。

### Q3. 为什么 AI ethics 不是 checklist？

A：伦理风险贯穿系统生命周期，需要持续审查、监测和调整，而不是上线前勾选一次。

### Q4. 为什么 LLM 的风险不同于传统窄 AI？

A：LLM 具有通用性、流利性和规模化部署能力，同一模型可被用于学习、医疗、诈骗或宣传等多种场景。

### Q5. LLM harms 可以影响哪些层面？

A：个体用户、部署组织和社会整体都可能受到同一输出的不同形式伤害。

### Q6. Weidinger et al. 的风险分类有什么作用？

A：它把语言模型风险系统化为多个类别，使研究者能更精确讨论 LLM 可能如何伤害谁。

### Q7. 本讲最核心的伦理问题可以概括为什么？

A：谁受益、谁受害、谁决定、谁负责。

### Q8. 为什么弱势群体更容易受到 AI harm？

A：训练数据、机构实践和社会结构中的不平等会被模型复制、放大，并在部署中造成差异化影响。

## LLM 风险分类

### Q9. Risk Category 1 是什么？

A：Discrimination, hate speech and exclusion，即歧视、仇恨言论和排斥。

### Q10. Category 1 的机制是什么？

A：LLM 从训练数据中复制并放大偏见、刻板印象和排斥性语言。

### Q11. Allocational harm 是什么？

A：系统在资源、机会或服务分配中对某些群体不公平，例如招聘、贷款、教育或医疗机会。

### Q12. Representational harm 是什么？

A：系统以刻板、贬低、缺失或错误方式呈现某些群体，强化社会从属和偏见。

### Q13. Allocational harm 和 representational harm 的区别是什么？

A：allocational harm 影响实际资源和机会；representational harm 影响群体如何被描述、想象和社会定位。

### Q14. 招聘简历例子说明了什么？

A：即使简历内容受控，只改变姓名等人口统计信号，模型评分也可能随性别或种族暗示变化。

### Q15. Persistent Anti-Muslim Bias 例子说明什么？

A：LLM 可能在类比或生成中把某些宗教群体与负面刻板印象联系起来，造成 representational harm。

### Q16. Risk Category 2 是什么？

A：Information hazards，即模型输出训练数据中存在或可推断出的私密或安全关键知识。

### Q17. Information hazards 包括哪些 harm？

A：个人数据泄露、dual-use 知识生成、membership inference 或 training data extraction。

### Q18. 为什么 PII removal 不能完全解决隐私问题？

A：识别和删除所有个人信息成本高且不可靠，模型还可能记忆罕见训练样本。

### Q19. Training data extraction attack 是什么？

A：攻击者通过查询语言模型恢复训练数据中的原始文本片段，可能包含姓名、邮箱、地址等敏感信息。

### Q20. 为什么大模型可能更容易泄露训练数据？

A：更大模型容量更强，可能记住更多罕见或重复样本，尤其在过拟合或数据未清理时。

### Q21. Risk Category 3 是什么？

A：Misinformation harms，即模型给错误、误导、无意义或低质量信息较高概率并生成出来。

### Q22. Misinformation harms 的机制是什么？

A：模型生成看似可信但实际错误的科学、医疗、法律或事实声明。

### Q23. Epistemic trust harm 是什么？

A：当 AI 生成内容大量混入信息生态，用户更难判断什么是真的，从而削弱对知识和媒体的信任。

### Q24. Risk Category 4 是什么？

A：Malicious uses，即人类故意使用 LLM 造成伤害。

### Q25. Malicious uses 包括哪些例子？

A：低成本生成宣传材料、诈骗文本、钓鱼邮件、恶意代码、武器相关说明、非法监控或审查。

### Q26. Risk Category 5 是什么？

A：Human-computer interaction harms，即人机交互方式本身带来的伤害。

### Q27. Anthropomorphism harm 是什么？

A：用户把模型误认为有理解、人格或情感，从而过度信任或产生不健康依赖。

### Q28. Sycophancy 是什么？

A：模型倾向于迎合用户、说用户想听的话，而不是给出真实、有益或安全的回答。

### Q29. 哪些人群在 HCI harms 中特别脆弱？

A：心理健康脆弱者、老人、儿童和依赖系统获得建议的人。

### Q30. Risk Category 6 是什么？

A：Environmental and socioeconomic harms，即环境和社会经济伤害。

### Q31. Category 6 包括哪些 harm？

A：训练大模型的碳足迹、知识工作自动化带来的就业冲击、权力集中和数字鸿沟。

## Measuring Bias

### Q32. 为什么要测量 bias？

A：如果不能测量，就很难比较模型、定位风险、评估 mitigation 是否有效。

### Q33. Implicit Association Test 的直觉是什么？

A：人类或模型对某群体和某属性之间的联想强度可通过反应或相似度差异体现。

### Q34. WEAT 是什么？

A：Word Embedding Association Test，用词向量几何距离衡量目标词组与属性词组之间的联想差异。

### Q35. WEAT 使用哪四类词集？

A：两个 target sets，例如男性名和女性名；两个 attribute sets，例如 career words 和 family words。

### Q36. WEAT 的单词 association score 怎么理解？

A：`s(w, X, Y)` 比较词 `w` 与属性集 `X` 的平均 cosine similarity 和与 `Y` 的平均 similarity 差。

### Q37. WEAT 分数为正表示什么？

A：目标词更接近属性集 `X` 而不是 `Y`。

### Q38. WEAT 的优点是什么？

A：简单、可量化，能检测 embedding 中某些预定义 stereotype association。

### Q39. WEAT 的局限是什么？

A：依赖研究者预先定义词表，只测内部表示，不一定预测真实部署行为。

### Q40. Counterfactual input testing 是什么？

A：构造只在人口统计信号上不同的成对输入，观察模型输出是否发生系统性差异。

### Q41. Counterfactual testing 的例子是什么？

A：同一份简历只替换候选人姓名，用模型评分差异检测招聘偏见。

### Q42. Counterfactual testing 的优点是什么？

A：更贴近具体部署场景，可以直接测量某个系统在某个任务中的差异行为。

### Q43. Counterfactual testing 的难点是什么？

A：需要谨慎构造匹配输入，避免引入额外 confounds。

### Q44. Benchmark datasets 如何测 bias？

A：用标准化、大规模样本比较模型在不同群体、语境和问题类型上的表现。

### Q45. BBQ 是什么？

A：Bias Benchmark for QA，用 ambiguous 和 disambiguated contexts 测试 QA 模型是否依赖 stereotype。

### Q46. BBQ 的 ambiguous context 测什么？

A：当信息不足时，模型是否用 stereotype 选择答案。

### Q47. BBQ 的 disambiguated context 测什么？

A：当有足够信息时，模型是否仍被 bias 影响而覆盖正确答案。

### Q48. 为什么单一 bias metric 不够？

A：单一指标可能被 gaming，漏掉其他偏见类别，并反映设计者文化假设而非受害者经验。

### Q49. Good practice 推荐怎样测 bias？

A：结合 structured benchmark、部署领域 counterfactual testing 和 open-ended output 的 human evaluation。

## Bias Mitigation

### Q50. Bias mitigation 为什么困难？

A：bias 多维、上下文相关，干预常在一个指标上有效却迁移失败，甚至加重其他群体或任务的偏差。

### Q51. Bias Whack-a-Mole Problem 是什么？

A：降低 metric A、group X、task T 上的 bias 后，metric B、group Y 或 task T+1 上的 bias 仍存在或变糟。

### Q52. Preprocessing mitigation 是什么？

A：在训练前处理数据，例如去除、重加权或平衡有偏内容。

### Q53. Preprocessing 的优点是什么？

A：可以针对明确偏见来源做数据层干预，减少模型接触某些有害模式。

### Q54. Preprocessing 的局限是什么？

A：劳动密集，不能清除所有偏见，且大模型中数据层 debiasing 的边际收益可能下降。

### Q55. In-training mitigation 包括什么？

A：curated data fine-tuning、adversarial training、RLHF 等训练时干预。

### Q56. In-training mitigation 的优点是什么？

A：可以显著减少一些 overt bias 和显性有害行为。

### Q57. In-training mitigation 的局限是什么？

A：可能留下 subtler biases，并且 reward model 会编码 annotator 的文化视角。

### Q58. 为什么 annotator demographics 重要？

A：标注者决定什么被视为偏见、有害或可接受，不同文化和群体可能判断不同。

### Q59. Intra-processing mitigation 是什么？

A：在模型内部或推理过程中改变表示、激活或决策流程来降低偏见。

### Q60. Post-processing mitigation 是什么？

A：在输出阶段过滤、重写或拒绝有偏/有害结果。

### Q61. Post-processing 的优点是什么？

A：部署容易、成本较低，可快速加入系统作为防护层。

### Q62. Post-processing 的局限是什么？

A：容易被改写绕过，只压制表现症状，不解决底层模型和数据偏见。

### Q63. 为什么 mitigation 必须 domain-specific？

A：不同部署场景的 harm、群体、评价标准和责任不同，通用 debiasing 难以保证具体场景安全。

### Q64. 为什么技术修复需要制度补充？

A：偏见治理还需要部署后审计、受影响社区反馈、监管披露和明确问责。

### Q65. Diversity in the pipeline 为什么重要？

A：数据、标注者、评测和开发团队中代表性不足的群体，往往也是 mitigation 表现最差的群体。

## 综合比较题

### Q66. Embedding metric、counterfactual testing 和 benchmark 的区别是什么？

A：embedding metric 测内部表示；counterfactual testing 测具体场景中的输出差异；benchmark 提供标准化大规模比较。

### Q67. Bias measurement 和 bias mitigation 的关系是什么？

A：measurement 用来发现和评估 bias；mitigation 用来减少 harm，但 measured improvement 不保证真实世界 harm 消失。

### Q68. Representational harm 是否会导致 allocational harm？

A：可能会。长期刻板呈现会影响社会判断，并在招聘、服务和资源分配中转化为实际不公平。

### Q69. Information hazard 与 misinformation harm 的区别是什么？

A：information hazard 关注私密或危险信息泄露；misinformation harm 关注错误或低质量信息被可信生成。

### Q70. Malicious use 与 accidental harm 的区别是什么？

A：malicious use 是人有意滥用模型；accidental harm 可由模型错误、部署失误或用户过度信任造成。

## 考前作答模板

### Q71. 如果考“AI ethics 的核心问题”，应该怎么答？

A：答 AI 系统不只是技术工具，还涉及谁受益、谁受害、谁决策、谁负责；伦理需要贯穿系统生命周期。

### Q72. 如果考“Weidinger 风险分类”，应该怎么答？

A：列出 discrimination/exclusion、information hazards、misinformation、malicious uses、HCI harms、environmental/socioeconomic harms，并给每类一个例子。

### Q73. 如果考“WEAT”，应该怎么答？

A：说明 WEAT 用 target sets 和 attribute sets 的 cosine similarity 差衡量 embedding association，但依赖预定义词表且不一定预测应用偏见。

### Q74. 如果考“BBQ”，应该怎么答？

A：说明 BBQ 是 QA bias benchmark，用 ambiguous context 测 stereotype reliance，用 disambiguated context 测 bias 是否覆盖正确答案。

### Q75. 如果考“为什么 bias 不能只靠技术解决”，应该怎么答？

A：说明 bias 干预泛化差、受数据和标注文化影响，真实 harm 需要部署审计、社区反馈、制度问责和领域化治理。

