# UX edge cases

Apply only the sections relevant to the component. These are examples of interaction contracts, not requirements to add validation or text-field behavior to every control.

## Validation timing

For components that validate input, separate the validation result from error visibility. Hiding an error must not mark invalid input as valid. Choose error timing to fit the interaction: editing, leaving the field, or a submit attempt.

Example for a required field whose only rule is nonempty input; `shouldRevealErrors` comes from the chosen interaction policy:

```swift
private var isValid: Bool { !text.isEmpty }

private var visibleError: String? {
    shouldRevealErrors && !isValid ? "Required" : nil
}
```

Use the actual validation result for decisions that require validity. An optional empty field follows its own rules. If validation is deferred or asynchronous, do not assume an unevaluated or pending result means valid. This distinction does not require a new public type or callback.

### Example presentation states

Distinguish:

| Condition | Validation result | Presentation |
|-----------|-------------------|--------------|
| Required, empty, before error-reveal trigger | Invalid | Neutral appearance, no error copy |
| Required, empty, after error-reveal trigger | Invalid | Error appearance and message |
| Meets the applicable rules | Valid | Normal appearance or success feedback if useful |

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
