## Skills

This repository includes local Agent Skills submodules:

- `.ai/openai-skills`
- `.ai/codex-skills`
- `.ai/agent-skills`
- `.ai/dimillian-skills`

When a task matches a reusable workflow, first check the local skill catalog before
implementing from scratch:

- `.ai/openai-skills/skills/.system/*/SKILL.md`
- `.ai/openai-skills/skills/.curated/*/SKILL.md`
- `.ai/codex-skills/skills/*/SKILL.md`
- `.ai/agent-skills/skills/*/SKILL.md`
- `.ai/dimillian-skills/*/SKILL.md`

Read the relevant `SKILL.md` before acting, then follow its instructions and reuse
any scripts, assets, templates, or references from that skill directory when they
apply.

If multiple skills could apply, choose the smallest set that covers the task and
state which skill files are being used. If no skill matches, continue with the
normal repository conventions.
