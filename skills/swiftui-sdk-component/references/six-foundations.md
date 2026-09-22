# The six foundations

These six principles guide reusable SwiftUI components. Review each principle, while choosing the smallest implementation that meets the component's needs. CommonSwiftUI patterns are preferred examples, not a requirement to generate the same files and types for every control. Do not redesign a compatible existing public API merely to match a pattern.

---

## How strongly to apply these rules

- **Enforce principles:** one source of truth, explicit required inputs, appropriate access control, behavior independent of app branding, and usable defaults.
- **Recommend a preferred implementation:** use suitable system controls and standard styling first; prefer a nested Config for coherent groups of component-specific appearance settings; prefer focused modifiers for optional customization; use environment keys for intentionally inherited settings; extract subviews around distinct responsibilities.
- **Allow justified alternatives:** follow a compatible existing API or choose a simpler mechanism when it better fits the component. Explain meaningful departures briefly in the proposal. Do not add types, files, or indirection solely to match an example.

Conditional does not mean arbitrary: apply the preferred pattern when its conditions hold, and review whether an alternative preserves the principles.

---

## 1. Separate “what it is” from “how it looks”

| What it is (behavior) | How it looks (appearance) |
|------------------------|---------------------------|
| Validation rules, gestures | Colors, fonts, corner radius |
| Layout contract (label + field + message) | Border width, stroke colors |
| When to show error vs neutral | Message text styling |

**Rule:** Keep behavior independent of host-app branding. Prefer standard SwiftUI modifiers or existing styles for simple controls. A Config is useful when several related appearance settings form a coherent customization API; nesting and sub-configs are optional. Semantic defaults may be used directly when they fit the component. Do not expose every internal spacing value as configuration.

```swift
// BAD — fixed styling when consumers need to customize validation appearance
RoundedRectangle(cornerRadius: 8).stroke(isValid ? .green : .red)

// PREFERRED for a coherent group of component-specific appearance settings
RoundedRectangle(cornerRadius: config.borderConfig.radius)
    .stroke(isValid ? config.borderConfig.validColor : config.borderConfig.invalidColor)
```

---

## 2. Own state deliberately

Every piece of state has exactly one owner. Never duplicate source of truth.

| Owner | Holds |
|-------|--------|
| **Parent** (value or `Binding`) | Values for read-only inputs; Binding for text or selection the component edits |
| **Component** (`@State` private) | `hasInteracted`, animation phase, internal toggle (e.g. show password) |
| **Environment** | Optional modifiers, theme, feature flags |

**Rule:** Use values for read-only inputs, Binding for two-way edits to parent-owned state, and focused callbacks for actions or notifications. Keep component-owned transient state private. Derive results where practical; do not mirror bindings into local state or use constant bindings as editable fallbacks.

See [state-ownership.md](state-ownership.md).

---

## 3. Prefer modifiers over giant inits

**Rule:** Keep initialization focused, with required inputs explicit. Five parameters is a review prompt, not a hard limit. A longer coherent initializer can be clearer than hiding essential inputs in modifiers. Optional behavior may use direct parameters or ordinary modifiers; use EnvironmentKey when values are intended to be inherited by descendants.

```swift
// BAD — mixes required inputs with many unrelated customization options
init(..., isMandatory: Bool, mandatoryMessage: String, showClear: Bool, …)

// PREFERRED — focused init with coherent grouped appearance
init(title: String, text: Binding<String>, config: Config = .init())
```

**Init typically holds:** required data, bindings, actions/content, and optional Config when useful.  
**Modifiers may hold:** optional behavior or styling with clear scope. Fluent syntax alone does not require EnvironmentKey.

See [api-design.md](api-design.md) — EnvironmentKey modifiers.

---

## 4. Design for composition, not inheritance

SwiftUI has no real view inheritance. Reuse through **composition**.

**Rule:**

- Prefer system controls (`TextField`, `SecureField`, `Button`, `Toggle`) when they meet the interaction contract. Custom drawing or bridging is valid when the control requires it; account for accessibility and platform behavior.
- Extract private subviews when `body` grows (`fieldRow`, `clearButton`, `validationMessages`).
- Use `@ViewBuilder` slots when callers need custom visual content, including labels or accessories on controls. Do not add unused slots for hypothetical flexibility.
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
- Follow existing public naming conventions; use `{Name}.Config` and `{Name}+EnvironmentKey.swift` when those mechanisms exist.
- One compilable usage example in the type’s doc comment.
- Preserve existing public names; breaking changes require an authorized migration. Version bumps and releases are separate requested work.

See [shipping.md](shipping.md), [documentation.md](documentation.md), [folder-structure.md](folder-structure.md).

---

## 6. Sensible defaults, explicit customization

**Rule:** The component should work with its required inputs and content, without extra styling setup. Required actions, data, or label/content closures are not failures of sensible defaults.

Customization is **explicit**:

- Appearance → standard modifiers, styles, or Config with defaults as appropriate.
- Behavior → focused parameters or modifiers; inherited options use documented environment defaults.
- Actions and notifications → focused callbacks when needed; Binding only for genuine two-way editing.

```swift
public init(
    title: String,
    text: Binding<String>,
    config: Config = .init()  // all sub-configs also default
) { … }
```

**Check:** Demonstrate basic use without styling setup in the host's existing sample or preview arrangement.

---

## Foundation review

Review these questions when designing the API; communicate the decisions relevant to the change:

| # | Question |
|---|----------|
| 1 | Which appearance API fits this component, and why? |
| 2 | State map — who owns each property? |
| 3 | Are required inputs explicit and options coherent? Are environment settings intentionally inherited? |
| 4 | Built on system controls / subviews, not monolith? |
| 5 | What is `public` vs `internal`? Doc example included? |
| 6 | Does default usage need zero modifiers? |
