# Platform versioning

## Package minimum

Declare platform in `Package.swift` / podspec (e.g. iOS 15). Do not use APIs above minimum without a fallback.

## Single fallback pattern

```swift
.apply {
    if #available(iOS 16.0, *) {
        $0.background(/* iOS 16 API */, in: .capsule)
    } else {
        $0.background(in: .capsule).overlay(/* iOS 15 fallback */)
    }
}
```

Or a small internal helper:

```swift
enum ComponentStyle {
    @ViewBuilder
    static func capsuleBackground<Content: View>(_ content: Content) -> some View {
        if #available(iOS 16.0, *) {
            content.background(.regularMaterial, in: Capsule())
        } else {
            content.background(.ultraThinMaterial).clipShape(Capsule())
        }
    }
}
```

## Rules

- One branch per feature gap — not ten scattered `#available` checks in `body`.
- Document which path is preferred when the fallback is visually different.
- Test sample on minimum OS simulator when possible.
