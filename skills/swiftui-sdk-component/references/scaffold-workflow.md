# Scaffold workflow (full)

Use when **creating** or **substantially extending** a public SDK component.

## Why phases?

| Phase | Purpose |
|-------|---------|
| **Phase 1 — Propose** | Align on API, state, foundations, and files **before** code. Prevents wrong `public` surface, giant inits, and missing samples. |
| **Phase 2 — Implement** | Build the approved mechanisms in dependency order, then docs and sample. |
| **Phase 3 — Review** | Verify six foundations and ship gate before merge. |

**Rule:** Do not create or edit component source files during Phase 1. Do not skip Phase 1 for “small” components.

---

## Phase 1 — Propose (no file creation)

Output a single **proposal document** in the chat (or PR description). Wait for explicit user approval.

### 1.1 Identity

- [ ] **Component name** — `PascalCase` noun (`PinCodeField`, `ValidationTextField`)
- [ ] **One-sentence responsibility** — what the control does, not how it looks
- [ ] **Package module** — e.g. `CommonSwiftUI`
- [ ] **Category** — TextField, Alert, Button, etc. (for folder + sample grouping)

### 1.2 Behavior spec

- [ ] **Inputs** — what the parent provides (bindings, callbacks, config)
- [ ] **Outputs** — what the parent learns (validity, selection, events)
- [ ] **User interactions** — tap, drag, focus, secure toggle, clear
- [ ] **Edge cases** — empty, mandatory, first paint, disabled, error display rules
- [ ] **Side effects** — camera, haptics, window overlay? If yes → `Internal/` helper + plist keys

### 1.3 State map (foundation #2)

List every piece of state:

| Property / concern | Owner | Type | Notes |
|--------------------|--------|------|-------|
| e.g. `text` | Parent | `Binding<String>` | |
| e.g. `isValid` | Parent | `Binding<Bool>?` | default `.constant(true)` |
| e.g. `hasInteracted` | Component | `@State private` | |
| e.g. `isMandatory` | Environment | modifier | |

### 1.4 Public API draft (foundations #1, #3, #5, #6)

**Focused `init` (required inputs explicit; no fixed parameter limit)**

```text
init(
  title: String,
  text: Binding<String>,
  isValid: Binding<Bool>? = nil,
  config: Config = .init()
)
```

- [ ] List each parameter and why it belongs in initialization
- [ ] List useful nested types, if any: `Config`, `BorderConfig`, `MessageConfig`, etc.
- [ ] Apply preferred patterns where their conditions hold; briefly justify meaningful departures
- [ ] Confirm **default usage** needs no modifiers

**Modifiers (use `+EnvironmentKey.swift` only for inherited settings)**

| Modifier | EnvironmentKey default | Purpose |
|----------|------------------------|---------|
| `.isMandatory(_:message:)` | `(false, "")` | |
| `.onValidate { }` | `nil` | |

- [ ] Optional behavior or appearance has clear scope; per-instance options do not require environment keys

**Callbacks (if any)**

- [ ] Prefer `Binding` for state; use closures for one-shot events (`onCommit`, `onRequestPermission`)

### 1.5 Composition plan (foundation #4)

- [ ] **System control(s)** wrapped — `TextField`, `SecureField`, `Button`, …
- [ ] **Private subviews** — `fieldRow`, `clearButton`, `validationMessages`, …
- [ ] **Subview boundaries** — extract distinct responsibilities where it improves clarity; no fixed body-length limit
- [ ] **Not in scope** — no navigation, no API calls, no screen layout

### 1.6 Six foundations review

Answer every row — copy into proposal:

| # | Question | Answer |
|---|----------|--------|
| 1 | Which appearance API fits, and why? | Standard styling / grouped Config / focused options → … |
| 2 | Who owns each state property? | see state map |
| 3 | Required inputs explicit? Optional customization coherent? Environment intentionally inherited? | … |
| 4 | Built on which system controls / subviews? | … |
| 5 | What is `public` vs `internal`? Doc example sketched? | … |
| 6 | Zero-config example one-liner? | `Component(title: "X", text: $t)` |

If any answer is weak → revise API before approval.

### 1.7 File tree

Adapt to host package. Example:

Include only files needed by the selected mechanisms.

```text
Sources/CommonSwiftUI/Components/PinCodeField/
├── Public/
│   └── PinCodeField.swift
├── Internal/
│   └── PinCodeFieldLayout.swift    # only if needed
└── PinCodeField+EnvironmentKey.swift

SampleCode/SampleCode/TextField/PinCodeFieldTestView.swift
```

- [ ] List every new file path
- [ ] Note files touched in sample app navigation (`ContentView`, menu)

### 1.8 Sample plan (foundation #6, #9)

| Section | Purpose |
|---------|---------|
| **Default** | No modifiers; proves zero-config |
| **Customization** | Supported styling or Config values, if applicable |
| **Modifiers** | Supported modifier chains, if applicable |
| **Failure / invalid** | Error state, empty mandatory, etc. |

- [ ] Preview provider included (`#Preview`)

