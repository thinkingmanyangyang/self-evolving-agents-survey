---

## 结论

回到四个问题:

- **Q1 范式地图**:自进化 agent 沿"进化什么(logits/上下文/记忆/技能/harness/参数)× 学什么信号 × 何时 × 免梯度否 × 巩固机制 × 内化层级"六维展开。**主流(约 130/152)是"外挂层免梯度演进、不进权重"**;只有 < 20 篇真正"巩固进参数"。
- **Q2 解三难**:权重冻结 → 大多绕开(外挂层);梯度冲突 → 几何共识/Fisher 投影/逐参数符号冻结;灾难性遗忘 → 隔离 / merging / **on-policy 隐式 KL-min(RL's Razor 机理)** / 保持正则。
- **Q3 三层协同**:记忆(状态)、技能(专长)、harness(脚手架)经 externalization 统一为"外化"三类,由 harness 协调;但三者都**受 base 能力封顶**,harness 还有"脚手架饱和"。
- **Q4 idea 定位**:「探索-巩固」横跨"内化分水岭",落在"把在线经验巩固进参数"这一**全领域最大空白**;探索侧有 jitrl 近同款、巩固侧有 agent_dice 同款 + auto_dreamer 可学习范本,但"在线探索→离线可学习巩固进参数"的完整闭环**尚无人做通**——这是机会。

**三条贯穿性结论**:① 不进权重的自进化受 base 封顶(SICA 脚手架饱和、技能/记忆迁移上限);② on-policy/RL 比 SFT 少遗忘是反复验证的机理(但"输出 KL≠内部能力保留",别只用 KL 当度量);③ **巩固有三大坑**(巩固坏经验 / 覆盖旧能力 / 过度抽象),任何"可学习巩固"必须内建过滤+正则+安全闸。

## 局限与未尽事项(诚实标注)

- **未深读**:brief 档(背景奠基 <2024-06:Voyager/Reflexion/MemGPT/EWC/PCGrad 等 14 篇 + 早期 AIOS)仅记录不深读。
- **未开源 / 未完整克隆**:按"重思想、不设代码门槛",仅对部分关键篇 clone(skillgrad/skillx/autoskill/skill0_zero/dual_arch 等已 clone 入 `resource/repos/`);大量 2026 新作未开源或正文未给链接,已逐篇在 ⑦ 字段标"未找到/待核",代码需手动追。
- **待核项**:少数 2026 上半年新作的机构/框架/代码链接标〔待核〕;`mech_analysis_cf` 声称在闭源 API 上取梯度/CKA,技术上存疑,已标【推断·存疑】;`procmem` 真名 Skill-Pro、`evoskills`=CoEvoSkills、`bench_tales` 实有 arXiv 2504.14128 等已修正。
- **覆盖判定**:P5 核查后判七线饱和,补齐了 0 覆盖的"安全/misevolution"子线;近重复工作(MemRL/SkillRevise/HASP 等)已判同质冗余、未凑数。

## 局限与统计

- 检视候选 152;深读 **152 篇**:deep 83(全 8 字段 + 三层精读 + 机制速览6轴 + top-2 图)+ light 69(轻量靶心 + top-2 图)。
- 主题线分布(主归属):L1 免梯度 23 · L2 记忆 29 · L3 技能 28 · L4 反思内化 20 · L5 巩固防遗忘 20 · L6 harness 18 · L7 综述评测安全 14。
- 配图:`analysis/figs/` 共 ~292 张 top-2 精华图(每篇 1–2 张,整页渲染 + 标注原文图号)。
- PDF 落 `resource/papers/`;逐篇笔记 `analysis/<key>.md`(152 个)。
- 验证:P3 核 34 高危 id(31真/5修正/0伪,负控 404 验证);P4.5 抽检 14 篇全过(含 28 图核验);零编造。

## 附录索引

- **深读队列 + 全部 key↔arXiv↔代码↔框架↔处理档**:`_state/verified_shortlist.md`。
- **候选主表(含相关性/主题线/命中来源/status)**:`_state/candidates_master.md`。
- **逐篇笔记**:`analysis/<key>.md`(152,每篇含 top-2 图);**配图**:`analysis/figs/`。
- **验证记录**:`_state/verify/V1.md`、`_state/verify/P45_spotcheck.md`;**完整性核查**:`_state/P5_coverage.md`。
- **机制文件**:`PLAN.md`(流程+运行机制)、`ANALYSIS_PROMPT.md`(三层精读模板)。
- **背景奠基(brief,不深读)**:Voyager · Reflexion · ExpeL · MemGPT/Letta · Generative-Agents · Self-Refine · EWC · PCGrad · GPM · O-LoRA · AWM · Symbolic-Learning · AIOS · in-context-RL/AD。
