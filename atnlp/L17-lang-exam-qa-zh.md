# L17 LLMs and Automata 考试复习 QA

这份是一套面向考试的总复习问答，不按页码整理，而是按整份讲义的知识结构整理。根据 `examguide.pdf`，Lecture 17 的 slide 28 到最后不考；本文件聚焦可考核心：autoregressive language modelling、RNN language models、finite-state machines、formal languages、RNNs as recognisers、Turing machines、two-stack pushdown automata，以及 unbounded precision RNN 的表达能力。

## 高频核心题

### Q1. 为什么要用 formal methods 研究 neural language models？

A：形式化研究能帮助理解模型理论能力、限制和与传统计算模型的关系，而不只看 benchmark 表现。

### Q2. 本讲关注的核心问题是什么？

A：神经语言模型，尤其 RNN，在形式语言识别和计算能力上相当于什么传统自动机。

### Q3. Autoregressive language model 如何分解序列概率？

A：用 chain rule 把序列概率分解为每个 token 在前缀条件下的概率乘积，并包括 EOS 概率。

### Q4. 典型公式是什么？

A：`p(y) = p(EOS | y) product_t p(y_t | y_<t)`。

### Q5. Representation-based LM 的基本形式是什么？

A：先把前缀映射为 hidden representation `h(prefix)`，再用 softmax 给出下一个 token 分布。

### Q6. 典型公式是什么？

A：`p(y_t | prefix) = softmax(E h(prefix))_{y_t}`。

### Q7. RNN 为什么适合作为语言模型？

A：它递归更新 hidden state，使当前状态总结前缀信息，并用于预测下一个 token。

### Q8. RNN 的基本状态更新是什么？

A：`h_{t+1} = f(h_t, x_t)`。

## RNN Language Models

### Q9. Elman RNN 的状态更新形式是什么？

A：`h_{t+1} = sigma(Uh_t + V r(x_t) + b)`，其中 `r(x_t)` 是输入 token 表示。

### Q10. RNN LM 如何生成下一个词分布？

A：用当前 hidden state 通过输出矩阵和 softmax 计算词表上概率分布。

### Q11. RNN LM 如何给完整句子赋概率？

A：逐步乘上每个 token 在前缀条件下的概率，再乘 EOS 概率。

### Q12. RNN LM 与 n-gram LM 的共同点是什么？

A：二者都用条件概率的乘积给句子赋概率。

### Q13. RNN LM 与 n-gram LM 的关键区别是什么？

A：n-gram 只看固定长度历史；RNN hidden state 理论上可编码整个前缀。

### Q14. 为什么 RNN 不需要像 HMM 那样对 hidden states 求和？

A：给定输入前缀，RNN 的 hidden state 是确定性更新的。

### Q15. RNN hidden state 在形式研究中为什么重要？

A：它可被视为模型内部状态，类似自动机状态，但通常是连续向量。

### Q16. 连续 hidden state 与有限状态有什么区别？

A：连续空间理论上有无限多状态，而 finite-state machine 只有有限个离散状态。

## Finite-State Machines 与 Formal Languages

### Q17. Finite-state machine 是什么？

A：由有限状态集合、初始状态、输入 alphabet、转移函数和接受状态组成的自动机。

### Q18. FSM 的组成有哪些？

A：`Q` 状态集、`q0` 初始状态、`Sigma` alphabet、`delta` 转移函数、`F` 接受状态集合。

### Q19. FSM 的转移函数表示什么？

A：`delta(q, y) = q'` 表示在状态 `q` 读到符号 `y` 后进入状态 `q'`。

### Q20. FSM 如何接受一个字符串？

A：从初始状态依次读入字符串符号，最终落在接受状态集合 `F` 中则接受。

### Q21. Formal language 是什么？

A：某个 alphabet 上字符串的集合。

### Q22. 形式系统 `F` 的语言类如何表示？

A：`L(F) = {L(A) | A in F}`，表示该形式系统中所有机器可识别的语言集合。

### Q23. FSM 能识别哪类语言？

A：regular languages。

### Q24. 为什么 regular languages 对自然语言不够？

A：自然语言可能需要长距离依赖、嵌套结构和计数能力，超出有限状态机表达力。

