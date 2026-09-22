# Folder structure

Adapt to the host package. CommonSwiftUI-style layout:

These trees illustrate a component with grouped configuration and inherited settings. Create only the files needed by the selected API; a simple component may need just its view file.

```
Sources/{ModuleName}/
└── Components/
    └── {ComponentName}/
        ├── Public/
        │   └── {ComponentName}.swift          # public struct + Config types
        ├── Internal/
        │   └── {ComponentName}Helper.swift    # UIKit, window, layout math
        └── {ComponentName}+EnvironmentKey.swift
```

If the package does **not** use Public/Internal folders, use a flat folder:

```
Components/{ComponentName}/
├── {ComponentName}.swift
├── {ComponentName}+EnvironmentKey.swift
└── {ComponentName}ViewModel.swift   # only if unavoidable; prefer keeping logic in the view
```

## Sample target

```
SampleCode/SampleCode/{Category}/{ComponentName}TestView.swift
```

Register navigation from the sample app’s menu / `ContentView` when the project already does so.

## Naming

| Item | Pattern |
|------|---------|
| View | `PascalCase` noun: `ValidationTextField` |
| Config | `{Name}.Config`, `{Name}.BorderConfig` |
| Environment file | `{Name}+EnvironmentKey.swift` |
| Sample | `{Name}TestView` or `{Name}Test` |

Avoid app-specific prefixes (`MyBankTextField`).
