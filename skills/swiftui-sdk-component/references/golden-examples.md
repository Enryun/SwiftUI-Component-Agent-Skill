# Reference examples

[CommonSwiftUI](https://github.com/Enryun/Common_SwiftUI) supplies useful examples, not normative implementations. When available locally, inspect the current source and nearby usage before borrowing a pattern. Locate files by name; paths and APIs may change.

| Pattern to inspect | Reference |
|--------------------|-----------|
| Generic Shape/ShapeStyle customization using a standard ButtonStyle | ShapeButtonStyle.swift, LoadingButtonStyle.swift |
| Caller-built content behind a public modifier and internal presentation | UniversalAlertView+ViewModifier.swift, UniversalAlertView.swift |
| Grouped appearance and editable text Binding | ValidationTextField.swift |
| Inherited settings and their consumers | ValidationTextField+EnvironmentKey.swift |
| Alert configuration and presentation ownership | UniversalAlertConfig.swift |
| Internal implementation behind a public entry point | Toast / ToastView |
| Existing availability helper conventions | ToastView.swift |
| Direct modifier API | ShimmerView and shimmer modifiers |
| Consumer integration | Existing TextField and Alert sample screens |

## Evaluate before copying

For the design lessons behind styles, content slots, and public effect APIs, read [composition.md](composition.md).

- Copy the useful boundary or interaction, not every type and file.
- Distinguish appearance data from presentation state and actions; a legacy Config name does not establish correct ownership.
- Check that modifier names and scope fit the new component. Broad legacy names are not a naming recommendation.
- Do not copy output bindings with constant fallbacks or mirrored state synchronization; use [state-ownership.md](state-ownership.md).
- Do not copy validation deferral that marks untouched required input valid; see [ux-edge-cases.md](ux-edge-cases.md).
- Verify that helpers such as `.apply` exist in the destination package and that sample APIs support its deployment targets.
- Use correct spelling for new APIs, but preserve existing public names, including legacy typos, unless a migration is authorized.
- Compile documentation examples rather than assuming they are complete.

If the source repository is unavailable, use the bundled references and templates. Do not invent claims about its current implementation or block unrelated component work on access to it.
