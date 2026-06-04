---

## 对「探索-巩固」idea 的借鉴清单 + 选型建议

> 这是本调研对你最直接的产出。你的 idea =「**在线免梯度探索**(冻结基座 + 历史经验缓冲区算局部优势 + logit 调制即时自适应 + 收集高质量轨迹)+ **离线巩固进参数**(任务级梯度 + 几何共识过滤消冲突 + KL 约束自然梯度投影全参更新)」。下面逐环节给可操作的对标与可借组件,每条附现成来源。

### 一、idea 在地图中的定位:横跨"内化分水岭"的稀缺格

152 篇里约 130 篇停在"外挂层(记忆/技能/harness)免梯度演进、只增不删、**不进权重**";真正"把经验巩固进参数"的 < 20 篇。**你的 idea 横跨这条分水岭(在线在左侧探索、离线向右巩固进参数),正落在全领域最大的开放空白上**——两篇综述锚都把"巩固/防遗忘算法族"列为缺失。这是卖点,也意味着竞品稀少、可借组件分散在各线需自己拼装。

### 二、在线探索期(logit 调制)——有"近同款"

- **`jitrl`(2601.18510,已开源)= 探索期的近同款**:检索经验估优势 Â → `z'=z+β·Â` logit 加性调制,**并证明这正是 KL 约束策略优化的闭式最优解**。可直接作你探索期的理论基座 + 头号 baseline。
- **`training_free_grpo`**:把"数值优势"换"语义优势"存经验库 + ADD/DELETE/MODIFY 治理 —— 补 jitrl 的"只增不删"。
- **`icrl_prompting`**:reward 拼 context 即涌现 RL,最简对照。
- 共同警示:三者知识都停在 context/经验层、**不进权重**——正是你巩固期要补的。

### 三、离线巩固期(几何共识 + KL 投影)——种子同款 + 机理背书

- **`agent_dice`(2601.03641,已开源/LLaMA-Factory)= 巩固期几何共识的同款**:逐维符号投票剪冲突梯度 + 曲率加权,免梯度、~1 分钟/步。直接基线。
- **`cnl_gradsim`**:逐参数符号掩码冻结冲突参数,**在线 per-step、近零存储**,比 agent_dice 更轻;其"只放行协作参数梯度"与你关注的 forward-hard/backward-soft 解耦同构,可作在线增量巩固。
- **`fopng_fisher`**:Fisher 正交投影 = KL 局部二次型 —— 给你"KL 约束自然梯度投影"的现成理论根与闭式解。
- **`rls_razor` + `retaining_by_doing`(均开源)= 机理背书**:on-policy 数据隐式做前向 KL-min 投影,**比 SFT 少遗忘**(retaining 用 β=0/REINFORCE 实锤"是 on-policy 数据这个因,不是 KL 正则")。直接支撑"用探索期收集的 on-policy 高质量轨迹做巩固"这一设计。
- 备选:`gorp`(梯度子空间投影,BWT 0.8%)、`soup_to_go`/`merge_before_forget`(merging)。

### 四、"把探索轨迹巩固进参数"的可学习范本(最贴落点,强烈建议)

- **`auto_dreamer`(2605.20616,veRL)= 最直接的"可学习巩固"范本**:GRPO 端到端学一个**离线巩固器**(快 writer 采集 + 慢巩固器区域重写),复合奖励 = 下游效用 + 反事实掩码效用。**建议把你巩固期的"固定几何共识规则"升级为这种可学习巩固器**(几何共识作其先验/baseline)。可借:反事实掩码判承重/冗余/有害、区域重写=省略式遗忘、provenance 防幻觉。
- **`in_place_ttt`(已开源,ByteDance Seed)**:就地复用 MLP 的 W_down 作 fast weights、一步内积闭式写入、slow/fast 分离防遗忘;论文自述 NTP 目标与 MTP 同源。其 future work"fast→slow 蒸馏"正是"探索(fast)→巩固(slow)"的现成载体。
- **`SDPO`/`SD-ZERO`(SDPO 开源 lasgroup/SDPO)**:on-policy 自蒸馏,**stopgrad =「前向硬/反向软」**(与你的技术签名同构),self-teacher 条件于探索轨迹产 token 级 KL 目标,无需外部 reward —— 把探索轨迹蒸进权重的最干净配方。
- **`skill0_zero`/`skillc`(skill0 开源/veRL)**:训练给脚手架→推理撤→能力进参数,用"带/不带脚手架的性能差"作内化探针、修正项随内化自动衰减。

