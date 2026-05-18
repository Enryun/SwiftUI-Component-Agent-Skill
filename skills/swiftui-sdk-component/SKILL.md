---
name: swiftui-sdk-component
description: >-
  Authors reusable SwiftUI SDK components using the six foundations—separate behavior
  from Config, deliberate state ownership, EnvironmentKey modifiers, composition,
  minimal public API, and sensible defaults. Use when creating or extending components
  in CommonSwiftUI, SwiftUI libraries, or package UI modules—not app feature screens.
---

# SwiftUI SDK Component

Use this skill for **reusable SwiftUI controls** in a package or design system.

## When not to use

- App features (screens, navigation, ViewModel, UseCase) → **swiftui-architecture** skill.
- Swift 6 concurrency / actor isolation fixes → **swift-concurrency** skill.
- One-off screen layout inside a single app with no reuse → plain SwiftUI, no SDK rules.

## The six foundations (core — never skip)

Every reusable component must satisfy **all six**. Read [six-foundations.md](references/six-foundations.md) for detail, examples, and Phase 1 review questions.

| # | Foundation | One-line rule |
|---|------------|----------------|
| 1 | **Separate what it is from how it looks** | Behavior in the view; colors/fonts/radii only in nested `Config` |
| 2 | **Own state deliberately** | Parent `Binding` for outcomes; `@State` private for UI-only; Environment for modifiers |
| 3 | **Modifiers over giant inits** | `init` ≤5 params; optional behavior via `EnvironmentKey` + `View` extension |
| 4 | **Composition, not inheritance** | Wrap `TextField`/`Button`; private subviews; never a monolithic 200-line `body` |
| 5 | **Stable, discoverable public API** | Minimal `public` surface; consistent names; doc comment with one example |
| 6 | **Sensible defaults, explicit customization** | Zero-config usage works; override via `Config` + modifiers, not required params |

**Reject any design that fails a foundation** — revise the Phase 1 proposal before implementing.

## Additional non-negotiables

7. **UX as API:** defer aggressive errors until interaction when appropriate; distinguish empty, neutral, invalid, valid.
8. **One platform fallback path:** centralize `#available` branches; avoid copy-pasted availability checks.
9. **Document + prove:** doc comment with one example block; `SampleCode/…TestView.swift` for every new public component (first section = “Default”, no modifiers).
10. **Recommend-first:** propose API, **foundation review table**, and file tree before creating files.

## Decision tree

```
Building a reusable control?
├─ Parent must react (submit enabled, form valid, selection)?
│  └─ Expose Binding (optional Binding with .constant fallback for simple cases)
├─ Customization at call site?
│  ├─ Few per-instance options → nested Config on init
│  ├─ Fluent optional behavior → EnvironmentKey + View extension
│  └─ App-wide theme → Environment / style protocol (document in shipping.md)
├─ Touches camera / UIWindow / UIKit?
│  └─ Isolate in Internal/ type; document Info.plist in sample README
└─ iOS 16+ API with iOS 15 support?
   └─ Single #available branch with one fallback (see platform-versioning.md)
```

## Scaffold workflow (recommend-first)

**Why phases?** SDK components are hard to change after shipping — `public` API, samples, and docs are contracts. Phase 1 aligns the design (especially the **six foundations**) without code churn. Phase 2 implements only what was approved. Phase 3 verifies before merge.

**Do not create component source files until Phase 1 is approved.**

Full step-by-step checklists: **[scaffold-workflow.md](references/scaffold-workflow.md)** (read when scaffolding).

| Phase | Goal | Output |
|-------|------|--------|
| **1 — Propose** | Design API, state, files, sample | Proposal in chat → **stop for approval** |
| **2 — Implement** | Write code in fixed order | Source + sample + docs |
| **3 — Review** | Verify foundations + build | Summary for user |

### Phase 1 — Propose (summary)

1. Name + one-sentence responsibility  
2. Behavior spec + edge cases  
3. **State map** (Binding / `@State` / Environment)  
4. **Public API** — `init` ≤5, `Config`, modifier table  
5. **Composition plan** — system controls + subview names  
6. **Six foundations review** — all six answers ([six-foundations.md](references/six-foundations.md))  
7. **File tree** + sample sections (Default first)  
8. Platform / accessibility notes  
9. **Stop — wait for approval**

Use the [proposal template](references/scaffold-workflow.md#quick-reference-proposal-template) in scaffold-workflow.md.

### Phase 2 — Implement (summary)

Only after approval. Order:

1. `{Name}+EnvironmentKey.swift` (modifiers)  
2. `Config` types  
3. `public struct {Name}: View` — compose system controls, private subviews  
4. `Internal/` helpers (if UIKit/window/camera)  
5. Platform fallback helper (if needed)  
6. Doc comment + example  
7. `SampleCode/…/{Name}TestView.swift` + navigation wire-up  
8. → Phase 3 review

Full substeps: [scaffold-workflow.md § Phase 2](references/scaffold-workflow.md#phase-2--implement-after-approval).

### Phase 3 — Review (summary)

Run ship checklist below + sample builds. Post files changed + how to run sample.

Full gate: [scaffold-workflow.md § Phase 3](references/scaffold-workflow.md#phase-3--review-before-merge).

## Ship checklist (short)

**Six foundations**

- [ ] **1** Appearance only in `Config` — no brand colors in `body`
- [ ] **2** State map documented — bindings vs `@State` vs Environment
- [ ] **3** `init` ≤5 parameters; behavior exposed as modifiers
- [ ] **4** Composed from system controls + private subviews — not monolithic
- [ ] **5** `public` surface minimal; doc comment with one example
- [ ] **6** Default usage works with no modifiers; sample “Default” section proves it

**Also**

- [ ] Validation/errors deferred until interaction when appropriate
- [ ] `#available` fallback in one place
- [ ] Sample view: Default + custom + failure path
- [ ] Accessibility label/hint for non-standard controls

## Anti-patterns (reject if suggested)

```swift
// BAD — 12-parameter init
public init(title:text:isValid:isSecured:showBorder:borderColor:…)

// GOOD
public init(title: String, text: Binding<String>, isValid: Binding<Bool>? = nil, config: Config = .init())
// + .isMandatory(true) via EnvironmentKey
```

```swift
// BAD — errors on first paint for empty mandatory field
.onAppear { validate(text) } // marks invalid immediately

// GOOD — neutral until interaction (when UX requires it)
.onAppear { validate(text, shouldDeferEmptyMandatory: true) }
```

```swift
// BAD — public helper leaking implementation
public struct ToastWindowHelper { … }

// GOOD
struct ToastWindowHelper { … } // internal
public struct Toast { … }
```

```swift
// BAD — foundation #4: reimplementing TextField, 200-line body
// GOOD — TextField + private clearButton + Config-driven stroke
```

## Additional resources

Read only what you need:

- [**Scaffold workflow (full)**](references/scaffold-workflow.md) — Phase 1 / 2 / 3 checklists
- [**The six foundations**](references/six-foundations.md) — start here
- [Index](references/_index.md)
- [State ownership](references/state-ownership.md)
- [API design](references/api-design.md)
- [Composition](references/composition.md)
- [Folder structure](references/folder-structure.md)
- [UX edge cases](references/ux-edge-cases.md)
- [Platform versioning](references/platform-versioning.md)
- [Documentation](references/documentation.md)
- [Shipping](references/shipping.md)
- [Anti-patterns](references/anti-patterns.md)
- [Golden examples](references/golden-examples.md)

Templates: `../../templates/swift/` (relative to skill install root, or repo `templates/swift/`).
