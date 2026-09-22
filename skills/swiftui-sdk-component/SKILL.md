---
name: swiftui-sdk-component
description: >-
  Authors and reviews reusable SwiftUI SDK components with clear state ownership,
  focused customization, composition, stable public APIs, and sensible defaults.
  Use for CommonSwiftUI, SwiftUI libraries, and package UI modules—not app feature
  screens or general concurrency migrations.
---

# SwiftUI SDK Component

Use for reusable SwiftUI controls in a package or design system. For app features, use the host's architecture guidance. For substantial concurrency work, consult dedicated Swift concurrency guidance when available.

## Six foundations

Review all six principles; choose the smallest implementation that meets the component's needs. Preserve compatible existing public APIs.

| # | Foundation | Rule |
|---|------------|------|
| 1 | Separate behavior from appearance | Keep behavior independent of app branding; prefer standard styling, then grouped Config where useful |
| 2 | Deliberate ownership | Values for read-only inputs, Binding for parent-owned edits, callbacks for meaningful events; one source of truth |
| 3 | Focused initialization and customization | Required inputs explicit; modifiers for useful options; environment only for inherited settings |
| 4 | Composition | Prefer suitable system controls; extract distinct responsibilities, not arbitrary line counts |
| 5 | Stable public API | Minimal public surface, clear names, documented contract and usage |
| 6 | Sensible defaults | Required inputs suffice for basic use without styling setup |

Enforce principles and prefer established patterns when their conditions hold. Config, environment keys, and private subviews are not mandatory artifacts. Briefly justify meaningful departures. Read [six-foundations.md](references/six-foundations.md) for detail.

## Data flow and callbacks

- Use a value for read-only input, Binding for two-way editing, and private state for component-owned transient state.
- Add optional callbacks only for concrete consumer needs that existing inputs, bindings, or derived results cannot satisfy.
- Reject redundant state notifications, callbacks for every internal change, and collections of workflow closures.
- When callbacks multiply, review responsibilities before adding event enums or callback containers.
- Do not mirror a Binding into local state or use constant bindings as editable fallbacks.
- Presentation choices must not distort underlying state. Apply validation-specific guidance only when the component validates input.

See [state-ownership.md](references/state-ownership.md).

## Customization

- Prefer standard SwiftUI styling first; use nested Config for coherent groups of component-specific appearance settings.
- Prefer focused modifiers for optional customization. For a setting affecting only one component instance, prefer a scoped API rather than a global View extension.
- Use EnvironmentKey for intentional descendant inheritance. Name global modifiers and keys for their component family, document defaults and overrides, and ensure the component consumes the value.
- Do not shadow SwiftUI modifiers or propagate legacy broad names into new APIs. Preserve existing public names unless migration is authorized.

See [api-design.md](references/api-design.md).

## Reuse and evolution

Choose View for a coherent control/layout, a standard style protocol for styling an existing control, and ViewModifier for an effect on existing content. Prefer existing Shape/ShapeStyle protocols and focused builder slots when real consumers need those variations. Keep implementation replaceable behind a small public API.

Review one realistic variation: can it be supported without duplicating the component or adding unrelated flags and callbacks? Add flexibility for demonstrated needs, not hypothetical futures. See [composition.md](references/composition.md) for lessons from CommonSwiftUI's button styles, alert content, and shimmer effects.

## Work within the user's request

1. Inspect host conventions, package/target settings, existing APIs, and consumer usage.
2. Outline material API and ownership decisions, scaled to the change.
3. Implement the authorized scope. An implementation request does not require another approval checkpoint.
4. Verify and report the result.

Pause only for proposal-only requests or a material unresolved decision that needs the user. Do not silently broaden scope into breaking changes, dependencies, migrations, or releases. Follow [scaffold-workflow.md](references/scaffold-workflow.md) for details.

## Compatibility and verification

- Preserve supported platforms, deployment targets, Swift tools/language settings, and existing dependencies.
- Use availability checks or compatible alternatives as appropriate. Runtime checks do not solve unsupported compiler features.
- Reuse custom helpers only when they exist in the host.
- Build affected targets and compile consumer examples for public API changes.
- Run relevant tests and inspect changed rendering/interactions where available; report unperformed checks explicitly.
- Use existing sample/preview infrastructure rather than requiring a new sample app or fixed folder path.

See [platform-versioning.md](references/platform-versioning.md).

## Documentation and release discipline

Document public contracts and one complete usage example. Keep implementation comments for non-obvious reasons, invariants, and workarounds; do not narrate obvious code or mechanically add banners and section markers.

Treat source examples as references, not universal rules. Preserve legacy public names during unrelated work. Do not bump versions, publish, tag, or rebuild distribution artifacts unless requested.

See [documentation.md](references/documentation.md), [golden-examples.md](references/golden-examples.md), and [shipping.md](references/shipping.md).

## Ship checklist

- [ ] All six foundations hold; preferred patterns applied where appropriate.
- [ ] Ownership and required inputs are clear; callbacks serve concrete needs.
- [ ] Customization has an observable effect; modifier names, scope, and overrides are explicit.
- [ ] No unused state, duplicate truth, app-specific dependencies, or obvious descriptive comments.
- [ ] Supported platform/toolchain constraints and compatible APIs are preserved.
- [ ] Public usage example is complete; host sample covers relevant behavior.
- [ ] Appropriate builds, consumer checks, and behavioral/visual checks completed or gaps reported.
- [ ] Release actions remain within the requested scope.

## Resources

- [Reference index](references/_index.md)
- [Composition](references/composition.md)
- [Folder structure](references/folder-structure.md)
- [UX edge cases](references/ux-edge-cases.md)
- [Anti-patterns](references/anti-patterns.md)
- [Bundled template instructions](templates/README.md)

Templates are inside this skill at `templates/swift/`. Resolve paths relative to this SKILL.md; installation does not require the original repository.
