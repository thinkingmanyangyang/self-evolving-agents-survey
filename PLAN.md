# 自进化 / 持续学习 LLM Agent 全景调研 — 执行计划与运行机制 (canonical)

> **任务**:系统调研「持续学习 / 自进化 / Skill / Harness 的 LLM Agent」,产出全景地图。
> **范围**:四主轴(持续学习 Continual Learning · 自进化 Self-Evolving · 技能 Skill · 脚手架 Harness)+ 记忆(Memory)作贯穿底座;用户「探索-巩固」idea 作为地图内一个具体落点,额外做对标分析。
> **独立性**:全新独立课题,**不绑 TSRD/OPD**(忽略 TSRD,只对标"探索-巩固"这一 idea)。
> **工作模式**:大任务——先设计+用户确认,再**全程自主执行,执行期不提问/不 human-in-loop**。
> **时间窗**:**近 2 年(2024-06 ~ 2026-06)为主——只深读这 2 年内的论文**;更早的奠基作(Voyager/Reflexion/Generative Agents/CLS 等)仅作背景简述、不进深读队列(除非被反复用作直接基线)。
> **工作目录**:`D:\claude_code_workspace\mtp_opd`;本调研独立子目录:`resource/research/survey_agent_self_evolving/`。
> **启动日期**:2026-06-04。**状态**:PLAN 已制定,**待用户确认后执行 P1**。

---

## 0. 锚定与边界
- 做**全景地图**,不是只围绕单一 idea;但"探索-巩固"(在线免梯度 logit 调制探索 + 离线几何共识巩固)是关心的落点,P6 给它专门一节对标。
- **不扯 TSRD**:本调研自成一体;"探索-巩固"对标只针对该 idea 本身(支撑/竞品/缺口/创新空间),不引入 TSRD 话题。

## 1. 调研要回答的核心问题
- **Q1 范式地图**:自进化/持续学习 agent 有哪几类范式?(免梯度 test-time vs 梯度参数更新 vs 混合;记忆/技能/巩固/反思各占什么生态位)
- **Q2 解三难**:每类范式如何解决——部署后**权重冻结**、多任务**梯度冲突**、**灾难性遗忘**?
- **Q3 三层协同**:记忆层、技能层、harness/脚手架层如何协同支撑 agent 持续变强?
- **Q4 idea 定位**:"探索-巩固"在地图中的位置——支撑者/竞品/可借组件/缺口与创新空间?

## 2. 主题线 Taxonomy(7 条)
- **L1 免梯度 · test-time 持续学习**:部署后不改权重的在线适应(logit/激活调制、in-context RL、memory-based 适应、O(1) 策略干预)。种子:JitRL、探索-巩固在线期。
- **L2 Agent Memory(贯穿底座)**:参数式 / 检索式 / **生成式潜在**记忆;读写/触发/遗忘;memory trigger+weaver。种子:MemGen;对标 ExpeL/AWM/Mem0/Generative Agents。
- **L3 经验→技能演进(Skill)**:技能发现/抽象/复用/共享/优化;技能库治理(去重/提质/版本/跨用户同步)。种子:SkillClaw、SkillOpt;谱系 Voyager。
- **L4 反思 · 自我纠错 · 经验内化**:反思机制、回溯重试、经验→SFT/蒸馏内化、Pass@k。种子:LEAFE;对标 Reflexion/Early Experience/Self-Refine。
- **L5 巩固 · 防遗忘 · 知识更新**:stability-plasticity;梯度手术/几何共识、model merging、CLS、KL/自然梯度投影、参数融合。种子:Agent-Dice、探索-巩固离线期。
- **L6 Agent Harness / 脚手架**:把 LLM 包成 agent 的执行框架(工具调用+规划+记忆循环),**重点是 harness 如何承载/支撑 agent 自进化**;含"技能层嫁接到多种 harness"(SkillClaw 集成 OpenClaw/Claude Code/Hermes 等)。对标 OpenHands/SWE-agent/AutoGPT 类。〔界定:agent 执行脚手架,非训练框架〕
- **L7 自进化综述 + 评测环境**:领域综述/分类法 + benchmark(WebArena/Jericho/GUI/tool-use/终身学习/长程 Pass@k)。

