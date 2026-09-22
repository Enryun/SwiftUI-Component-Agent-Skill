# SwiftUI Component Agent Skill

Expert guidance for any AI coding tool that supports the [Agent Skills](https://agentskills.io) open format — **reusable SwiftUI SDK components** with stable APIs, clear state boundaries, environment modifiers, samples, and documented edge cases.

Distilled from practices used in [CommonSwiftUI](https://github.com/Enryun/Common_SwiftUI) and general SwiftUI library authoring.

## Who this is for

- Authors adding or extending controls in a SwiftUI **package** or design system
- Teams building `CommonSwiftUI`-style component libraries
- Developers reviewing public API surface, samples, or validation UX in reusable views

## When to use

- Creating a new reusable control (`ValidationTextField`, toast, slider, alert wrapper)
- Extending an existing SDK component (new modifier, config, validation rule)
- Reviewing whether a view belongs in an app vs a library module

## When not to use

- Scaffolding an **app feature** (screens, ViewModels, UseCases) → use **[SwiftUI-Architecture](https://github.com/Enryun/SwiftUI-Architecture)** or your app architecture skill
- Swift Concurrency migration / actor isolation → use [AvdLee's swift-concurrency skill](https://github.com/avdlee/swift-concurrency-agent-skill)

## Quick start

### Cursor (personal)

```bash
cp -R skills/swiftui-sdk-component ~/.cursor/skills/
```

Open your Swift package or Xcode project and say:

> Use the **swiftui-sdk-component** skill to add a `PinCodeField` to the package. Propose bindings, Config, modifiers, file tree, and sample view first; do not create files until I approve.

### Project-local (share with team)

```bash
mkdir -p .cursor/skills
cp -R skills/swiftui-sdk-component .cursor/skills/
```

Optional: copy [`templates/component.mdc`](templates/component.mdc) to `.cursor/rules/` as an opt-in rule (`alwaysApply: false`).

### Claude Code / Codex / other tools

Copy `skills/swiftui-sdk-component/` into your tool's skills directory. The skill format is portable; only the install path differs.

The skill folder includes its Swift templates and all references; no sibling repository folders are required. See [template instructions](skills/swiftui-sdk-component/templates/README.md).

## Included skill

| Skill | Purpose |
|-------|---------|
| `swiftui-sdk-component` | Component API design, scaffold workflow, checklists, anti-patterns |

## Repository layout

```
SwiftUI-Component-Agent-Skill/
├── README.md
├── COMPONENT-CHECKS.md          # Human-readable index of rules
├── AGENTS.md
├── skills/swiftui-sdk-component/
│   ├── SKILL.md                 # Agent entrypoint
│   ├── references/              # Detailed guidance
│   └── templates/               # Installed Swift scaffolds and instructions
└── templates/
    └── component.mdc            # Optional Cursor rule template
```

## The six foundations

Review all six principles, choosing implementations appropriate to the component (detail in the skill):

1. Separate **what it is** from **how it looks** (modifiers, styles, or `Config` as appropriate)
2. **Own state deliberately** (`Binding` vs `@State` vs Environment)
3. **Focused initializers** and customization APIs (`EnvironmentKey` for inherited settings)
4. **Composition**, not inheritance (wrap system controls, private subviews)
5. **Stable, discoverable** public API (minimal `public`, doc example)
6. **Sensible defaults**, explicit customization (zero-config works)

## How it works (three phases)

| Phase | What happens |
|-------|----------------|
| **1 — Design** | Inspect host conventions and explain material API/ownership decisions |
| **2 — Implement** | Implement the authorized scope, docs, and relevant sample usage |
| **3 — Review** | Verify foundations, compatibility, consumer usage, and relevant behavior |

Scale the workflow to the change. An implementation request already authorizes ordinary work; stop for proposal-only requests or unresolved material decisions. Preserve existing public APIs and avoid unsolicited releases. The quick-start prompt above explicitly requests an approval pause; that pause is not required for every task.

- Summary: [`SKILL.md`](skills/swiftui-sdk-component/SKILL.md)
- **Full checklists:** [`references/scaffold-workflow.md`](skills/swiftui-sdk-component/references/scaffold-workflow.md)

## Related skills

| Skill | Scope |
|-------|--------|
| **swiftui-sdk-component** (this repo) | Library controls, Config, EnvironmentKey, samples |
| **swiftui-architecture** | App features: Factory → UseCase → ViewModel → View |
| **swift-concurrency** | Actors, Sendable, Swift 6 migration |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT — see [LICENSE](LICENSE).
