# gemma-action-demo

A GitHub Actions demo that runs **[Gemma](https://ai.google.dev/gemma)** locally on a free GitHub-hosted runner using the **[LiteRT-LM CLI](https://ai.google.dev/edge/litert-lm/cli)** — no GPU, no external API calls, just on-runner CPU inference.

## How to use

1. Open any issue or PR in this repo (or comment on an existing one).
2. Write `@gemma` followed by your prompt anywhere in the body:

   ```
   @gemma What is the capital of France?
   ```

3. Watch the [Actions tab](../../actions) — a **Gemma Chat** workflow run will appear.
4. Within a few minutes the bot replies as a comment on your issue/PR.

### Choosing a model

Append `:MODEL` right after `@gemma` to pick a model. Omit it to use the default (E4B).

| Syntax | Model | Size | Speed |
|---|---|---|---|
| `@gemma …` *(default)* | `gemma-4-E4B-it` | ~2 GB | slower, more capable |
| `@gemma:E4B …` | `gemma-4-E4B-it` | ~2 GB | slower, more capable |
| `@gemma:E2B …` | `gemma-4-E2B-it` | ~1 GB | faster, lighter |

Examples:

```
@gemma:E2B Summarize this issue in one sentence.

@gemma:E4B Write a detailed fix plan for the bug described above.
```

## Context awareness

The bot automatically fetches the issue title, body, and up to the last 5 comments before passing everything to the model. You don't need to repeat context in your prompt.

## Notes

- **First run:** ~3–10 minutes — the model is downloaded from HuggingFace and cached for all subsequent runs.
- **Cached runs:** materially faster (model download skipped). Each model variant (E2B/E4B) has its own cache.
- **Owner-only:** only comments from `arash77` trigger the bot, preventing misuse of free Actions minutes.
- **Runner:** `ubuntu-latest` (CPU only, ~16 GB RAM).

## Stack

| Component | Tool |
|---|---|
| Model runtime | [LiteRT-LM CLI](https://ai.google.dev/edge/litert-lm/cli) |
| Python tooling | [uv](https://docs.astral.sh/uv/) |
| Model weights (E4B) | [litert-community/gemma-4-E4B-it-litert-lm](https://huggingface.co/litert-community/gemma-4-E4B-it-litert-lm) |
| Model weights (E2B) | [litert-community/gemma-4-E2B-it-litert-lm](https://huggingface.co/litert-community/gemma-4-E2B-it-litert-lm) |
| CI | GitHub Actions (`ubuntu-latest`) |

## Potential issues

If the HuggingFace model repo requires gating, the workflow will fail with an auth error. In that case, create a HuggingFace token and add it as a repo secret named `HF_TOKEN`.