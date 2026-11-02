---
description: Run a read-only adversarial Codex review of exactly the files or change you name
argument-hint: '<files, change, or doc to review>'
disable-model-invocation: true
allowed-tools: Bash(node:*)
---

Run a read-only adversarial Codex review of exactly what the user names. Do not review the git working tree, and do not paste file contents into the prompt: Codex reads the files itself.

Review target (verbatim from the user): $ARGUMENTS

Run this in the foreground and return stdout verbatim with no commentary:

```bash
echo "[codex-review-files] cwd=$(pwd) target=$ARGUMENTS"
node "${CLAUDE_PLUGIN_ROOT}/scripts/codex-companion.mjs" task --fresh "Adversarial review, read-only. Review exactly this and nothing else: $ARGUMENTS. Read the named files yourself, and other repo files only as needed to verify claims. Never read .env* files. Challenge whether this is the right approach, what assumptions it depends on, and where it fails under real-world conditions. Output findings ranked by severity, each with a concrete failure scenario."
```

Rules:
- Review-only: never fix, patch, or offer to change anything.
- Do not add `--write`.
- If the command fails, show the exact error; do not fall back to a git-diff based review.
