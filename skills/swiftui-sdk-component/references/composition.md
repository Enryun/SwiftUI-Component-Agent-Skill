# Composition, not inheritance

Foundation **#4**. SwiftUI components are built by **combining** views, not subclassing.

## Do

### Wrap platform controls

| Need | Compose from |
|------|----------------|
| Text input | `TextField`, `SecureField` |
| Actions | `Button`, `Toggle` |
| Lists in component | `ForEach` + small row subview |
| Material / blur | `background`, `.ultraThinMaterial`, versioned helper |

### Extract private subviews

When `body` exceeds ~40 lines or has distinct regions:

```swift
public var body: some View {
    VStack(alignment: .leading) {
        fieldRow
        validationMessages
    }
}

private var fieldRow: some View { … }
private var validationMessages: some View { … }
```

### Keep one responsibility

One public component = one user-facing control (a field, a toast line, a slider). Do not bundle navigation, networking, and layout of an entire screen.

## Do not

| Anti-pattern | Why |
|--------------|-----|
| Subclass another `View` struct | SwiftUI views are structs; use wrapping |
| 200-line `body` | Unreadable; untestable; hard for agents to extend |
| Reimplement `TextField` drawing | Accessibility and keyboard behavior regress |
| Hidden singletons inside component | Breaks reuse and preview |
| Screen-level logic (API calls, routes) | Belongs in app layer, not SDK |

## Optional slots

Use `@ViewBuilder` trailing closures only when the component is a **container** by design (e.g. card with header/footer). Default content should still work without providing a builder.

```swift
public init(
    title: String,
    @ViewBuilder accessory: () -> Accessory = { EmptyView() }
) { … }
```

## Checklist

- [ ] Built on `TextField` / `Button` / system control where applicable
- [ ] `body` delegates to private subviews
- [ ] No screen navigation or use-case calls inside component
- [ ] Previews and sample view compile with minimal setup