### 1.9 Platform & accessibility

- [ ] Minimum OS — any `#available` gap? Describe fallback in one sentence
- [ ] VoiceOver — labels for icon-only buttons
- [ ] Dynamic Type — layout still works?

### 1.10 Stop

- [ ] Post full proposal
- [ ] **Wait for user approval** — do not implement until approved

---

## Phase 2 — Implement (after approval)

Implement selected mechanisms in dependency order. Skip mechanisms the component does not need; the checklist does not require identical artifacts for every control.

### 2.1 Environment keys (if inherited settings planned)

File: `{Name}+EnvironmentKey.swift`

- [ ] `EnvironmentKey` struct per modifier concern
- [ ] Sensible `defaultValue` for “off” or safe default
- [ ] `extension EnvironmentValues` (internal)
- [ ] `public extension View` with modifier methods
- [ ] Modifier names match proposal table

### 2.2 Config or style types (if grouped customization planned)

In `{Name}.swift` or split if large:

- [ ] Prefer nested `public struct Config` for grouped component-specific appearance; sub-configs only for useful groups
- [ ] `private(set)` on stored properties where appropriate
- [ ] Default `init()` — neutral colors (`.primary`, `.red`), not app brand
- [ ] No behavior logic inside `Config` — data only

### 2.3 Main view struct

File: `{Name}.swift` (under `Public/` if package uses it)

- [ ] `public struct {Name}: View`
- [ ] `init` matches approved API exactly
- [ ] `@Binding` + optional binding fallbacks
- [ ] `@State private` only for UI-only state from state map
- [ ] `@Environment` only for intentionally inherited settings
- [ ] Prefer suitable system controls; justify custom implementations when needed
- [ ] Private subviews extracted around distinct responsibilities (`private var fieldRow: some View`)
- [ ] Appearance uses the selected modifiers, style, or Config; no dependency on app-specific brand assets
- [ ] Validation / UX rules from proposal (defer errors, etc.)
- [ ] `onChange` / `onAppear` match behavior spec

### 2.4 Internal helpers (only if needed)

File: `Internal/{Name}Helper.swift`

- [ ] UIKit, window, camera, layout math isolated here
- [ ] Types `internal` or `private`
- [ ] No `public` leakage

### 2.5 Platform fallback (if needed)

- [ ] Single helper or `apply` block — not scattered `#available`
- [ ] Document in code comment which OS uses which path

### 2.6 Documentation

- [ ] Doc comment on `public struct {Name}: View`
- [ ] Summary + modifiers list + **one compilable ` ```swift ` example**
- [ ] Public modifier methods documented briefly

### 2.7 Sample view

File: `SampleCode/.../{Name}TestView.swift`

- [ ] `import` package module
- [ ] Section **Default** — no modifiers
- [ ] Section **Customization** (if applicable)
- [ ] Section **Modifiers** (if applicable)
- [ ] Section **Failure / invalid**
- [ ] `#Preview` with `NavigationStack` if needed
- [ ] Wire into sample app menu / `ContentView` if project does that elsewhere

### 2.8 Package hygiene

- [ ] Files under correct `Sources/{Module}/` path
- [ ] No new dependencies without approval
- [ ] Bump package version / changelog only if maintainer requests

---

## Phase 3 — Review (before merge)

### 3.1 Six foundations

- [ ] **1** Appearance API fits the component; preferred patterns applied or meaningful departures justified
- [ ] **2** State map honored in code
- [ ] **3** Focused `init`; environment keys only for inherited settings
- [ ] **4** Suitable system controls reused; distinct responsibilities separated without arbitrary size limits
- [ ] **5** Minimal `public`; doc example present
- [ ] **6** Default sample section works without modifiers

### 3.2 Additional rules

- [ ] UX: empty vs invalid vs neutral per spec
- [ ] `#available` centralized
- [ ] Accessibility labels on custom controls
- [ ] No `print` / debug noise in `body`
- [ ] No app-specific asset names in public API

### 3.3 Build & manual test

- [ ] Sample target builds
- [ ] Run sample on minimum OS simulator if platform branching exists
- [ ] Preview renders

### 3.4 Output summary for user

Post short summary:

- Files created/changed
- Public API one-liner
- How to run sample
- Any follow-ups (tests, README, plist keys)

---

## Extending an existing component

**Small change** (one modifier, one config color): Phase 1 can be a 5-line delta (what changes, foundation impact). Still get approval before editing.

**Breaking change** (rename, remove init param): Full Phase 1 + note migration in proposal.

---

## Quick reference: proposal template

Copy into chat for Phase 1:

```markdown
## Component proposal: [Name]

**Responsibility:** …

### State map
| Property | Owner | Type |
|----------|--------|------|

### init
…

### Modifiers
| Modifier | Default |
|----------|---------|

### Config
…

### Composition
- System controls: …
- Subviews: …

### Six foundations review
| # | Answer |
|---|--------|

### File tree
…

### Sample sections
1. Default
2. …

**Awaiting approval before implementation.**
```
