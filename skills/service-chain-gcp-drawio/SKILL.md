---
name: service-chain-gcp-drawio
description: Generate draw.io XML service architecture diagrams from service chains and module boundaries using GoogleCloudPlatform style, with required legend, module clustering, deduplicated overlapping paths, and strict line semantics (solid for call/control, dashed for message/data, and query RPC arrows following data-flow direction). Use when users ask for architecture diagrams based on service call paths/modules.
---

# Service Chain to GCP draw.io XML

根据服务链路与模块边界，产出可直接导入 draw.io 的 XML 架构图。

## Workflow

1. 明确输入与粒度。
   - 收集服务清单、模块划分、链路类型（调用/控制/消息/数据）。
   - 确认输出目标是“服务架构图”，不是时序图。

2. 构建链路模型后再绘图。
   - 按“模块”分组服务节点。
   - 同一模块内链路优先放在一起，减少跨模块穿越。
   - 合并重叠链路：当多条链路在同一段路径重合时，保留主干并在边标签中聚合说明，避免重复画线。

3. 使用 GCP 风格组织画布。
   - 模块容器使用 GoogleCloudPlatform / Zones 组件风格。
   - 服务节点使用 GoogleCloudPlatform 体系中的服务类图元（或兼容的 GCP2 风格 shape）。
   - 主连线使用 GCP 蓝色，遵循样式 ID `2_F4Z3EIYrPrO-kPp1a8-2`。

4. 生成 draw.io XML。
   - 输出必须是可导入的 draw.io XML（`mxfile` / `diagram` / `mxGraphModel` 结构完整）。
   - 所有节点、边 ID 唯一。
   - 图中必须包含独立“图例（Legend）”区域。

5. 自检并交付。
   - 检查线型、方向、图例、分组、聚合是否全部符合规则。

## Mandatory diagram rules

### 1) 输出格式

- 只交付 draw.io XML 内容（可保存为 `.drawio` 或 `.xml`）。
- 不用伪代码或 JSON 代替 XML。

### 2) 图例（必需）

图中必须有 Legend，至少说明：

- 实线：调用链路 / 控制链路。
- 虚线：消息链路 / 数据链路。
- 箭头方向语义（特别是数据流向规则）。
- 模块容器（GCP Zones）与服务节点的视觉含义。
- 主连线颜色与样式 ID：`2_F4Z3EIYrPrO-kPp1a8-2`。

### 3) 线型与方向

- 调用链路、控制链路：**实线**。
- 消息链路、数据链路：**虚线**。
- 表达数据流时，箭头必须表示“数据从哪里流向哪里”。
  - 例如：A 服务查询 B（query RPC）。如果表达的是数据流，边应画 **B -> A**，因为数据从 B 返回到 A。
  - 不要用 A -> B 去表达该数据流。

### 4) 模块分组

- 相同模块的服务与链路在图中相邻布局。
- 模块容器使用 GoogleCloudPlatform 的 Zones 风格组件进行标注。

### 5) 链路聚合

- 对不同链路重合的公共路径进行必要聚合。
- 保留可读性的前提下，尽量减少重复边与重复标签。

### 6) 风格约束

- 绘图整体使用 draw.io 的 GoogleCloudPlatform 风格。
- 主连线颜色使用 GCP 蓝色；在样式上显式使用样式 ID `2_F4Z3EIYrPrO-kPp1a8-2`（或其等价样式引用）。

## XML implementation hints

- 常见容器与节点样式可使用 `mxgraph.gcp2` 前缀图元。
- 边样式建议至少显式声明：
  - 主色：GCP 蓝色（如 `strokeColor=#4285F4`）
  - 实线/虚线（`dashed=0/1`）
  - 箭头方向（`endArrow=block`）
- 若使用样式引用机制，主连线需包含样式 ID `2_F4Z3EIYrPrO-kPp1a8-2` 的引用信息。

## Output contract

交付时按以下顺序：

1. 简短说明模块与链路聚合策略（1-5 行）。
2. 提供完整 draw.io XML。
3. 确认 Legend 已包含线型、方向、样式 ID 说明。

## Quality checklist

结束前逐项确认：

- XML 可被 draw.io 成功导入。
- 存在 Legend 且信息完整。
- 调用/控制为实线，消息/数据为虚线。
- Query RPC 的数据流方向已按“提供方 -> 查询方”绘制。
- 同模块链路已聚拢，跨模块重叠路径已聚合。
- 模块容器为 GCP Zones 风格。
- 主连线为 GCP 蓝色并包含样式 ID `2_F4Z3EIYrPrO-kPp1a8-2`。
