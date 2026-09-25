# 从心开始桥接设计说明

## Identity Lock

- plugin_id: `from_the_heart`
- folder: `plugin/plugins/from_the_heart`
- name: `从心开始桥接`
- entry: `plugin.plugins.from_the_heart:FromTheHeartPlugin`
- package type: `plugin`

## Purpose

为 `NEKO-FromTheHeart-DEMO` 提供一个受合同约束的单轮对白槽位。插件只返回对白、语义标签、视觉标签和内置资源描述，不拥有游戏剧情状态。

## First Version Scope

- `resolve_interaction` 一个运行入口
- 合同 `1.2.4`
- 确定性 exact-answer 路由
- 可选的 `conversation` 模型分类与对白生成
- 本地 policy 收敛和 fail-closed fallback
- 通过 N.E.K.O `/runs` 调用

## Out of Scope

- `ensure_cg` 和动态 CG
- 中央 CG 服务、worker、SQLite、WebP 上传
- 静态或 Hosted UI
- 修改 N.E.K.O 核心
- 写入游戏存档、好感度、变量、长期记忆或剧情跳转

## Contract Ownership

合同 JSON 是游戏合同的版本化副本。当前支持 `game_version=0.5.0-demo` 和 `node_contract_version=1.2.4`。合同变化必须同时更新游戏端、插件副本和兼容性测试。

## Runtime and Failure Policy

插件保持 `auto_start=false`，由 N.E.K.O 手动启动。插件不可用、模型超时、合同不匹配或结果校验失败时，游戏端继续使用本地 fallback。
