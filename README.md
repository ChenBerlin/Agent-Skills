# Agent-Skills

一个用于沉淀与复用 Agent Skills 的仓库。

本项目完全由 AI 编写并维护。

## 内容

当前仓库包含以下技能：

- `skills/code-architecture-drawio`：用于阅读理解项目代码并生成可导入 draw.io 的架构图 XML（包含图例、线型语义、分层与生命周期状态规范）。
- `skills/call-chain-mapper`：用于从代码中梳理项目调用链路，覆盖 HTTP/RPC/MQ/定时任务等入口，按 entry/service/gateway/repo 分层追踪，并聚合高重复链路后按业务模块输出文字化链路说明。
- `skills/service-chain-gcp-drawio`：用于根据服务链路与模块划分生成 draw.io XML 服务架构图，强制包含图例、GCP Zones 模块分组、链路聚合与线型/方向规范（主连线样式 ID `2_F4Z3EIYrPrO-kPp1a8-2`）。

## 使用教程

### 1. 克隆仓库

```bash
git clone <your-repo-url>
cd Agent-Skills
```

### 2. 查看已有技能

```bash
find skills -maxdepth 2 -name "SKILL.md"
```

### 3. 使用指定技能

1. 打开对应技能目录下的 `SKILL.md`。
2. 按文档中的流程执行（例如读取 `references/`、复用 `assets/` 模板）。
3. 输出结果时遵循该技能定义的格式与质量检查项。

### 4. 典型用法（以 code-architecture-drawio 为例）

1. 让 Agent 阅读项目代码，提取服务、模块、依赖与数据流。
2. 根据技能规范输出 draw.io XML。
3. 将 XML 导入 draw.io（diagrams.net）进行查看与微调。

### 5. 典型用法（以 call-chain-mapper 为例）

1. 让 Agent 先识别项目中的入口（HTTP、RPC、MQ 消费组、定时任务等）。
2. 从每个入口向下追踪 service/gateway/repo 调用路径，并记录关键副作用（DB 写入、外部 RPC、MQ 发送等）。
3. 将高度相似的链路聚合为“主链路 + 等价入口变体”。
4. 按业务能力模块输出调用链说明（可直接套用 `references/output-template.md`）。

## 作者

- Bolin Chen

## License

本项目使用 [MIT License](./LICENSE)。
