---
name: lightcone-caster-release-bin
description: 兼容旧名称：通过 Lightcone 服务/实验定位 app 后进行 Caster 镜像发布。调用统一 caster-release，不维护独立联合发布流程。
---

# 兼容入口

读取并执行 [caster-release](../caster-release/SKILL.md)，选择 Lightcone 定位入口。

Lightcone 仅提供服务 → 实验 → CPU/GPU app → Caster 链接映射，镜像发布全部由 Caster 执行。不要创建第二套发布单、双平台发布步骤或虚构先后依赖。

只有用户另行要求配置变更时，才组合 [lightcone-deploy-config](../lightcone-deploy-config/SKILL.md)；入口导航本身不产生配置写入。