### 五、巩固三大坑(必须内建防护,否则复现已知崩溃)

1. **巩固坏经验**:`misevolution` 实测记忆通道 reward-hacking 致安全率暴跌 45–84%、round-60 突崩 → 巩固前装**坏经验过滤 + 安全闸**。
2. **覆盖旧能力**:`capability_erosion_cpe` 证侵蚀 ∝ gᵀH_<t g(曲率)→ 加**保持正则 Ω**(EWC-Fisher / 证据门控)。
3. **过度抽象丢细节**:`auto_dreamer` Pattern 3 → provenance grounding + 保留细节档。

### 六、评测(几乎为你量身定做,直接用)

- **`bench_agentcl`**:**Plasticity / Stability / Generalization** 三增益指标 = "探索增益 / 巩固不退化 / 可迁移"的现成度量;MEMPROBE = 巩固前筛经验,与你"巩固前先选"同构。**最贴的评测面 + 对照基线**。
- **`bench_benchtrace`(FAR/Reflection-Evolution)+ `bench_reliability`(RDC/VAF/GDS/MOP)**:衡量"巩固后是否真稳健(非单次更准)"、熔断率是否下降。
- **`bench_stulife`**:"知识内化"durability 指标。
- 共同实证:**朴素记忆/回放有害** → 佐证"必须巩固到参数/技能而非囫囵堆 episodic 记忆"。

### 七、现成可复用资产(有开源,可 clone 参考)

探索侧:`jitrl`、`training_free_grpo`;巩固侧:`agent_dice`(LLaMA-Factory)、`gorp`、`eaft`、`in_place_ttt`、`retaining_by_doing`;内化:`SDPO`(lasgroup/SDPO)、`skill0_zero`(veRL);可学习巩固:`auto_dreamer`(veRL)。**框架**:巩固/内化侧 RL 压倒性用 **veRL**;少数 LLaMA-Factory / TRL。

### 八、★ 推荐的"探索-巩固"实现配方(综合)

1. **探索期**:jitrl 式 logit 调制(KL 闭式)在冻结基座上收集高质量 on-policy 轨迹,经验库带 DELETE 治理。
2. **巩固器**:首选 `auto_dreamer` 式**可学习巩固器**(GRPO 学,复合奖励=下游效用+反事实掩码);以 `agent_dice`/`cnl_gradsim` 几何手术作 baseline/先验。
3. **内化路径**:`in_place_ttt` 的 fast→slow,或 `SDPO` on-policy 自蒸馏(stopgrad 前向硬/反向软)把探索轨迹蒸进权重。
4. **防遗忘**:用 on-policy 数据(rls_razor 机理)+ Fisher/KL 投影(fopng)+ 保持正则(CPE)。
5. **安全闸**:misevolution 式坏经验过滤 + 纵向 Safe Rate 回归监控。
6. **评测**:bench_agentcl 三指标 + benchtrace 稳健性。

### 九、可直接照搬的对照/消融

- 巩固机制:固定几何共识(agent_dice)vs 可学习巩固器(auto_dreamer)vs on-policy 自蒸馏(SDPO)。
- 内化 vs 外置:进参数 vs 纯记忆库(受 base 封顶对照,印证巩固进参数的增量)。
- 防遗忘:on-policy vs SFT(retaining_by_doing 协议直接套)。
- 安全:有无坏经验过滤(misevolution 四通道纵向回归)。
