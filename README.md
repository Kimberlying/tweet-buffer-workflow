# Tweet Buffer Workflow

一个把原始素材改写成 X 推文，并在明确审核后交给 Buffer 的可复用 Skill。

## 安装

将 `tweet-buffer-workflow` 目录复制到 Codex Skills 目录：

```bash
mkdir -p ~/.codex/skills
cp -R tweet-buffer-workflow ~/.codex/skills/
```

## 使用

在 Codex 中调用：

```text
$tweet-buffer-workflow
```

然后提供文字，或提供图片和 Caption。Skill 会让你选择模型和处理方式，再返回审核稿。

可选模型：Grok 4.6、Codex 5.6 Luna、DeepSeek V4 Flash、Gemini 3.8 Flash、Gemini 3.7 Flash。

可选处理方式：

- `润色`：保留原意和事实，适合手机阅读，最多 250 个字符；
- `翻译`：英文转自然简体中文，繁体中文转简体中文，自动结合语境处理。

审核后可选择 `Draft`、`进入队列`、`队列置顶`、`现在发送` 或 `重出`。没有 Buffer 连接时，Skill 只输出最终预览，不会假装已经发布。

## 安全边界

Skill 不包含 Telegram Token、Buffer Key、模型 API Key、数据库或运行代码。外部写入前必须确认目标频道、最终文本和动作；发布结果不确定时先去 Buffer 核对，不要直接重试。

详细规则见 [`references/workflow.md`](references/workflow.md)，中文使用文章见 [`ARTICLE.md`](ARTICLE.md)，英文说明见 [`README.en.md`](README.en.md)。
