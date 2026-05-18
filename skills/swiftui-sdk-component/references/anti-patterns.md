# Anti-patterns

Mapped to [six-foundations.md](six-foundations.md). Reject designs that match these patterns.

## Foundation #1 — appearance in body

```swift
// BAD — hard-coded colors in body (violates #1)
.stroke(isValid ? .green : .red)

// GOOD — Config-driven
.stroke(isValid ? config.borderConfig.validColor : config.borderConfig.invalidColor)
```

## Foundation #3 — giant init

```swift
// BAD
public init(title:text:isMandatory:mandatoryMessage:showClear:borderColor:…)

// GOOD — small init + modifiers
public init(title: String, text: Binding<String>, config: Config = .init())
```

## Foundation #4 — monolithic body

```swift
// BAD — 200-line body with inline validation, buttons, animations

// GOOD — private computed properties / subviews
private var fieldRow: some View { … }
private var validationMessages: some View { … }
```

## Leaking app theme

```swift
// BAD
.foregroundStyle(Color("BrandPrimary", bundle: .main))

// GOOD
.foregroundStyle(config.labelColor)
// host app: .init(labelColor: Color("BrandPrimary", bundle: .main))
```

## Giant public surface

```swift
// BAD
public var internalViewModel: FooViewModel
public func reload() { … }

// GOOD
public struct PinCodeField: View { … }
// internal: PinCodeFieldViewModel
```

## EnvironmentKey without default

```swift
// BAD
struct MyKey: EnvironmentKey {
    static var defaultValue: String?  // nil forces optional handling everywhere
}

// GOOD — explicit default matching “off” behavior
static var defaultValue: Bool = true  // e.g. clear button hidden by default
```

## Validation on every keystroke without need

For expensive validation (regex, network), debounce or validate on submit/blur unless live feedback is a requirement.

## Screen logic in SDK component

```swift
// BAD — navigates to settings, calls API singleton
Button("Fix") { UIApplication.shared.open(settingsURL) }

// GOOD — callback or Binding
var onRequestPermission: (() -> Void)?
```

## Copy-paste between components

Extract shared styling to `Extension+Helper` or a small internal `ComponentStyle` — do not duplicate identical `#available` blocks in ten files (see platform-versioning.md).