## 3. 纳入/排除 + 相关性
- **不设代码门槛**(重思想);有代码记链接+框架/harness,不强制 clone。
- **相关性 High**:直接做自进化/持续学习/记忆/技能/harness 的核心机制。**Med**:相关外围(诊断/单一组件/评测)。**Low**:仅边缘。
- 排除:与"agent 持续学习/自进化"无关的纯静态 agent 应用、与记忆/技能/巩固无关的通用 RL。

## 4. 来源 + 检索词库
来源:arXiv · OpenReview(ICLR/NeurIPS/COLM) · ACL Anthology · HuggingFace papers · 大厂/机构(NUS、SJTU、OPPO、阿里 AMAP、字节、清华等 agent 团队)· **GitHub(harness 生态)**。
词库:continual/lifelong learning LLM agent · self-evolving/self-improving agent · test-time / training-free RL · gradient-free adaptation · agent memory (generative/latent/episodic/parametric/retrieval) · memory trigger/weaver · skill library/acquisition/discovery/evolution/reuse · experience replay/distillation/internalization · reflective experience · catastrophic forgetting · stability-plasticity · gradient surgery / geometric consensus / model merging · agent harness/scaffold/framework · Reflexion/ExpeL/Voyager/AWM/Mem0/Generative-Agents。

## 5. 调研流程(六阶段;每阶段 输入→动作→产出)
1. **P0 脚手架**(本文件 + 目录)。✅(待确认)
2. **P1 发现**:并行子代理 A1–A7(主题线×来源)撒网 → 各写 `_state/wave1/A*.md` → 主控合并去重 → `_state/candidates_master.md`。目标候选 ≥80。**7 篇种子先入表。**
3. **P2 粗筛**:相关性 High/Med/Low 打标(随合并)。
4. **P3 验证**:WebSearch 快照交叉验证论文真伪(id↔标题)+ **下载 PDF 字节核验**(防 WebFetch 摘要污染);代码链接可达性 → `_state/verify/V*.md`。
5. **P3.5 固化**:合并 → 回写 master status → 产出 `_state/verified_shortlist.md`(深读队列,带处理档 deep/light + status)+ `_state/rejected.md`。
6. **P4 深读**:按 shortlist 逐篇,下载真实 PDF→`resource/papers/<key>.pdf`,**严格按 `ANALYSIS_PROMPT.md`(三层递进+结构化抽取)写 `analysis/<key>.md`**,并**提取 top-2 最能表现精华的图(方法主图/动机图/核心发现图)→ `analysis/figs/<key>_fig<N>.png`** 在笔记内引用+一句话说明。**幂等**:`analysis/<key>.md` 已存在则跳过;完成置 status=analyzed。
7. **P4.5 抽检**:随机抽若干篇核对"机制类型/数据集/方法"是否真见于 PDF 正文;编造即打回。
8. **P5 引文扩展 + 完整性核查**:从已读核心篇引用二次撒网 + 用综述对比找缺口 → 回灌 → 迭代至"连续两轮无新增 High"。
9. **P6 汇总**:合并 `analysis/*.md` → `SURVEY_self_evolving_agents.md`,含 ①全景 taxonomy 总表 ②L1–L7 各线综述 ③**「探索-巩固」idea 对标+缺口+创新空间**。

## 6. 每篇深读分析模板(三层递进 + 结构化抽取)
**统一遵循 `ANALYSIS_PROMPT.md`(融合升华版,P4 所有深读代理必读)。** 概要:
- **三层递进**:① 一眼看懂(TL;DR + 最巧的一步) ② 为什么做(背景/具体痛点/相关工作不足/动机链/与最近邻Δ,写透) ③ 怎么做+靠不靠谱(方法流水线/逐组件必要性+消融/公式直觉/实验证据+具体数字+baseline公平性/假设与失效边界/祛魅真货vs营销)。
- **结构化抽取(必填,供全景汇总)**:🎯 机制速览 6 轴(学什么信号/改什么/何时改/免梯度?/记忆-技能生命周期/防遗忘) + 代码·框架/harness + 资源成本 + 🎯 对"探索-巩固"对标(支撑/竞品/可借组件/缺口) + 开放问题。
- **防幻觉三标注**:【原文】事实 /【推断】有据判断(附依据)/【待核】;关键论断带原文锚点(§/图/表);**绝不编造**,基于真实 PDF 正文。
- **deep 全套 / light 简版**(仅 头部+第一层+机制速览6轴+假设一句+对标一句)。

