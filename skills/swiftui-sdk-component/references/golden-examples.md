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
- Optional `Binding<Bool>?` with default (same file)
- `FormValidationElement` for multi-rule checklist UI (same file)
- `private(set)` on config nested types (same file)

## What not to copy blindly

- Typos in legacy names (`HambugMenu`) — fix when touching, do not propagate
- Inconsistent naming across old components — follow this skill’s naming table instead

## Other packages

When authoring outside CommonSwiftUI, apply the same **shapes** (Config, EnvironmentKey, sample view) and adapt folder paths to the host package layout.
