# Agent-Skills

一个用于沉淀与复用 Agent Skills 的仓库。

本项目完全由 AI 编写并维护。

## 内容

当前仓库包含以下技能：

- `skills/code-architecture-drawio`：用于阅读理解项目代码并生成可导入 draw.io 的架构图 XML（包含图例、线型语义、分层与生命周期状态规范）。

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

## 作者

- Bolin Chen

## License

本项目使用 [MIT License](./LICENSE)。
