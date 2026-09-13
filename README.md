# Cuplivo → Kelivo 备份转换

Cuplivo 导出的备份（`settings.json` + `chats.json`）在 Kelivo 侧导入时会被拒绝：

Kelivo 的恢复流程会逐项校验 `settings.json`，只接受布尔 / 数字 / 字符串 / 字符串数组四种值，
外加若干「JSON 字符串」型键的正确形状。Cuplivo 新增的部分设置项不满足这个约束，
校验直接抛 `FormatException`，导入失败。

本工具在浏览器本地把备份清洗成 Kelivo 可接受的形状：

- `settings.json`：按 Kelivo 的校验规则逐项检查，不合规的键剔除并写入报告；
- `chats.json`：顶层只保留 `version` / `conversations` / `messages` / `toolEvents` / `geminiThoughtSigs`，
  会话与消息只保留 Kelivo 认识的字段；`image` / `file` 片段若 payload 不合规转为文本片段；
- 无主会话（`assistantId` 为空）可自动挂到占位助手，否则在 Kelivo 里不可见；
- 媒体目录原样透传，可勾选跳过以便先验证数据。

全部处理在浏览器内完成，不上传任何数据。

在线使用：https://xuanxuan9929.github.io/cuplivo-to-kelivo/
