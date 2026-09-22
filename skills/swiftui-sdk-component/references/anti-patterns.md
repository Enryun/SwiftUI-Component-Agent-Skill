# Anti-patterns

Mapped to [six-foundations.md](six-foundations.md). Reject designs that match these patterns.

## Foundation #1 — inflexible appearance when customization is needed

```swift
// BAD — fixed colors when consumers need validation styling
.stroke(isValid ? .green : .red)

// GOOD — one option for grouped styling; standard styles can also work
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
// BAD — body mixes unrelated validation, button, and animation responsibilities

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

## Environment settings without a meaningful contract

```swift
// BAD — broad name with no clear component scope or consumer
func exampleFeatureEnabled(_ enabled: Bool) -> some View { self }

// GOOD — a scoped setting, documented default, and a component that reads it
// See the paired component and environment templates.
```

A nil default is valid when absence has defined meaning. Do not replace meaningful optional values merely to avoid optional handling.

## Validation on every keystroke without need

For expensive validation (regex, network), debounce or validate on submit/blur unless live feedback is a requirement.

## Screen logic in SDK component

```swift
// BAD — navigates to settings, calls API singleton
Button("Fix") { UIApplication.shared.open(settingsURL) }

// GOOD — a focused action request, when the component needs this capability
var onRequestPermission: (() -> Void)?
```

## Copy-paste between components

Extract shared styling to `Extension+Helper` or a small internal `ComponentStyle` — do not duplicate identical `#available` blocks in ten files (see platform-versioning.md).
