# Agent Review - Agent体系审查框架

> 一套标准化的Agent架构审查工具，让你的多Agent系统符合四角色架构规范

## 🎯 它能做什么？

Agent Review 是一个 **Agent体系审查主调度框架**，用于验证你的多Agent系统设计是否符合 **四角色架构规范**：

| 角色 | 职责 | 审查维度 |
|------|------|----------|
| **主Agent (Orchestrator)** | 任务拆解、协调调度 | 流程协调、KVP验证、错误处理 |
| **子Agent (Specialist)** | 单一领域深度执行 | 输入契约、输出契约、职责边界 |
| **报告Agent (Reporter)** | 结构化输出 | 格式规范、完整性、可读性 |
| **审查Agent (Reviewer)** | 质量把关 | 独立性、客观性、规则应用 |

## 🚀 核心特性

### 1. 四维度完整审查
- **主Agent审查** - 验证调度流程、任务拆解能力、KVP验证机制
- **子Agent审查** - 检查输入输出契约、职责边界是否清晰
- **报告Agent审查** - 确保输出格式规范、内容完整
- **审查Agent审查** - 验证独立性与客观性

### 2. 结构化评分体系
markdown
A级 (≥90分) → 完全合规，无需修改
B级 (75-89分) → 基本合规，有改进建议
C级 (60-74分) → 部分不合规，需修改
D级 (40-59分) → 严重不合规，需重写
F级 (<40分) → 完全不合格，退回重写


### 3. KVP验证机制
每个子Agent调用后自动验证输出结果，确保调用真实发生且有有效输出。

### 4. 自动化问题检测
- 识别职责边界模糊
- 发现契约缺失
- 检测环路风险
- 标注质量门禁问题

## 📦 安装使用


### 将 skill 复制到你的 Claude Code skills 目录
```
cp -r agent-review ~/.claude/skills/
```
### 🔧 使用方式
```
/agent-review <skill-path>
```
### 示例
```
/agent-review .claude/skills/my-skill/
```
## 审查模式
模式	说明
full	完整审查（默认）
orchestrator	仅主Agent审查
specialist	仅子Agent审查
reporter	仅报告Agent审查
reviewer	仅审查Agent审查

## 📊 输出示例
```
{
  "agent_review_report": {
    "overall_score": 82,
    "overall_status": "pass",
    "overall_grade": "B",
    "sub_reviews": {
      "orchestrator": { "score": 85, "grade": "B" },
      "specialist": { "score": 72, "grade": "C", "status": "warning" },
      "reporter": { "score": 90, "grade": "A" },
      "reviewer": { "score": 80, "grade": "B" }
    },
    "require_revision": true,
    "revision_scope": ["specialist"]
  }
}
json
```
## 🏗️ 架构设计
```
用户请求
    │
    ▼
┌─────────────────────┐
│   主调度Agent        │  ← 解析目录、调度审查
└──────────┬──────────┘
           │
     ┌─────┼─────┐
     ▼     ▼     ▼
  主Agent  子Agent  报告Agent  审查Agent
  审查器   审查器    审查器      审查器
     └─────┬─────┘
           │
           ▼
      审查报告
```
## 🤝 适用场景
Agent开发者 - 发布前自检架构合规性
团队负责人 - 审核团队成员提交的Agent设计
框架维护者 - 确保贡献的Agent符合规范
## 📜 许可证
MIT License
