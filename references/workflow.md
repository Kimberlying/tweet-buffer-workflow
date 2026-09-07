# Workflow Reference

This reference contains the details needed only when the current request reaches model selection, Buffer writing, or image handling.

## Model selection

| User-facing choice | Model ID | Typical role |
| --- | --- | --- |
| Grok 4.6 | `grok-4.6` | X-style Chinese polishing |
| Codex 5.6 Luna | `gpt-5.6-luna` | Strict prompt and format following |
| DeepSeek V4 Flash | `deepseek-v4-flash` | API-based batch rewriting |
| Gemini 3.8 Flash | `gemini-3.8-flash` | User-selected option for natural Chinese/X tone |
| Gemini 3.7 Flash | `gemini-3.7-flash` | Manual fallback when 3.8 is temporarily unavailable |

The Gemini preference is a practical editorial judgment, not a benchmark claim. A `503 UNAVAILABLE` response means the provider is temporarily overloaded or out of capacity; it is different from a personal quota error such as `429`. Do not silently switch models. Let the user choose 3.7 or another provider.

## Text output contract

Return a review block with:

```text
模型：<model>
模式：<润色|翻译>
字符数：<count>/250
事实说明：<brief notes>

<draft text>
```

For `润色`, keep 2–4 short visible paragraphs where the material supports it, use no emoji unless essential to the source, preserve links and proper nouns, and remove a trailing `?` or `？`. Do not add `RT`, `Reply`, `Follow`, or other forced engagement language.

For `翻译`, detect English, Traditional Chinese, or Simplified Chinese before writing. Preserve meaning, tone, names, figures, dates, and factual uncertainty. If the source mixes languages, translate only the parts that need translation and keep proper nouns when appropriate.

## Buffer preflight and actions

Before a Buffer write:

1. Confirm the exact target channel and platform.
2. Read the current queue when the connector provides a read operation.
3. Display the final text and requested action.
4. Wait for the user's explicit action confirmation.

Map actions as follows:

| Action | Buffer behavior | Extra rule |
| --- | --- | --- |
| `Draft` | Save without publishing | Never combine with direct or queue flags |
| `进入队列` | Use next available slot | On capacity error, save the same post as Draft and report queue full |
| `队列置顶` | Place at queue front | Never silently fall back to Draft |
| `现在发送` | Publish immediately | Always treat as a real external write |
| `重出` | Generate a new revision | No Buffer write until reviewed |

If the connector reports an uncertain result, do not repeat the mutation. Ask the user to inspect Buffer for a duplicate first.

## Image checklist

For image input, keep the original and generated image paths separate. Record:

- OCR text and whether it was Traditional Chinese, Simplified Chinese, or English;
- the chosen English handling (`翻译成中文` or `保留英文`);
- third-party IDs removed and the user's explicitly supplied ID, if any;
- generated image path, image revision, and separate image approval;
- a stable public HTTPS URL before attaching the image to Buffer.

Do not publish a media post when either text approval or image approval is missing. Do not fabricate an image URL or claim an upload succeeded.
