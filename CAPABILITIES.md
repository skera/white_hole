# 平台与场景能力索引

本文件是 AI 路由表。第一层只描述平台界面、栏目功能及结构；第二层描述为达成 skill 目标的 plan 和 workflow。**优先第二层；没有匹配场景时，基于第一层界面知识自动组织临时 plan/workflow。**

## 第二层：目标、Plan 与 Workflow（优先匹配）

| ID / skill | 任务意图与匹配词 | 边界 / 必要输入 | 依赖第一层 | 输出 / 完成条件 | 实证状态 | 入口 |
| --- | --- | --- | --- | --- | --- | --- |
| `lightcone-deploy-config` | Lightcone 发布配置、CPU/GPU gflags、批量实验配置修改 | 服务/实验/环境、参数名/类型/值、是否发布；不包含 bin 升级 | `lightcone-platform` | 保存持久化、diff 核对、部署终态、QPS/打分验收分别有证据 | 已有多服务/多实验补旗与纠错实证；GPU 已配置跳过、最多 3 个在途发布、滚动 pod 收敛验收；其他字段须现场确认 | [SKILL.md](skills/scenarios/lightcone-deploy-config/SKILL.md) |
| `lightcone-caster-release-bin` | 明确通过 Lightcone + Caster 联合发布 bin、跨平台二进制升级 | 两平台目标映射、制品/版本、依赖顺序与兼容要求；不是单纯配置更新 | `lightcone-platform` + `caster-platform` | 两平台版本/配置对应、部署状态及业务验收；明确部分完成状态 | 待实证：编排框架，尚无联合发布的实际操作与先后顺序证据 | [SKILL.md](skills/scenarios/lightcone-caster-release-bin/SKILL.md) |
| `caster-release` | 基于 Caster 的单平台发布、升级或发布 bin | 应用/环境、发布对象、目标版本、批次及验收条件；联合发布转上一行 | `caster-platform` | 版本和目标实例一致、任务终态及监控结论 | 待实证：场景框架，尚无 Caster 发布案例 | [SKILL.md](skills/scenarios/caster-release/SKILL.md) |

## 第一层：平台界面与结构知识

| ID / skill | 页面与栏目 | 字段 / 结构语义 | 实证边界 | 入口 |
| --- | --- | --- | --- | --- |
| `lightcone-platform` | 服务收藏、实验列表、CPU/GPU 配置、保存确认、发布 diff、发布记录与关联审批 | 模型/KFC、额外 gflags、性能字段、实例/zone、草稿/线上对象、状态含义 | 配置相关界面已实证；bin 界面未知；不含执行流程 | [SKILL.md](skills/platforms/lightcone-platform/SKILL.md) |
| `caster-platform` | 目前只有 Lightcone 中的 Caster 关联入口，Caster 本体栏目待补充 | 已观察 app、zone、实例关联；本体版本/任务字段未知 | 本体界面待实证；不预设菜单、按钮或操作流程 | [SKILL.md](skills/platforms/caster-platform/SKILL.md) |

## 路由算法

1. 提取平台、目标对象（配置/bin/其他）、操作（查看/修改/保存/发布）和结果需求；沿用当前已明确的信息。
2. 匹配第二层最具体的一行。包含 Lightcone 和 Caster 的联合 bin 发布优先于单平台场景；平台不明的“发布 bin”需先确定平台，不能默认为 Caster。查看发布状态也可采用对应场景的只读步骤，不因此执行发布。
3. 读取所选场景及它依赖的平台文档。满足前提时，在授权范围内执行；缺少前提时先做独立的只读工作，再请求必要信息。`待实证` 不能被当作可直接执行的操作手册。
4. 若匹配多个场景且目标边界不同，询问会影响实施路径的最小问题；只有用户确实要求多个目标时才组合多个场景。
5. **完全没有匹配场景**时，读取相关第一层界面知识，根据目标生成“输入 → 页面动作 → 验证输出”的临时 plan/workflow，并说明平台间依赖。如果平台界面信息尚不充分，先发现或请求平台信息，不能用不相关平台代替。
6. 无论哪条路径，不能以降级到第一层绕过 diff 核对、授权、缺失的操作证据或监控失败。仅保存配置的任务在保存与读回完成后结束；包含发布的任务才继续发布与验收。

## 分层约定

- 第一层只拥有本平台的描述性界面知识：页面层级、栏目、字段、控件功能、对象关系和状态语义；不包含步骤顺序、跳过/重试策略、授权门禁、异常处置或验收方法。
- 第二层拥有目的、输入输出、plan、workflow、实际操作步骤、跨平台顺序、范围、决策分支、异常处理和验收方法；通过相对链接读取第一层界面知识。监控公式和问题处理资料随对应场景存放。
- 新增平台放入 `skills/platforms/<name>/SKILL.md`；新增场景放入 `skills/scenarios/<name>/SKILL.md`，同时添加表格行、依赖、匹配边界和实证状态。
- 发布结果分别记录“配置保存”“发布触发”“部署完成”“业务验收”，不得用前一阶段替代后一阶段。
- 公开仓库只存通用流程和参数模板，不提交内部凭据、生产原始数据或完整私有页面快照。

## Lightcone 运行问题入口

Lightcone 第二层 workflow 需要读取 [问题处理与证据表](skills/scenarios/lightcone-deploy-config/references/troubleshooting.md)；监控使用 [滚动更新验收参考](skills/scenarios/lightcone-deploy-config/references/monitoring.md)。已收录页面加载空值误判、零实例发布资格、旧审批阻塞、diff 截图错误、监控口径与滚动新旧版本收敛问题。Caster 的独立发布操作仍为待实证，Lightcone 列表中的 Caster 信息不等于已验证 Caster 发布 API 或 UI。
