# Composition, not inheritance

Foundation **#4**. SwiftUI components are built by **combining** views, not subclassing.

## Choose the public form

Choose the API that matches what callers reuse; a reusable component does not always need a new View type.

| Caller needs | Preferred form | CommonSwiftUI example to inspect |
|--------------|----------------|----------------------------------|
| A control or layout that owns a coherent structure | View with explicit inputs | ValidationTextField |
| A reusable appearance or interaction treatment for an existing control | Standard style protocol such as ButtonStyle | ShapeButtonStyle, LoadingButtonStyle |
| An effect applied to arbitrary existing content | ViewModifier, optionally exposed by a focused View extension | Shimmer modifiers |

Keep the public entry small and move implementation into internal views, modifiers, or helpers when that separation helps. Shimmer's public modifier and internal effect illustrate this boundary: consumers should not need to change their call sites when internal rendering changes. Do not add a wrapper or helper solely to create another layer.

A general-purpose effect can legitimately extend View without using EnvironmentKey. Distinguish applying an effect to the modified content from publishing an inherited setting for descendant controls.

## Design for demonstrated variation

### Reuse existing SwiftUI protocols

ShapeButtonStyle accepts Shape and ShapeStyle types, allowing callers to vary shapes and supply colors or gradients through the same API. When those variations are needed, prefer the existing protocol over a growing list of flags or a Color-only API that cannot express them.

Keep generic parameters tied to meaningful variation. A simple Color input is appropriate when color is the intended contract; do not make every property generic, introduce a custom style protocol, or add type erasure just for hypothetical flexibility. Follow the host's supported toolchain and existing style conventions.

### Separate caller content from reusable behavior

UniversalAlert accepts builder content while its implementation handles presentation. Apply that division to labels, accessories, and other variable regions: callers supply content; the component owns its documented layout and interaction behavior. Prefer a focused content slot over flags such as showIcon/showSubtitle/useCustomHeader when callers actually need varied content.

Builder closures describe visual content; they are not action callbacks and should not carry business workflows. Keep a clear layout contract for each slot and add only slots required by real use cases. See the initializer guidance below.

### Review a realistic change

Ask: **Can a realistic new appearance or content variation be supported without duplicating the component or adding unrelated flags and callbacks?**

Use a variation from an actual consumer requirement, such as a gradient button fill or a custom alert body. Demonstrate it in existing sample usage when relevant. If the variation changes the component's core responsibility, a separate component may be cleaner than extending the original. This review does not require speculative extension points or compatibility with every imaginable variation.

## Implementation practices

### Wrap platform controls

| Need | Compose from |
|------|----------------|
| Text input | `TextField`, `SecureField` |
| Actions | `Button`, `Toggle` |
| Lists in component | `ForEach` + small row subview |
| Material / blur | `background`, `.ultraThinMaterial`, versioned helper |

### Extract private subviews

When `body` has distinct responsibilities or repeated regions, extract them when a name clarifies their purpose. Line counts are review signals, not mandatory extraction thresholds:

```swift
public var body: some View {
    VStack(alignment: .leading) {
        fieldRow
        validationMessages
    }
}

private var fieldRow: some View { … }
private var validationMessages: some View { … }
```

### Keep one responsibility

Keep each public view, style, or modifier focused on one reusable responsibility. Do not bundle navigation, networking, and layout of an entire screen.

## Do not

| Anti-pattern | Why |
|--------------|-----|
| Subclass another `View` struct | SwiftUI views are structs; use wrapping |
| Body mixing unrelated responsibilities | Hard to understand and extend; extract coherent regions |
| Reimplement `TextField` drawing | Accessibility and keyboard behavior regress |
| Hidden singletons inside component | Breaks reuse and preview |
| Screen-level logic (API calls, routes) | Belongs in app layer, not SDK |

## Optional slots

Use `@ViewBuilder` trailing closures when callers need custom visual content, such as a button label, control accessory, or container header/footer. Required content is valid; provide defaults only when they represent useful behavior. Do not add slots merely for hypothetical reuse.

```swift
public init(
    title: String,
    @ViewBuilder accessory: () -> Accessory
) { … }
```

This is a signature sketch for a component generic over `Accessory: View`. If an empty accessory is useful, add a convenience initializer constrained to `Accessory == EmptyView`; do not assume an EmptyView default satisfies every generic Accessory type.

## Checklist

- [ ] Built on `TextField` / `Button` / system control where applicable
- [ ] Distinct responsibilities are extracted where useful
- [ ] Public form fits the use; demonstrated variations use appropriate styles, protocols, or content slots
- [ ] Internal implementation can evolve without exposing helpers or changing unrelated consumer code
- [ ] No screen navigation or use-case calls inside component
- [ ] Previews and sample view compile with minimal setup