### Q25. RNN 如何被看作 recogniser？

A：读完整个字符串后，用最终 hidden state 判断该字符串是否属于某语言。

### Q26. RNN recogniser 的输出如何二值化？

A：可用一个分类器或阈值把最终状态映射为 accept/reject。

### Q27. 为什么可以把 RNN 和自动机比较？

A：二者都逐步读取输入并更新状态，最后基于状态做接受/拒绝判断。

### Q28. RNN 与 FSM 的关键差别是什么？

A：FSM 状态有限离散；RNN 状态是连续向量，理论上可编码无限精细信息。

## Computational Power

### Q29. 研究 RNN power 时需要区分什么假设？

A：需要区分 unbounded precision/computation 与 bounded precision/computation。

### Q30. 本复习集覆盖哪种假设？

A：覆盖 exam guide 可考范围内的 unbounded precision/computation 结果，不覆盖 slide 28 之后的 bounded precision 细节。

### Q31. Unbounded precision 指什么？

A：RNN hidden state 中的实数可以用任意精度表示，不受有限机器位数限制。

### Q32. Unbounded computation 指什么？

A：允许足够计算步骤和精度进行理想化计算，而不是受实际硬件限制。

### Q33. 在 unbounded precision 下，RNN 的理论能力如何？

A：可以达到 Turing machine 级别的计算能力。

### Q34. Church-Turing thesis 的相关直觉是什么？

A：任何可有效计算的函数都可由 Turing machine 计算，因此 Turing machine 代表通用计算能力。

### Q35. Turing machine 为什么比 FSM 强？

A：Turing machine 有无限 tape 作为外部 memory，而 FSM 只有有限状态。

### Q36. 为什么 memory 是计算能力的关键？

A：许多语言需要记录任意长历史或计数，有限状态无法做到。

### Q37. Pushdown automaton 比 FSM 强在哪里？

A：PDA 有 stack memory，可识别一些需要嵌套或匹配的 context-free languages。

### Q38. Two-stack PDA 的重要性是什么？

A：两个 stack 的 PDA 与 Turing machine 等价，具备通用计算能力。

## Two-Stack PDA

### Q39. Two-stack PDA 有哪些核心组件？

A：状态集、输入 alphabet、stack alphabet、转移函数、初始状态、接受状态和拒绝状态。

### Q40. Two-stack PDA 的转移依赖什么？

A：当前状态、可选输入符号，以及两个 stack 顶部符号。

### Q41. Two-stack PDA 的转移输出什么？

A：新状态，以及对两个 stack 的 push/pop 操作。

### Q42. 为什么两个 stack 等价于 Turing tape？

A：一个 stack 可表示 tape 左侧内容，另一个表示当前及右侧内容，通过 push/pop 模拟 tape head 左右移动。

### Q43. 本讲为什么用 two-stack PDA 证明 RNN 能力？

A：如果 RNN 能模拟 two-stack PDA，就能模拟 Turing machine。

### Q44. RNN 模拟 PDA 的关键想法是什么？

A：把 stack 内容编码进 hidden state 的实数中，并用 RNN 更新模拟 push/pop。

### Q45. 为什么用实数编码 stack 依赖 unbounded precision？

A：任意长 stack 需要在实数小数位中保留任意多信息，实际有限精度做不到。

## Stack Encoding

### Q46. 讲义中如何编码二进制 stack？

A：把 stack 符号编码成十进制小数位，例如用 `1` 表示符号 0，用 `3` 表示符号 1。

### Q47. 为什么不用 `0` 和 `1` 直接作为小数位？

A：尾随 0 会产生歧义，例如不同长度 stack 可能有相同小数表示。

### Q48. Stack 顶部在小数表示中放在哪里？

A：通常放在靠近小数点的位置，便于通过乘除 10 模拟 push/pop。

### Q49. Pop 操作的直觉是什么？

A：去掉最靠近小数点的编码位，并把后续位左移。

### Q50. Push 操作的直觉是什么？

A：把新符号编码放到最靠近小数点的位置，并把原 stack 右移。

### Q51. 讲义中 pop 0 的形式是什么？

A：若顶部编码为 `1`，可用类似 `10 * stack - 1` 的操作去掉顶部。

### Q52. 讲义中 pop 1 的形式是什么？

