# Tweet Buffer Workflow

**English | [中文](README.md)**

A reusable Skill that turns supplied material into X-ready posts and sends them to Buffer only after explicit review.

## Install

Copy the `tweet-buffer-workflow` directory into the Codex Skills directory:

```bash
mkdir -p ~/.codex/skills
cp -R tweet-buffer-workflow ~/.codex/skills/
```

## Use

Invoke it in Codex:

```text
$tweet-buffer-workflow
```

Then provide text, or an image with a caption. The Skill guides model and mode selection before returning a reviewable draft.

Available models: Grok 4.6, Codex 5.6 Luna, DeepSeek V4 Flash, Gemini 3.8 Flash, and Gemini 3.7 Flash.

Available modes:

- `润色` (polish): preserve meaning and facts, optimize for mobile reading, and stay within 250 characters;
- `翻译` (translate): detect the source language, translate English into natural Simplified Chinese, and convert Traditional Chinese into Simplified Chinese with contextual phrasing.

After review, choose `Draft`, `进入队列` (queue), `队列置顶` (prioritize), `现在发送` (publish now), or `重出` (rewrite). Without a Buffer connection, the Skill returns a final preview and never claims that a post was published.

## Safety boundaries

The Skill contains no Telegram token, Buffer key, model API key, database, or runtime service code. Before an external write, confirm the target channel, final text, and action. If a publish result is uncertain, inspect Buffer before retrying.

See [`references/workflow.md`](references/workflow.md) for detailed rules and [`ARTICLE.md`](ARTICLE.md) for the Chinese usage article.
