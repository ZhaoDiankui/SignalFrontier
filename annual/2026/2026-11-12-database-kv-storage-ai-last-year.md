# Database × KV × Storage × AI 基础设施：近一年产业全景追踪

统计周期：2025-10-16 至 2026-10-16  
生成日期：2026-11-12  
报告类型：基于每日产业雷达框架生成的年度专题回顾

> 本文是年度专题报告，不替代每日简报。区分已核实事实、厂商主张与产业判断；未完成逐家核查的领域明确标注，不将“未检索到”表述为“没有进展”。

## 一、年度总览

过去一年最重要的变化不是某一种数据库、缓存或存储产品取代其他系统，而是 Database、KV、Storage 与 AI 系统围绕数据生命周期逐渐协同：

1. 数据库开始成为 Agent 的持久化运行底座，竞争边界从 SQL 和存储扩展到上下文、执行状态、工具调用、权限治理与任务恢复。
2. 对象存储与开放数据格式从容量层走向共享数据层，数据库、分析引擎和 AI 检索系统尝试直接访问湖上数据。
3. 推理 KV Cache 从 GPU 进程内部优化发展为跨 GPU、CPU、网络和存储的分层基础设施。
4. 向量数据库、缓存和传统存储引擎相互渗透，但在一致性、持久性、数据语义和工作负载方面仍有本质区别。
5. AI 基础设施竞争越来越需要同时评估性能、资本成本、能源、网络、存储和单位工作负载成本。

## 二、商业数据库与云数据库

### 国际厂商

