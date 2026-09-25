# 从心开始桥接

这是 `NEKO-FromTheHeart-DEMO` 的独立 N.E.K.O 插件。第一阶段只提供一个 `resolve_interaction` 入口，通过 N.E.K.O `/runs` 接收一个受版本合同约束的对白槽位请求。

## 边界

插件不修改游戏剧情、好感度、变量、结局、存档或长期记忆，也不操作外部游戏窗口。游戏仍然是这些状态的唯一权威；插件不可用时游戏使用本地 fallback。

第一阶段不启用动态 CG、中央 CG 服务、worker、SQLite 队列或插件 UI。

## 兼容性

```text
plugin version: 0.1.x
game id: from_the_heart
game version: 0.5.0-demo
node contract: 1.2.4
```

合同副本位于 `contracts/ch2_restaurant_favorite_food.v2.json`。合同版本、资源哈希、允许的反应和 `cache_miss_fallback` 必须与游戏端同步；不兼容时插件拒绝请求。

## 本地开发

在 N.E.K.O 源码根目录运行：

```powershell
uv run neko-plugin check from_the_heart --strict
uv run pytest -q plugin/plugins/from_the_heart/tests
```

也可以在安装版 N.E.K.O 的插件开发模式中加载本目录，手动启动插件后触发 `resolve_interaction`。

插件默认 `auto_start=false`，游戏端不会负责启动或停止插件。
