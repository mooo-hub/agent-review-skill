# Agent Review Skill

> 一套完整的 Agent 体系审查 Skill，用于审查 agent 体系的设计与行为是否符合四角色架构规范。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://github.com/anthropics/claude-code)

---

## 📖 简介

本 Skill 基于**四角色架构规范**（Orchestrator/Specialist/Reporter/Reviewer）设计，实现对 Agent 体系的**元审查**——不审查具体业务输出，而是审查 Agent 体系本身的架构合规性。

### 核心特性

- ✅ **四类审查对应四角色**：每类审查严格对齐架构指南中的角色定义
- ✅ **KVP 验证机制**：强制读取知识文件并输出规则，确保审查客观性
- ✅ **三状态输出**：pass/warning/error + A/B/C/D/F 等级
- ✅ **检查清单框图化**：使用 ASCII 框图组织检查项，便于逐项核对

---

## 🚀 快速开始

### 安装

将本 Skill 复制到 Claude Code 的 skills 目录：

```bash
# 复制到 Claude Code skills 目录
cp -r agent-review ~/.claude/skills/
```

或者直接克隆仓库：

```bash
cd ~/.claude/skills/
git clone https://github.com/your-username/agent-review-skill.git agent-review
```

### 使用方式

在 Claude Code 中使用：

```bash
# 完整审查（四类审查全流程）
/agent-review .claude/skills/your-target-skill

# 仅审查主 Agent
/agent-review orchestrator .claude/skills/your-target-skill

# 仅审查子 Agent
/agent-review specialist .claude/skills/your-target-skill

# 仅审查输出 Agent
/agent-review reporter .claude/skills/your-target-skill

# 仅审查审核 Agent
/agent-review reviewer .claude/skills/your-target-skill
```

---

## 📁 目录结构

```
agent-review/
├── SKILL.md                                    # 主调度 Skill
├── knowledge/
│   ├── 四角色架构规范.md                         # 核心方法论
│   ├── 评分标准与处理动作.md                      # 评分等级定义
│   └── 审查报告模板.md                           # 最终输出格式
│
└── agents/
    ├── orchestrator-reviewer/                   # 主 Agent 审查
    │   ├── SKILL.md
    │   └── knowledge/主agent审查检查清单.md
    │
    ├── specialist-reviewer/                     # 子 Agent 审查
    │   ├── SKILL.md
    │   └── knowledge/子agent审查检查清单.md
    │
    ├── reporter-reviewer/                       # 输出 Agent 审查
    │   ├── SKILL.md
    │   └── knowledge/输出agent审查检查清单.md
    │
    └── reviewer-reviewer/                       # 审核 Agent 审查
        ├── SKILL.md
        └── knowledge/审核agent审查检查清单.md
```

---

## 🔍 四类审查维度

### 1. 主 Agent 审查（orchestrator-reviewer）

| 维度 | 权重 | 核心检查点 |
|------|------|-----------|
| 流程协调 | 30% | 调度流程图、串行/并行、全局上下文、唯一入口 |
| 任务拆解 | 25% | 子 Agent 注册表、输入/输出契约、任务依赖 |
| KVP验证 | 25% | KVP列表、output_sample、验证失败处理 |
| 错误处理 | 20% | 超时重试、数据缺失处理、审核返工、升级机制 |

### 2. 子 Agent 审查（specialist-reviewer）

| 维度 | 权重 | 核心检查点 |
|------|------|-----------|
| 输入契约 | 25% | 输入数据结构、必填/选填字段、校验规则 |
| 输出契约 | 25% | 输出格式、task_id/status/error字段 |
| 职责边界 | 30% | 一句话描述、NEVER约束、不响应用户、不调其他子Agent |
| 知识调用 | 20% | KVP验证点、rules_read、知识文件路径 |

### 3. 输出 Agent 审查（reporter-reviewer）

| 维度 | 权重 | 核心检查点 |
|------|------|-----------|
| 格式规范 | 35% | 报告模板存在、模板内嵌、多格式支持 |
| 完整性 | 35% | 汇总上游数据、数据来源标注、缺失数据处理 |
| 可读性 | 30% | 先数据后解读、表述风格指南 |

### 4. 审核 Agent 审查（reviewer-reviewer）

| 维度 | 权重 | 核心检查点 |
|------|------|-----------|
| 独立性 | 35% | 不参与生产、只出意见不修改、审核节点触发时机 |
| 客观性 | 30% | 基于检查清单、禁止放宽标准、评分标准量化 |
| 规则应用 | 35% | KVP知识验证、结构化JSON输出、三状态定义 |

---

## 📊 评分标准

| 等级 | 分数范围 | 状态 | 处理动作 |
|------|----------|------|----------|
| A级 | ≥90分 | ✅ pass | 无需修改 |
| B级 | 75-89分 | ✅ pass | 有改进建议 |
| C级 | 60-74分 | ⚠️ warning | 需修改 |
| D级 | 40-59分 | ❌ error | 需重写 |
| F级 | <40分 | ❌ error | 退回重写 |

---

## 📋 审查报告示例

```markdown
# Agent体系审查报告

## 执行摘要

| 项目 | 内容 |
|------|------|
| **审查日期** | 2026-06-17 |
| **被审查Skill** | classical-astro-natal |
| **整体评分** | 82/100 |
| **整体状态** | ✅ pass |
| **主要发现** | 主调度架构良好，子Agent职责边界有改进空间 |

## 审查结果汇总

| 审查维度 | 评分 | 等级 | 状态 |
|----------|------|------|------|
| 主Agent审查 | 85 | B | pass |
| 子Agent审查 | 72 | C | warning |
| 输出Agent审查 | 90 | A | pass |
| 审核Agent审查 | 80 | B | pass |
```

---

## 🔧 设计原则

1. **元审查架构**：审查 Agent 体系本身的设计，而非业务输出
2. **四类审查对应四角色**：严格对齐 Orchestrator/Specialist/Reporter/Reviewer
3. **KVP 验证机制**：强制读取知识文件并输出 rules_read
4. **三状态输出**：pass/warning/error + A/B/C/D/F 等级

---

## 📚 参考资料

- [Agent 体系搭建指南](./knowledge/四角色架构规范.md) - 四角色职责边界定义
- [评分标准与处理动作](./knowledge/评分标准与处理动作.md) - 评分等级定义
- [审查报告模板](./knowledge/审查报告模板.md) - 最终输出格式

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

---

## 📄 许可证

本项目采用 [MIT License](./LICENSE) 开源协议。

---

*版本: v1.0 | 创建日期: 2026-06-17*
