# Contributing to SwiftUI Component Agent Skill

Thanks for helping improve this Agent Skills package. Keep contributions focused on **reusable SwiftUI SDK components** (API design, state, samples, shipping).

## Scope

**In scope**

- Component API rules, folder structure, naming, anti-patterns
- Scaffold / review workflows in `SKILL.md`
- Neutral Swift examples (no app-specific product names)
- References for Config, EnvironmentKey, validation UX, `#available` fallbacks

**Out of scope**

- App architecture (ViewModel, UseCase, Factory) — use SwiftUI-Architecture skill
- Full sample Xcode apps (unless proposed as optional separate folder)
- Build optimization, CI, unrelated iOS topics

## Skill quality

- Every `SKILL.md` must have valid YAML `name` and `description` frontmatter.
- Keep `SKILL.md` concise (<500 lines); put depth in `references/`.
- Preserve **recommend-first** behavior: propose API and file tree before creating files.
- Generalize examples — avoid names tied to a single commercial app.

## Pull requests

1. Branch from `main`.
2. Update `SKILL.md` and any affected `references/` together.
3. If you add checks, update `COMPONENT-CHECKS.md`.
4. Describe what you changed and why.

## Resources

- [Agent Skills format](https://agentskills.io)
- Cursor skills: install under `~/.cursor/skills/` or `.cursor/skills/`
- Inspiration: [Swift Concurrency Agent Skill](https://github.com/avdlee/swift-concurrency-agent-skill)
