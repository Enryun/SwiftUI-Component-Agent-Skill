# Component checks (human index)

Use with [`skills/swiftui-sdk-component/SKILL.md`](skills/swiftui-sdk-component/SKILL.md).

## The six foundations (core — never skip)

Full detail: [`references/six-foundations.md`](skills/swiftui-sdk-component/references/six-foundations.md)

| # | Foundation | Pass criteria |
|---|------------|----------------|
| 1 | **Separate what it is from how it looks** | Appearance uses suitable modifiers, styles, or `Config` |
| 2 | **Own state deliberately** | Parent `Binding` for outcomes; `@State` for UI-only; no duplicate truth |
| 3 | **Focused initializers and customization** | Required inputs explicit; environment only for inherited settings |
| 4 | **Composition, not inheritance** | System controls + private subviews; no monolithic `body` |
| 5 | **Stable, discoverable public API** | Minimal `public`; consistent names; doc example on type |
| 6 | **Sensible defaults, explicit customization** | Works with zero modifiers; sample “Default” section proves it |

**Gate:** Do not merge if any foundation fails.

Review the principles, not a fixed set of artifacts. Config types, environment keys, and extracted subviews are conditional; parameter counts and body length are review signals, not limits. Preserve compatible public APIs.

Apply preferred patterns when their conditions hold: standard controls and styling first, nested Config for grouped component-specific appearance, focused modifiers for optional customization, environment for inherited settings, and subviews for distinct responsibilities. Briefly justify meaningful departures in the proposal.

## Additional non-negotiables

| # | Rule |
|---|------|
| 7 | **UX edge cases** — no errors before interaction when appropriate; empty ≠ invalid |
| 8 | **Platform fallbacks** — one `#available` path, not scattered checks |
| 9 | **Documentation** — doc comment + one compilable usage example |
| 10 | **Sample screen** — `SampleCode/…TestView.swift` (Default + custom + failure) |
| 11 | **Side effects isolated** — camera, windows, haptics in dedicated internal types |
| 12 | **Recommend-first** — Phase 1 proposal approved before files |

## Scaffold workflow

Full phases: [`references/scaffold-workflow.md`](skills/swiftui-sdk-component/references/scaffold-workflow.md)

| Phase | Rule |
|-------|------|
| 1 | Proposal approved before any component files |
| 2 | Implement selected mechanisms in dependency order, then docs and sample |
| 3 | Ship checklist + sample builds |

## Scaffold gate (before merge)

**Foundations**

- [ ] Foundation 1: Suitable appearance API; no dependency on an app's brand assets
- [ ] Foundation 2: State map written (Binding / @State / Environment)
- [ ] Foundation 3: Focused `init`; environment keys only for inherited settings
- [ ] Foundation 4: Reuses suitable system controls; subviews extracted where responsibilities warrant it
- [ ] Foundation 5: `public` only consumer API; doc comment with example
- [ ] Foundation 6: Default usage with no modifiers; sample first section = “Default”

**Also**

- [ ] Phase 1 proposal approved (API, foundation review, file tree, sample name)
- [ ] `public struct [Name]: View`
- [ ] Config or style only where useful; access control appropriate to the chosen API
- [ ] Sample: happy + supported customization + failure path
- [ ] Accessibility: label/hint for custom controls

## When to read references

| Task | File |
|------|------|
| **All six foundations** | `references/six-foundations.md` |
| Composition (#4) | `references/composition.md` |
| Bindings vs `@State` (#2) | `references/state-ownership.md` |
| Config vs modifiers (#1, #3, #6) | `references/api-design.md` |
| Folder layout (#5) | `references/folder-structure.md` |
| Validation / first paint | `references/ux-edge-cases.md` |
| iOS version branches | `references/platform-versioning.md` |
| Doc comments (#5) | `references/documentation.md` |
| `public` / SPM (#5) | `references/shipping.md` |
| What not to do | `references/anti-patterns.md` |
| Real-world patterns | `references/golden-examples.md` |
