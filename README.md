# gemma-action-demo

A GitHub Actions demo that runs **[Gemma](https://ai.google.dev/gemma)** locally on a free GitHub-hosted runner using the **[LiteRT-LM CLI](https://ai.google.dev/edge/litert-lm/cli)** — no GPU, no external API calls, just on-runner CPU inference.

## How to use

1. Open any issue in this repo (or comment on an existing one / a PR).
2. Write `@gemma` followed by your prompt anywhere in the body:

   ```
   @gemma What is the capital of France?
   ```

3. Watch the [Actions tab](../../actions) — a **Gemma Chat** workflow run will appear.
4. Within a few minutes the bot replies as a comment on your issue/PR.

## Notes

- **First run:** ~3–10 minutes — the ~2 GB model is downloaded from HuggingFace and then cached for all subsequent runs.
- **Cached runs:** materially faster (model download skipped).
- **Owner-only:** only comments from `arash77` trigger the bot, preventing misuse of free Actions minutes.
- **Model:** `gemma-4-E2B-it` (2B instruction-tuned) via `litert-community/gemma-4-E2B-it-litert-lm` on HuggingFace.
- **Runner:** `ubuntu-latest` (CPU only, ~16 GB RAM).

## Stack

| Component | Tool |
|---|---|
| Model runtime | [LiteRT-LM CLI](https://ai.google.dev/edge/litert-lm/cli) |
| Python tooling | [uv](https://docs.astral.sh/uv/) |
| Model weights | [litert-community/gemma-4-E2B-it-litert-lm](https://huggingface.co/litert-community/gemma-4-E2B-it-litert-lm) |
| CI | GitHub Actions (`ubuntu-latest`) |

## Potential issues

If the HuggingFace model repo requires gating, the workflow will fail with an auth error. In that case, create a HuggingFace token and add it as a repo secret named `HF_TOKEN`, then add `-e HF_TOKEN` to the `litert-lm run` command in the workflow.