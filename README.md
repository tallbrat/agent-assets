# agent-assets

External runtime skills, prompts, and policies repository for personal-os.

Use this repo with:
- `SKILLS_REPO=/srv/vaults/agent-assets`
- `SKILLS_BRANCH=main`
- `SKILLS_PATH=.`

The runtime asset root is the repository root. Top-level runtime directories are:
- `skills/`
- `prompts/`
- `policies/`
- `templates/`
- `schemas/`

`bootstrap/` contains copies of repo-local Copilot customization assets from personal-os. Those are reference assets for development workflow, not the runtime curator prompt pack.

`examples/` holds small reference cases for future prompt and skill tuning.
