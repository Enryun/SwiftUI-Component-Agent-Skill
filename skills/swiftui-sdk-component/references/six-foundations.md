# The six foundations

These six principles define a **reusable** SwiftUI component. Apply every one on every new control. If a proposal violates any foundation, fix the design before writing code.

---

## 1. Separate “what it is” from “how it looks”

| What it is (behavior) | How it looks (appearance) |
|------------------------|---------------------------|
| Validation rules, gestures | Colors, fonts, corner radius |
| Layout contract (label + field + message) | Border width, stroke colors |
| When to show error vs neutral | Message text styling |

**Rule:** No hard-coded brand or semantic colors in `body`. Appearance lives in nested `Config` (and sub-configs like `BorderConfig`, `MessageConfig`).

```swift
// BAD — appearance in body
RoundedRectangle(cornerRadius: 8).stroke(isValid ? .green : .red)

// GOOD
RoundedRectangle(cornerRadius: config.borderConfig.radius)
    .stroke(isValid ? config.borderConfig.validColor : config.borderConfig.invalidColor)
```

---

## 2. Own state deliberately

Every piece of state has exactly one owner. Never duplicate source of truth.

| Owner | Holds |
|-------|--------|
| **Parent** (`Binding`) | Text, selection, `isValid`, values that enable Submit / navigation |
| **Component** (`@State` private) | `hasInteracted`, animation phase, internal toggle (e.g. show password) |
| **Environment** | Optional modifiers, theme, feature flags |

**Rule:** If the parent must react, expose a `Binding`. If only the control’s UI cares, keep `@State` private. Optional bindings use safe defaults (`?? .constant(true)`).

See [state-ownership.md](state-ownership.md).

---

## 3. Prefer modifiers over giant inits

**Rule:** `init` stays small (≤5 parameters). Optional or fluent behavior uses `EnvironmentKey` + `public extension View`.

```swift
// BAD
init(..., isMandatory: Bool, mandatoryMessage: String, showClear: Bool, …)

// GOOD
init(title: String, text: Binding<String>, config: Config = .init())
    .isMandatory(true)
    .clearButtonHidden(false)
```

**Init holds:** identity + primary bindings + `Config`.  
**Modifiers hold:** per-call-site behavior flags, validation closures, visibility toggles.

See [api-design.md](api-design.md) — EnvironmentKey modifiers.

---

## 4. Design for composition, not inheritance

SwiftUI has no real view inheritance. Reuse through **composition**.

**Rule:**

- Wrap system controls (`TextField`, `SecureField`, `Button`, `Toggle`) — do not reimplement them.
- Extract private subviews when `body` grows (`fieldRow`, `clearButton`, `validationMessages`).
- Prefer `@ViewBuilder` slots only when the component is intentionally a container.
- **Never** subclass `UIView` / `NSView` for a SwiftUI-only control unless bridging is the explicit goal.

```swift
// BAD — custom text drawing replacing TextField
Canvas { … draw glyphs … }

// GOOD
TextField("", text: $text)
    .overlay(alignment: .trailing) { clearButton }
```

See [composition.md](composition.md).

---

## 5. Stable, discoverable public API

Consumers learn the component from **names**, **small surface**, and **doc comments** — not by reading implementation.

**Rule:**

- `public`: main `View`, `Config`, nested public types, modifier methods on `View`.
- `internal` / `private`: helpers, window managers, layout math, ViewModels.
- Consistent naming: `{Name}.Config`, `{Name}+EnvironmentKey.swift`, `{Name}TestView`.
- One compilable usage example in the type’s doc comment.
- Avoid breaking renames without a major version bump.

See [shipping.md](shipping.md), [documentation.md](documentation.md), [folder-structure.md](folder-structure.md).

---

## 6. Sensible defaults, explicit customization

**Rule:** The component must work with **zero extra configuration** — `Component(title: "Email", text: $email)` compiles and looks acceptable.

Customization is **explicit**:

- Appearance → `Config` parameter or sub-configs with defaults.
- Behavior → modifiers with documented `EnvironmentKey` defaults.
- Outcomes → optional `Binding` with fallback when parent does not care.

```swift
public init(
    title: String,
    text: Binding<String>,
    isValid: Binding<Bool>? = nil,
    config: Config = .init()  // all sub-configs also default
) { … }
```

**Test:** In the sample view, the first section is always “Default” with no modifiers.

---

## Foundation review (use in Phase 1)

Before approval, confirm in the proposal:

| # | Question |
|---|----------|
| 1 | Where is appearance? (Config only) |
| 2 | State map — who owns each property? |
| 3 | Init param count ≤5? Modifiers listed? |
| 4 | Built on system controls / subviews, not monolith? |
| 5 | What is `public` vs `internal`? Doc example included? |
| 6 | Does default usage need zero modifiers? |
