# cost-aware-dispatch

面向 Codex 项目中枢的成本优先派工 skill：中枢负责需求沟通、调研、规划和验收，将实际开发交给最低合格档的执行者，依据失败证据升级，并限制返修轮数。

这是提示词与工作流程规则，不是自动调度程序。

## 适用范围

适用于代码、写作、设计、研究、报表和学习项目中的中枢协调工作。普通问答、无需派工的操作及单节点 worker 不需要扩张为中枢。保持主会话模型设置；用户指定的模型、预算、禁止委派与项目边界优先。此 skill 不赋予发布、删除或向其他对话发送消息的额外权限。

核心政策见 [SKILL.md](skills/cost-aware-dispatch/SKILL.md)：

- 先形成可验收的任务卡，再选择最低合格档。
- 中枢与执行者分工；小改动合并派发，同一范围保持单写入者，默认最多两个并行执行者。
- 分别记录通过、能力失败与环境阻塞，根据证据验收和升级。
- 每个任务最多四个档位各一次，加一次同档定向修复，总计最多五轮；U 为终点。
- 记录返修、耗时、核验与可取得的实际计量，如实保留未验证项。

## 模型适配

L / S / A / U 表中的 `gpt-6-luna`、`gpt-6.1-sol`、`gpt-6-astra` 和强度是作者环境配置示例，不是普遍可用保证，也不构成官方模型能力或价格声明。

使用前查看自身工具 schema、模型清单和推理参数，将档位映射到自身可用模型，并核对成本顺序。无法核实成本时记为未知；无法控制模型参数时明确报告限制。操作示例见 [工具适配与交接](skills/cost-aware-dispatch/references/tool-adapters.md)。

## 安装

仓库：[zhanshuguicai/cost-aware-dispatch](https://github.com/zhanshuguicai/cost-aware-dispatch)。版本定位使用 Git tag `v1.0.0`；需要复现该版本时选择该标签，检查所用安装器是否支持指定 ref。

安装前先备份本机已有的同名 skill，避免覆盖已有定制。

在 Codex 中通过内置 `skill-installer` 请求安装：

```text
使用 $skill-installer 从 GitHub 仓库 zhanshuguicai/cost-aware-dispatch 安装 skills/cost-aware-dispatch 路径下的 skill；指定 ref 为 v1.0.0。若已有同名 skill，先备份，再按当前安装器支持的方式处理。
```

也可在检出对应标签后，将仓库的 `skills/cost-aware-dispatch` 整个目录复制到本机 Codex skill 目录，通常为 `$CODEX_HOME/skills`；未设置 `CODEX_HOME` 时通常为 `~/.codex/skills`。确保复制后是 `skills/cost-aware-dispatch/SKILL.md`，而非多嵌套一层。按当前 Codex 安装方式重新加载技能。

## 调用

```text
使用 $cost-aware-dispatch 作为当前项目中枢，先明确任务与验收标准，再按最低合格档派工，记录实际模型请求与失败证据。
```

默认保留自动发现设置。工具调用参数以运行环境 schema 为准；仅在用户明确要求新建独立聊天时创建侧栏任务。

## 验证与限制

公开包包含入口、UI 元数据和按需读取的三个参考文件，没有备份、日志或用户配置。可用 Codex `skill-creator` 附带的 `quick_validate.py` 检查 `skills/cost-aware-dispatch` 的格式，并检查文档中的相对链接。

格式及链接检查不验证实际派工行为、模型可用性、费用或节省效果。本公开副本尚无新的运行效果验证。效果记录方法见 [效果记录与预算](skills/cost-aware-dispatch/references/measurement.md)，政策摘要见 [修订说明](skills/cost-aware-dispatch/references/revision-2026-10-26.md)。

参见 [CHANGELOG.md](CHANGELOG.md)；skill 结构可参考 [官方 Skills 文档](https://developers.openai.com/plugins/build/skills)。
