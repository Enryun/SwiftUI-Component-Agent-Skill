# Shipping

Foundation **#5** — stable, discoverable public API. See [six-foundations.md](six-foundations.md).

## Visibility

| Access | Use for |
|--------|---------|
| `public` | `struct` view, `Config`, public modifiers, types returned from public API |
| `internal` | helpers, subviews, window managers, view models used only inside module |
| `private` | implementation details inside a file |

Do not expose UIKit types in `public` method signatures unless the API is explicitly a UIKit bridge.

## Dependencies

- Prefer SwiftUI + Foundation only.
- Add third-party packages only with strong justification; document in README.
- Camera, CoreImage, AVFoundation → isolate in `Internal/`; list `Info.plist` keys in README (e.g. `NSCameraUsageDescription`).

## SPM module

- One module per package product (e.g. `CommonSwiftUI`).
- New files under `Sources/{Module}/` are picked up automatically.
- Bump `CommonSwiftUI.version` (or package version) when releasing breaking API changes.

## Binary distribution (optional)

If the package ships an xcframework:

- Keep **source** and **binary** module names identical.
- Release process must rebuild xcframework when public API changes.
- Skill applies to **source** authoring; binary is a distribution concern.

## What not to ship

- App-specific asset names (`bank_logo`)
- Hard-coded copy that should be localized by the host app (prefer `String` parameters)
- Debug `print` in `body` or `onChange`
