# API design

Covers foundations **#1** (appearance in Config), **#3** (modifiers), and **#6** (defaults). See [six-foundations.md](six-foundations.md) for the full set.

## Init

- Keep **≤5** parameters on `init`.
- Required: identity (title/label), primary `Binding`(s).
- Optional: `config: Config = .init()`, `isSecured`, feature flags with defaults.

## Nested Config

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

For optional, fluent behavior:

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

**Use EnvironmentKey when:** behavior varies per call site, many optional flags, or you want modifier chaining.

**Use Config when:** appearance is fixed at construction and rarely changes.

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
- Never ship a single 200-line `body` for a public component.