## 7. ★ 持续稳定运行机制(长跑不跑偏、不忘上下文的保证)

### 7.1 一切落盘、即时回写(核心原则)
- **进度只认磁盘文件,不依赖对话上下文**。真值载体:本 `PLAN.md` 第 11 节进度表 + `_state/candidates_master.md` + `_state/verified_shortlist.md` + `analysis/*.md`。
- **每完成一波/一阶段,立即回写**对应状态文件与进度表,再进入下一步;严禁只把进度留在对话里。
- 即使 context 被压缩或换新会话,也能纯靠磁盘文件完整恢复。

### 7.2 状态机
- `candidates_master.md` 设 **status 列**:`new → screened → verified / rejected → analyzed`。
- `verified_shortlist.md` = P4 **唯一工作队列**,含 处理档(deep/light)+ status(pending→analyzed)。
- P3.5 把验证结果回写 master;P4 每篇完成把 shortlist 行置 analyzed。

### 7.3 断点续跑协议 + 恢复检查清单(任何中断点照此恢复)
1. 读本 `PLAN.md` 第 11 节进度表,确定停在哪一阶段。
2. 若在 P1–P3.5:读 `_state/` 下对应波次文件,补未完成的子任务。
3. 若在 P4:读 `verified_shortlist.md`,**Glob `analysis/*.md` 得已完成 key**,处理 status≠analyzed 且文件不存在的行(幂等)。
4. 若在 P5/P6:读全部 `analysis/*.md` 与 `_state/`,继续扩展或汇总。
5. 恢复后先回写进度表,再继续。

### 7.4 分波并行 + 去重守恒
- 每波多个子代理**各写独立文件**(A1.md…),主控合并;按 arXiv id / 归一标题去重;**记录"原始命中 vs 去重后"**,识别并合并的重复组数 log 出来。

### 7.5 防幻觉三重核验
- **元数据层**:论文真伪靠 WebSearch 快照交叉验证(id↔标题相符);**绝不靠单次 WebFetch**(实测会把给定标题原样回吐造成 prompt 污染);可靠核验=下载 `arxiv.org/pdf/<id>` 字节流 + pypdf/PyMuPDF 抽首页标题(假 id 在下载层 404)。
- **内容层**:8 字段+3 栏必须基于**下载到 `resource/papers/` 的真实 PDF 正文**;拿不准标 `〔待核〕`。
- **抽检层**:P4.5 随机抽篇核对关键事实是否真见于原文,编造即打回重做。

### 7.6 不静默截断 + 网络代理回退
- 任何上限/丢弃/跳过都 **log 说明丢了什么**;来源逐项打勾。
- 网络:直连优先;任何 clone/curl/WebFetch 失败 → 自动回退代理 `http://127.0.0.1:7897` 重试并 log。
- PDF 读取:Read 工具读 PDF 常误报 "password-protected/encrypted",改用 **PyMuPDF(fitz)** / pikepdf 抽正文;**运行抽取或打印的 python 命令必须加 `-X utf8`**(否则 Windows GBK 控制台遇中文/数学符号崩溃);**抽 top-2 图优先 `get_pixmap` 渲染整页/裁剪区域**(嵌入图常碎成几十块)。〔2026-06-04 冒烟测试:WebSearch/WebFetch、arXiv+OpenReview 直连下载、PyMuPDF 1.27.2 解析+渲染图 全 PASS;Python 3.13〕。

## 8. 产出物 + 目录结构
```
resource/research/survey_agent_self_evolving/
├── PLAN.md                         # 本文件(流程+机制+进度,断点续跑真值)
├── _state/
│   ├── wave1/A1..A7.md             # P1 各代理撒网候选
│   ├── candidates_master.md        # 合并去重主表(带 status)
│   ├── verify/V*.md                # P3 验证记录
│   ├── verified_shortlist.md       # P4 唯一工作队列(deep/light + status)
│   ├── rejected.md                 # 剔除项+原因
│   └── sections/sec_*.md           # P6 分线节
├── analysis/<key>.md               # 逐篇 ANALYSIS_PROMPT 三层精读 + top-2 精华图存 figs/ (幂等单元)
└── SURVEY_self_evolving_agents.md  # 终稿:全景taxonomy + L1-L7 + 探索-巩固对标
```
PDF 统一落 `resource/papers/<key>.pdf`(与既有共用)。

