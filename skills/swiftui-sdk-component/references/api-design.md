# API design

Covers foundations **#1** (appearance customization), **#3** (modifiers), and **#6** (defaults). See [six-foundations.md](six-foundations.md) for the full set and enforcement policy.

## Init

- Prefer a focused initializer. More than five parameters prompts a review of cohesion, not automatic rejection or a forced Config/environment refactor.
- Required: the data, bindings, actions, or content needed for the component's responsibility.
- Optional: `config: Config = .init()`, `isSecured`, feature flags with defaults.

## Nested Config

Prefer a nested Config when related component-specific appearance settings benefit from grouping. Reuse a suitable existing style API first; use direct parameters for a few simple options. Neither Config nor sub-config types are required for every component. Explain meaningful departures from these defaults in the proposal.

```swift
public struct Config {
    private(set) var messageConfig: MessageConfig
    private(set) var borderConfig: BorderConfig

    public init(
        messageConfig: MessageConfig = .init(),
        borderConfig: BorderConfig = .init()
    ) {
        self.messageConfig = messageConfig
        self.borderConfig = borderConfig
    }
}
```

- Sub-configs (`MessageConfig`, `BorderConfig`) group related appearance.
- Use `private(set)` on stored properties when consumers should not mutate after init.
- Defaults use semantic or neutral colors (`.primary`, `.red`), not app brand hex.

## EnvironmentKey modifiers

For settings intentionally inherited by descendants:

1. Define `EnvironmentKey` + default in `{Name}+EnvironmentKey.swift`.
2. Extend `EnvironmentValues` (internal).
3. Public `extension View` with modifier method.

```swift
public extension View {
    func isMandatory(_ value: Bool, message: String = "Required") -> some View {
        environment(\.isMandatory, (value, message))
    }
}
```

**Use EnvironmentKey when:** the setting should propagate through a view subtree. For per-instance behavior, consider a parameter or ordinary modifier first. Modifier chaining alone is not a reason to add environment state.

**Use Config when:** a group of related options forms a coherent value. It need not be immutable over the lifetime of the component; ordinary SwiftUI input updates should be honored.

## Result-based validation

For custom rules, prefer `Result<SuccessPayload, Error>` or a small enum over throwing from `body`:

```swift
.onValidate { value in
    value.count >= 6 ? .success("OK") : .failure(MyError.tooShort)
}
```

## Composition

Foundation **#4** — see [composition.md](composition.md) for full rules, checklist, and anti-patterns.

- Wrap `TextField` / `SecureField` / `Button` instead of reimplementing.
- Split into private subviews (`clearButton`, `messageRow`) when `body` grows.
- Extract coherent regions when it improves readability, reuse, or state ownership; there is no fixed body-length limit.