- **Snowflake**：Cortex Agent SDK 支持持久化对话线程、后台长任务和重新连接；Horizon Context 朝外部目录、元数据与治理互操作发展。2026-10 Postgres Data Mirroring GA，降低外部运营数据库进入 Snowflake 的同步成本。战略意义是数据平台向受治理的 Agent 平台扩展。官方资料：[Snowflake Release Notes](https://docs.snowflake.com/en/release-notes/all-release-notes)。
- **Databricks**：持续推进 Lakehouse、数据工程、分析、模型开发和治理整合。核心观察点是减少数据复制，并把数据处理与 AI 工作流纳入统一治理体系。
- **Google BigQuery**：跨区域表复制 GA、Hive catalog endpoint GA，以及 AI 分析能力演进，体现云数仓竞争从查询速度扩展到数据可用性、共享和 AI 分析入口。官方资料：[BigQuery Release Notes](https://docs.cloud.google.com/bigquery/docs/release-notes/)。
- **AWS Aurora / RDS**：继续围绕实例代际、存储性能、读写扩展、可用性、跨区域能力与迁移工具演进。2026-09 R8a/M8a 新实例系列是硬件代际变化的背景信号。
- **MongoDB**：文档模型、搜索、向量检索与 AI 应用平台的结合值得持续跟踪；不能将不同版本、托管版和自托管版能力混为一谈。
- **Oracle、SQL Server、IBM Db2、SAP HANA、Teradata、PostgreSQL 生态、Redis、CockroachDB、YugabyteDB**：应纳入一级监控，分别追踪企业事务、托管服务、兼容性、授权成本、云部署、向量与 AI 能力。本次没有完成这些厂商全部产品线的逐月发布审计，不能据此判断其年度表现。

### 国内厂商

- **TiDB / PingCAP**：2025-10 TiDB X 提出以对象存储为基础的存算解耦和多模型查询方向。2026-10 TiDB Cloud 宣布 Premium、BYOC、Filesystem 和 PostgreSQL 兼容 Starter 等进展；其中 BYOC、Filesystem 与 PostgreSQL 兼容 Starter 在公告时仍处预览阶段。TiDB X 核心代码开源属于未来承诺，不能提前视为已完成。官方资料：[TiDB Cloud 产品公告](https://www.pingcap.com/blog/tidb-cloud-scaile-2026-product-announcements/)。
- **OceanBase**：除分布式关系数据库内核外，OceanBase PowerContext 关注跨会话、模型和 Agent Host 的上下文交接。应重点验证状态持久化、查询、权限隔离、版本控制与恢复能力。官方资料：[OceanBase Blog](https://en.oceanbase.com/blog)。
- **阿里云 PolarDB**：应按 MySQL、PostgreSQL、Oracle 兼容版及分布式产品线分别追踪，重点观察存算分离、弹性扩缩、迁移兼容性、存储成本和向量检索。
- **腾讯云 TDSQL**：TDSQL Boundless 的对象存储冷热分离、全局二级索引、Binlog 并行回放、DDL 和监控增强，体现大规模企业负载的工程演进。
- **华为云 GaussDB**：官方更新提到向量搜索与标量数据混合查询，后续应验证其与事务、权限、备份、查询优化和企业运维的融合程度。
- **百度智能云、京东云、火山引擎，以及达梦、人大金仓、南大通用、神舟通用、瀚高、GreatSQL**：均应持续纳入国内数据库一级监控。本次未完成逐家全年发布审计，不对其年度表现作负面推断。

## 三、KV、缓存与存储引擎

- **RocksDB**：11.8.0 / 11.8.1（2026-07）引入异步读取接口，包括 `GetAsync`、`MultiGetAsync` 和文件系统异步读取能力，并优化 DB open 路径中的阻塞 I/O。说明优化重点从单次 Get/Put 微基准深入到文件打开、后台调度和远程文件系统。资料：[RocksDB Releases](https://github.com/facebook/rocksdb/releases)。
- **Redis**：Redis 8 的产品方向覆盖向量、JSON、时间序列和概率数据结构等，体现内存数据平台化。竞争点包括数据结构、持久化复制、查询能力、内存成本、许可和部署选择。
- 存储引擎持续关注的系统问题包括 LSM Compaction 的读写放大、异步 I/O、远端对象存储访问、缓存淘汰、NUMA、备份恢复、数据摄取和可观测性。

## 四、对象存储、数据湖与开放格式

- **Apache Iceberg**：2026 年持续推进多语言实现、格式 API、Schema 演进、快照和开放表格式生态。它使元数据、事务语义和跨引擎互操作逐渐成为独立于单个 SQL 引擎的基础设施能力。资料：[Apache Iceberg](https://github.com/apache/iceberg)。
- **Milvus 3.0**：2026-07 发布，推进面向对象存储和开放表格式的检索能力；3.0.2 等后续版本继续优化过滤搜索、索引构建、外部集合和 Storage V2。资料：[Milvus](https://github.com/milvus-io/milvus)。
- 关键技术路线包括计算存储分离、开放表格式、零复制检索、冷热分层、数据库直接访问对象存储。主要难点仍是小对象开销、元数据压力、索引维护、数据新鲜度、权限同步、远程 I/O 放大和故障恢复。

## 五、AI 推理基础设施与 KV Cache

代表项目：

- **NVIDIA Dynamo**：推进 KV-aware routing、跨 Pod GPU Memory Service、推理工作流调度和弹性扩缩。KV Cache 命中开始成为请求路由的输入，而非单个 Worker 的内部细节。资料：[NVIDIA Dynamo](https://github.com/ai-dynamo/dynamo)。
- **LMCache**：推进多节点 P2P CPU 内存共享、多进程架构和多推理引擎/硬件后端支持，尝试把 KV Cache 变成可复用的独立层。资料：[LMCache](https://github.com/LMCache/LMCache)。
- **vLLM**：2026 年持续推进分层 KV Cache offloading，在 GPU HBM、主机内存、存储和远端节点间保存与重载 KV 状态。资料：[vLLM](https://github.com/vllm-project/vllm)。
- **SGLang**：v0.5.20（2026-09）及 HiCache 等技术持续推进分层缓存、长上下文、混合模型和异构资源利用。资料：[SGLang Releases](https://github.com/sgl-project/sglang/releases)。

概念性缓存层次：

- L0：GPU HBM，低延迟、高带宽、容量昂贵。
- L1：CPU DRAM，本机缓存和近端复用。
- L2：远端内存 / 分布式缓存，跨 Worker 复用和路由。
- L3：本地 SSD / 共享存储 / 对象存储，容量扩展、恢复和预热。

关键研究问题：缓存身份与模型版本绑定、失效与一致性、KV-aware 调度、传输与分层成本、多租户安全隔离，以及包含 CPU/内存/网络/SSD/冗余副本的全成本评估。

KV Cache 与传统 KV 数据库存在技术交叉，但数据语义不同，不能简单视为普通 Key-Value 数据库的替代品。

## 六、Agent Memory 与 AI 数据基础设施

Agent Memory 不等于向量数据库，也不等于把聊天记录写入 Redis 或 PostgreSQL。应区分：

- 会话状态：对话、工具调用、任务进度，要求顺序、恢复和幂等。
- 长期记忆：用户事实、偏好和经验，要求检索、更新、冲突解决和来源追踪。
- 工作记忆：计划、临时变量和中间结果，要求低延迟与生命周期管理。
- 过程记忆：成功步骤、失败原因和工作流，要求版本和评估。
- 共享 Agent 状态：多 Agent 交接，要求权限、并发和所有权。
- 企业上下文：Schema、目录、权限和业务语义，要求治理与血缘。

代表性信号：OceanBase PowerContext、TiDB Cloud Filesystem（2026-10 预览）、Snowflake Cortex Agent SDK、Weaviate 的 MCP Server 预览与运维能力。

参考架构应将 Agent Runtime / Workflow / Tools 与任务状态、长期记忆、工作空间、企业上下文分层，并由关系数据库、KV、对象存储、向量索引和消息系统共同承载。竞争焦点将从检索质量扩展到状态可靠保存、恢复、更新和治理。

## 七、学术研究与会议

本年度可核实的研究信号包括：

- **F3: The Open-Source Data File Format for the Future**：SIGMOD 2026 荣誉提名，关注开放数据文件格式及数据库/分析系统间的数据共享。
- **Integrating Vector Databases across Embedding Models**：SIGMOD 2026 荣誉提名，关注 Embedding 模型变更时的向量数据库互操作。
- **Garnet: A Next-Generation Cache-Store for Accelerating Applications and Services**：VLDB 2026 最佳研究论文，说明缓存系统已成为独立系统研究对象。
- **How to Write to SSDs**：VLDB 2026 最佳研究论文荣誉提名，关注 SSD 写入行为与存储系统设计。
- CXL 与云原生数据库的相关研究可作为前置背景，但部分论文早于本报告起始时间，不计入本年度新成果。

重点会议入口：
- [SIGMOD 2026](https://2026.sigmod.org/)
- [VLDB 2026](https://vldb.org/2026/program.html)
- [OSDI 2026](https://www.usenix.org/conference/osdi26/technical-sessions)

建议持续监控清华、北大、人大、华东师大（石亮、胡卉芪、徐辰）、CMU、Berkeley、Stanford、MIT、UW、TUM、CWI、EPFL、TU Berlin、Saarland、NUS、NTU、SMU、HKUST、CUHK、HKU、CityUHK、PolyU、UQ、UNSW、悉尼大学及 KAIST 等团队。该清单是监控地图，不是年度成果排名；教授任职与最新研究方向需要逐人核实。

## 八、GitHub 项目观察

值得持续跟踪的项目：

| 项目 | 方向 | 重点观察 |
|---|---|---|
| RocksDB | KV 存储引擎 | 异步 I/O、Compaction、远端文件系统 |
| NVIDIA Dynamo | 推理服务 | KV 路由、GPU Memory Service、调度 |
| LMCache | 推理 KV Cache | P2P、多进程、后端互操作 |
| SGLang | 推理运行时 | HiCache、长上下文、异构推理 |
| vLLM | 推理运行时 | 分层 KV Cache、offloading |
| Milvus | 向量数据库 | 外部集合、开放格式、过滤搜索 |
| Apache Iceberg | 开放表格式 | 快照、Schema、Catalog、多语言实现 |

不能仅凭 Star 数判断项目价值。本次没有取得所有项目统一口径的近 30/90 天 Stars、Forks、Commit、PR、Issue 数据，因此不制作热度排名。后续应同时评估贡献者分布、License、测试和 Artifact、可复现性、生产案例、维护风险。

## 九、创业公司、投融资与并购

- **MotherDuck 收购 Tower Computing（2026-08）**：目标是增强面向 Agent 的数据工程执行能力，推动数据管道向 Data API 和 Agent 工作流延伸。资料：[MotherDuck](https://motherduck.com/)。
- **AI 数据中心融资风险**：Reuters 2026-10 报道部分项目上市计划推迟或撤回，资本成本、能源、土地和执行风险受到更多审视。该信号不等于 AI 需求全面下降，而是资本市场更加关注商业模式、合同质量和现金流。
- 创业方向包括分布式数据库、向量数据库、数据工程与分析、推理 KV Cache、Agent 上下文与 Memory、AI 基础设施云。融资金额和估值不能替代客户、收入、产品成熟度和留存验证。

## 十、硬件与系统架构

重点跟踪 CXL/内存池化、GPU HBM、CPU DRAM、NVMe SSD、高速网络、数据中心能源与冷却。CXL 不能简单理解为“所有节点共享本地 DRAM”：数据库需要感知内存位置、访问延迟、分配与迁移策略、NUMA 拓扑和故障边界。有效研究必须提供真实拓扑、端到端工作负载、尾延迟、资源成本和故障场景对比。

## 十一、未来 1—3 年值得研究的十个问题

1. 数据库是否会成为 Agent 的持久化运行底座？
2. 分层 KV Cache 能否形成通用基础设施？
3. 数据库能否直接在开放数据湖上提供高性能服务？
4. CXL 与数据库内存管理如何协同？
5. 向量数据库如何跨 Embedding 模型与存储系统互操作？
6. 数据文件格式与数据库内核是否会进一步解耦？
7. Agent Memory 如何满足企业级安全和审计要求？
8. 存储引擎如何在异步 I/O 和远端存储下保持低尾延迟？
9. AI 基础设施如何建立统一成本模型？
10. 如何建立可复现的 Database × AI 基础设施评测体系？

## 十二、综合结论与研究优先级

### 五个年度结论

1. 数据库正在进入 Agent 的执行与状态管理体系。
2. 对象存储和开放数据格式正在改变数据库架构。
3. 推理 KV Cache 正在成为独立的系统优化层。
4. 缓存、向量数据库和传统存储引擎的边界正在变化，但数据语义仍有差异。
5. 技术性能必须与能源、内存、网络、存储和资本成本一起评估。

### 建议优先级

- **P0：** 推理 KV Cache 与异构内存；数据库 × Agent Memory。
- **P1：** 对象存储、开放表格式与湖上检索；存储引擎与硬件协同。
- **P2：** 创业公司、投融资与产品竞争，重点核实客户、收入、产品成熟度和商业可持续性。

## 十三、覆盖范围与可信度说明

本报告是年度专题回顾，不是全球数据库与 AI 基础设施的完整事件数据库。国际商业数据库、国内厂商、学术团队、GitHub 项目和融资事件的公开信息可得性不同；部分领域只核实了代表性案例，没有完成逐家逐月审计。报告中的厂商性能数字应视为厂商主张，不能当成独立测试结果。未检索到的领域不等于没有进展。

**最终判断：** Database × KV × Storage × AI 的长期竞争，将围绕数据生命周期展开：数据产生、持久化、索引、检索、缓存、Agent 使用、权限治理和成本控制。真正有竞争力的架构，需要同时证明性能可预测、状态与数据可靠、单位工作负载总成本更低。
