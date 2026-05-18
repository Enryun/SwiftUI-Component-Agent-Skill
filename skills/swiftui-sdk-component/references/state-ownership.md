# State ownership

Foundation **#2** — own state deliberately. See [six-foundations.md](six-foundations.md).

## Table

| State | Owner | Example |
|-------|--------|---------|
| Text, selection, `isValid` | Parent `@Binding` | `$email`, `$isFormValid` |
| `hasInteracted`, animation phase | Component `@State` private | defer validation |
| Clear/secure button visibility defaults | `EnvironmentKey` | `.clearButtonHidden(false)` |
| App theme / feature flags | `Environment` | semantic colors |

## Optional bindings

When the parent may not care about an outcome, use an optional `Binding` with a safe default:

```swift
public init(
    title: String,
    text: Binding<String>,
    isValid isValidBinding: Binding<Bool>? = nil,
    config: Config = .init()
) {
    self._isValidBinding = isValidBinding ?? .constant(true)
}
```

Sync outward when internal validity changes:

```swift
@State private var isValid: Bool = true {
    didSet { isValidBinding = isValid }
}
```

## Do not

- Put parent business rules inside the component (e.g. “user is premium”) — pass results via bindings or closures.
- Use `@ObservedObject` for simple controls unless the component truly wraps a long-lived model.