## 9. P1 撒网 agent 分工(7 路,待执行)
- **A1**:L1 免梯度/test-time 持续学习(arXiv + Semantic Scholar)。
- **A2**:L2 Agent Memory(生成式潜在/检索/参数式)(arXiv + HF)。
- **A3**:L3 经验→技能演进 Skill(arXiv + GitHub)。
- **A4**:L4 反思/自我纠错/经验内化(arXiv + OpenReview)。
- **A5**:L5 巩固/防遗忘/知识更新(arXiv + ACL)。
- **A6**:L6 Agent Harness/脚手架生态(GitHub + arXiv + 大厂)。
- **A7**:L7 综述+评测环境 + 7 篇种子的一跳引文扩展。

## 10. 种子篇(7,P1 先入表并核实)+ 对标 idea
| key | 标题 | 链接/出处 | 线 |
|---|---|---|---|
| jitrl | Just-In-Time RL: Continual Learning in LLM Agents Without Gradient Updates | arXiv:2601.18510(P3 复核);github.com/liushiliushi/JitRL(NUS) | L1 |
| agent_dice | Agent-Dice: Disentangling Knowledge Updates via Geometric Consensus | github.com/Wuzheng02/Agent-Dice;arXiv〔待核〕(SJTU+OPPO) | L5 |
| memgen | MemGen: Weaving Generative Latent Memory for Self-Evolving Agents | github.com/KANABOON1/MemGen;arXiv〔待核〕(NUS,2025-09) | L2 |
| leafe | Internalizing Agency from Reflective Experience (LEAFE) | arXiv〔待核〕(UCSD,2026-03) | L4 |
| skillclaw | SkillClaw: Let Skills Evolve Collectively with Agentic Evolver | arXiv:2604.08377;github.com/AMAP-ML/SkillClaw | L3,L6 |
| skillopt | SkillOpt: Executive Strategy for Self-Evolving Agent Skills | arXiv:2605.23904 | L3 |
| explore_consolidate | 基于探索-巩固的大语言模型持续学习架构 | 用户本地 idea 文档(无 arXiv) | 对标基准 |
> `explore_consolidate` 是**用户 idea 文档,作 P6 对标基准**,不进深读队列;其组件(JitRL 式在线 logit 调制 + Agent-Dice 式离线几何共识)用于 P1 找邻居。

## 11. 进度表(状态机载体,随执行更新)
| 阶段 | 状态 | 备注 |
|---|---|---|
| P0 脚手架 | ✅ | PLAN+ANALYSIS_PROMPT+冒烟测试 全就绪,2026-06-04 |
| P1 发现 | ✅ | A1–A7 撒网,原始约 169 命中 |
| P2 粗筛 | ✅ | 去重 152 篇(L1-L7) |
| P3 验证 | ✅ | V1 核 34 高危 id:31真/5修正/0伪(负控404) |
| P3.5 固化 | ✅ | deep 83 / light 69 / brief 15 |
| P4 深读 | ✅ | 152 篇全完成,每篇 top-2 图(共 292 图) |
| P4.5 抽检 | ✅ | 抽 14 篇全过(含 28 图核验),0 编造 |
| P5 引文扩展 | ✅ | 补 5 篇(安全/巩固空白),覆盖饱和 |
| P6 汇总 | ✅ | SURVEY 初稿(综述版) |
| P7 完善 | ✅ | light 69 篇升级完整三层(152 篇 analysis 全完整);L1-L7 全部卡片化(每篇 第一+二层入正文 + 原文链接 + 精读链接);装订成卡片版 1652 行 |

## 12. 恢复指引(精简)
真值 = 本 PLAN(进度)+ `_state/candidates_master.md`(候选)+ `_state/verified_shortlist.md`(队列)+ `analysis/*.md`(已读)。
恢复:读第 11 节进度 → 按 7.3 检查清单定位 → 续跑;P4 幂等(analysis/<key>.md 存在则跳过)。
