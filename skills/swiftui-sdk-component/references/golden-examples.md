# Golden examples

Patterns below are illustrated in [CommonSwiftUI](https://github.com/Enryun/Common_SwiftUI). When working in that repo, **read these files** instead of re-deriving conventions.

| Pattern | Reference |
|---------|-----------|
| Config + Binding + validation deferral | `ValidationTextField.swift` |
| EnvironmentKey modifiers | `ValidationTextField+EnvironmentKey.swift` |
| Alert configuration object | `UniversalAlertConfig.swift` |
| Internal view + public entry | `Toast` / `ToastView` (internal) |
| iOS 16 vs 15 styling branch | `ToastView.swift` (`apply` + `#available`) |
| Shimmer / modifier-based API | `ShimmerView`, shimmer modifiers |
| Sample app usage | `SampleCode/SampleCode/TextField/`, `Alert/`, etc. |

## What to copy

- Doc comment with full usage example (`ValidationTextField`)
- Text Binding for parent-owned editable input (same file)
- `FormValidationElement` for multi-rule checklist UI (same file)
- `private(set)` on config nested types (same file)

## What not to copy blindly

- Optional output bindings with constant fallbacks or mirrored `@State` synchronization — follow [state-ownership.md](state-ownership.md); preserve existing APIs until migration is agreed

- Typos in legacy names (`HambugMenu`) — fix when touching, do not propagate
- Inconsistent naming across old components — follow this skill’s naming table instead

## Other packages

When authoring outside CommonSwiftUI, apply the same **principles** and adapt folder paths to the host package layout. Choose Config and EnvironmentKey only where the component needs them; the examples do not make those mechanisms mandatory.
