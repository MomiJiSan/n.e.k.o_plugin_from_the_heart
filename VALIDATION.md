# 本地验证记录

本文件记录第一阶段本地验收结果，不代表已经发布 `.neko-plugin` 或创建 Release。

## 已通过

| Gate | 结果 | 证据 |
| --- | --- | --- |
| 静态检查 | 通过 | `neko-plugin check from_the_heart --strict`，0 error |
| 插件单元测试 | 通过 | 13 passed |
| 代码风格 | 通过 | `ruff check plugin/plugins/from_the_heart` |
| 游戏端桥接测试 | 通过 | 游戏仓库 `tests/test_from_the_heart_ai.py` 与 `tests/test_release_build.py`，31 passed |
| N.E.K.O 发现 | 通过 | `/plugins` 发现 `from_the_heart` 和 `resolve_interaction` |
| `/runs` round-trip | 通过 | 手动启动插件后，`POST /runs`、状态轮询、`/export` 成功 |
| 游戏客户端 live 调用 | 通过 | `FromTheHeartClient` 实际调用 loopback `/runs` 并通过响应校验 |
| 合同拒绝 | 通过 | 旧 `node_contract_version` 返回 `CONTRACT_VERSION_MISMATCH` |
| 失败回退 | 通过 | 模型超时、非法 JSON、资源哈希错误、超长输入均 fail-closed |
| 迟到结果 | 通过 | run 超时后发送 cancel，未读取旧 run 的 export |

## 当前边界

- 第一阶段只提供 `resolve_interaction`。
- 动态 CG、中央服务、worker、SQLite 队列和插件 UI 未启用。
- 游戏仍拥有剧情、好感度、变量、结局和本地 fallback 的最终权威。
- 插件默认 `auto_start=false`，验证结束后服务和插件均已停止。
- `dialogue.py` 使用 N.E.K.O 宿主提供的模型工具模块；这是当前兼容性依赖，后续可在 N.E.K.O 提供稳定公开模型接口后迁移。

## 发布前还需确认

1. 在目标 N.E.K.O 安装环境重复一次 clean clone 加载和 `/runs` 验收。
2. 检查最终 `.neko-plugin` 不包含 `__pycache__`、`.pyc` 或本地缓存。
3. 冻结合同版本和内置资源哈希。
4. 再决定是否创建远程 Release。
