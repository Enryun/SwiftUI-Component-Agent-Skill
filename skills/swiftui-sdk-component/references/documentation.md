# Documentation

Every **public** component needs a doc comment on the main type.

## Structure

1. One-line summary
2. What it does (2–3 sentences)
3. Parameters / key modifiers
4. **One compilable usage example** in a ` ```swift ` block

## Example skeleton

```swift
/// A text field with validation feedback and optional secure entry.
///
/// Supports `.isMandatory`, `.onValidate`, and `.onFormValidate` modifiers.
///
/// ## Usage Example:
/// ```swift
/// struct ContentView: View {
///     @State private var email = ""
///     @State private var isEmailValid = false
///
///     var body: some View {
///         ValidationTextField(title: "Email", text: $email, isValid: $isEmailValid)
///             .isMandatory(true)
///             .onValidate { $0.contains("@") ? .success("") : .failure(MyError.invalid) }
///     }
/// }
/// ```
public struct ValidationTextField: View { … }
```

## Modifiers

Document public `View` extension methods with a short line each; group in the type’s doc comment under `## Modifiers:`.

## README

Package README can stay long for humans. The skill targets **agent-consumable** doc comments — do not duplicate the entire README into `SKILL.md`.
