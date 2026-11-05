# 数据工程工作论文系列与 qtcloud-data 实物对照

## 背景

两条线在同一领域并行演化：

- 工作论文系列（`docs/essay`，quanttide-essay-of-data-engineering v0.1.1）：五篇论文提出四层形式化工程方法论——数据需求、数据意图、数据规格、数据实现。
- 实物（`apps/qtcloud-data`）：qtcloud-data CLI v0.3.0（crates.io `qtcloud-data-cli`，含 Linux / Windows 二进制）+ Go Provider + Flutter Studio，已发布并有交付记录。

两边的产物此前互不引用。本文做一次对照，记录分歧点与下一步方向。

## 对照：四层框架的命令化

CLI 的命令序列与四层框架的对应关系：

| 方法论层级 | CLI 命令 | 实物形态 |
|:--|:--|:--|
| 数据需求（价值合同） | `clarify` | 从客户聊天记录生成 DRD 数据需求文档 |
| 数据规格（约束合同） | `design contract` / `design blueprint` | 生成契约与蓝图（YAML + MD + HTML） |
| 规格验收 | `spec` / `review` | 规格 envelope 固化与完整性审计 |
| 数据实现（工程合同） | `implement` | 从蓝图 YAML 生成 Python / R / Stata / Matlab 代码 |
| 执行 | Provider `POST /blueprints/{name}/runs` | Go 后端按蓝图 YAML 的状态机执行 pipeline |

examples 中的规格书范例是直接证据：Part A 契约列输入输出与质量承诺（去重率 100%、Bot 识别准确率下限），Part B 蓝图画处理流程。论文主张「契约约束数据形状、蓝图约束过程」，`design` 生成 YAML、Provider 顺 YAML 执行，链路已经跑通。

## 关键判断：双轨演化的互不相认

方法论不是先写论文再立项，也不是先写代码再补理论，而是论文与产品同步演化的双轨。这带来一个特征：两边都缺对方的最新版。具体有三处。

第一，论文不举证。五篇论文没有一处提到 qtcloud-data。intro 的唯一案例是搜索工程离线诊断，而四层框架的参考实现就在同一仓库群内，已发布、有测试、有交付记录。

第二，同名不同义。`catalog.rs` 的 registry.json 已实现登记、状态流转（received → processing → processed → delivered）与产物类型，但它是交付管道的登记簿，只登记 process 走过的数据。catalog 论文提的「本地版 Data Catalog，索引任意异构来源」是对该实现的推广，两边各写各的。

第三，互有超前。蓝图的实际形式语言是 YAML + CUE（Provider 按状态机执行），intent 篇主张的范畴论记号停在示意。反向，Provider 的状态机与 Spec envelope 版本管理等已验证的工程设计也未沉淀回论文。

## 定位判断

- qtcloud-data 是四层方法论的参考实现，论文系列是它的理论表述。
- catalog 论文的提案应定位为 registry.json 现状的推广：从「交付登记簿」（我交付过什么）扩展到「全域索引」（本地有什么可以处理）。
- contract 篇 3.4「契约绑定数据而非管道」应对照 catalog.rs 现状讨论：现状中契约与登记已按数据组织，管道按蓝图另行组织。

## 下一步方向

打通引用比新增内容优先：

1. essay 系列各篇标注 qtcloud-data 为参考实现，contract 篇与 catalog 篇分别对照 catalog.rs 与 registry.json 现状。
2. qtcloud-data 文档回指方法论论文，说明命令序列背后的层级依据。
3. catalog 从交付登记簿向全域索引扩展时，以论文的待答问题清单（元数据模型、进入标准、格式约定）为设计输入。