A：若顶部编码为 `3`，可用类似 `10 * stack - 3` 的操作去掉顶部。

### Q53. 讲义中 push 0 的形式是什么？

A：可用类似 `(1/10) * stack + 1/10` 的操作把编码 `1` 放到顶部。

### Q54. 讲义中 push 1 的形式是什么？

A：可用类似 `(1/10) * stack + 3/10` 的操作把编码 `3` 放到顶部。

### Q55. 为什么 RNN 可以模拟这些操作？

A：在理想化设置下，RNN 的连续 hidden state 和非线性可实现所需的状态检测和线性更新。

## Configuration Encoding

### Q56. PDA configuration 包含什么？

A：当前控制状态、两个 stack 内容，以及每个 stack 顶部符号等信息。

### Q57. RNN hidden state 如何编码 configuration？

A：用若干维度分别存储两个 stack 的实数编码、top-of-stack 标记和当前 PDA 状态 one-hot。

### Q58. 为什么需要 top-of-stack 标记？

A：转移函数要根据 stack 顶部符号选择下一步操作，显式标记便于实现条件更新。

### Q59. 为什么需要 current state one-hot？

A：PDA 转移依赖当前离散状态，one-hot 可让 RNN 表示控制状态。

### Q60. 这种构造是训练出来的吗？

A：讲义强调这是理论构造，证明存在某组 RNN 参数能模拟 PDA，不是说实际训练必然学到。

### Q61. “RNNs are Turing complete” 应如何理解？

A：在理想化 unbounded precision/computation 条件下，RNN 可以模拟 Turing machine。

### Q62. 这个结果是否说明实际 RNN 总能解决任意计算问题？

A：不能。实际模型受有限精度、训练、泛化和计算资源限制。

### Q63. 为什么理论能力和实际可学习性不同？

A：存在某组参数不代表训练算法能找到，也不代表模型能从有限数据泛化。

### Q64. 为什么本讲仍然有价值？

A：它澄清了架构理论表达力，并提示实际限制常来自训练、精度和资源，而非单纯形式能力。

## 概念对比题

### Q65. FSM、PDA、Turing machine 的主要区别是什么？

A：FSM 只有有限状态；PDA 有 stack；Turing machine 有可读写移动的无限 tape。

### Q66. One-stack PDA 和 two-stack PDA 的区别是什么？

A：one-stack PDA 识别 context-free languages；two-stack PDA 可模拟 Turing machine。

### Q67. RNN hidden state 和 PDA stack 的区别是什么？

A：RNN hidden state 是固定维连续向量；PDA stack 是离散且可增长的 memory。

### Q68. 为什么 RNN 能用固定维度模拟无限 stack？

A：在 unbounded precision 假设下，一个实数的小数位可编码任意长符号序列。

### Q69. 这个编码为什么不现实？

A：实际计算机只能用有限精度表示实数，长 stack 信息会因舍入和噪声丢失。

### Q70. Formal language view 给 NLP 什么启发？

A：它用语言类和自动机能力分析模型能否学习长距离依赖、嵌套和计数等结构。

## 考前作答模板

### Q71. 如果考“RNN LM 如何给句子赋概率”，应该怎么答？

A：写出 autoregressive chain rule，并说明 hidden state 总结前缀、softmax 预测下一个 token。

### Q72. 如果考“FSM 的定义”，应该怎么答？

A：列出 `Q, q0, Sigma, delta, F`，说明读入符号后状态转移，最终在接受状态则接受。

### Q73. 如果考“RNN 与 FSM 的关系”，应该怎么答？

A：二者都逐步读输入并更新状态；FSM 状态有限离散，RNN 状态连续，理论上可更强。

### Q74. 如果考“为什么 two-stack PDA 重要”，应该怎么答？

A：两个 stack 可模拟 Turing tape，因此 two-stack PDA 等价于 Turing machine；若 RNN 能模拟它，就具备 Turing completeness。

### Q75. 如果考“unbounded precision RNN 的结论”，应该怎么答？

A：在无限精度和理想计算条件下，RNN 可用实数 hidden state 编码 stacks 并模拟 two-stack PDA，因此与 Turing machine 等价；实际模型不一定具备这种可用能力。

