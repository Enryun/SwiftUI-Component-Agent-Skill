# UX edge cases

Treat these as part of the public contract, not polish.

## Validation timing

**Defer errors** for empty mandatory fields until the user has interacted, when the design calls for a neutral first paint:

```swift
private func validate(_ value: String, shouldDeferEmptyMandatory: Bool = false) {
    if shouldDeferEmptyMandatory && isMandatory.0 && value.isEmpty && !hasInteracted {
        isValid = true
        validationMessage = ""
        return
    }
    // … full validation
}
```

Set `hasInteracted = true` on first `onChange` of bound text.

## States

Distinguish:

| State | UI |
|-------|-----|
| Empty, not yet touched | Neutral border, no error copy |
| Empty, mandatory, touched | Invalid + message |
| Invalid rule | Invalid + specific message |
| Valid with hint | Valid border + success/hint copy |

## Secure fields

- Toggle secure entry with internal `@State` if `isSecured` is fixed at init; expose modifier to show/hide secure toggle button.
- Do not log or print bound secure text.

## Accessibility

- Provide accessibility labels for icon-only buttons (clear, show password).
- Support Dynamic Type — avoid fixed heights that clip large content.
- Ensure error text is associated with the field (`accessibilityLabel` / hint) when using custom layouts.

## Keyboard

- Support `@FocusState` from the parent; do not fight focus unless the component owns focus entirely.
- Respect `.textContentType`, `.keyboardType` via standard SwiftUI modifiers on the inner field.
