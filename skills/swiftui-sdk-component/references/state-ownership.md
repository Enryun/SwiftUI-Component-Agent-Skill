# State ownership

Foundation **#2** — own state deliberately. See [six-foundations.md](six-foundations.md).

## Choose by data flow

| State | Owner | Example |
|-------|--------|---------|
| Read-only input | Parent; plain value | Title, options, externally computed validation result |
| Editable text or selection | Parent; `Binding` | `$email`, `$selection` |
| Action or result notification | Focused callback | `onSubmit`, `onValidationChange` |
| `hasInteracted`, animation phase | Component `@State` private | defer validation |
| Clear/secure button visibility defaults | `EnvironmentKey` | `.clearButtonHidden(false)` |
| App theme / feature flags | `Environment` | semantic colors |

## Bindings and optional ownership

Use Binding for two-way edits to parent-owned state. The parent needing to react is not sufficient reason to expose a Binding. Prefer deriving inexpensive results from their inputs rather than storing another writable truth. If the parent computes validation, accept its result as a value; if the component computes it and notification is needed, prefer a focused callback with documented timing. Never emit events from `body`.

Do not default a missing editable binding to `.constant(...)`: writes are discarded. Constant bindings suit deliberately fixed previews or examples, not a working editable mode.

Prefer one clear ownership mode. Support both parent-controlled and internally owned modes only when needed, with one active source of truth. A convenience wrapper may own `@State` and pass its binding to the controlled component. An editing draft is valid when explicit commit/cancel semantics define when it replaces the parent's value.

Do not mirror a binding into `@State` and synchronize with `didSet` or paired change observers. Preserve compatible existing public APIs; propose migration explicitly for legacy output bindings.

## Closures without overuse

**Default:** do not add an optional callback without a concrete consumer need. Prefer a small number of semantic actions or events at the component boundary. A required action such as a button's action is a normal part of its contract.

- For each proposed callback, name the consumer need and why a value, existing Binding, or caller-derived result does not already satisfy it.
- Do not add notifications for every internal change, or duplicate a Binding with an `onValueChanged` callback that only repeats the same value. A distinct commit event may still be useful even when the edited value is bound.
- Use values for read-only data and Binding for genuine two-way editing, rather than getter/setter closures.
- A focused strategy closure, such as validation, is an input; distinguish it from an output notification.
- Keep business workflows outside the component; avoid passing feature ViewModels or collections of service closures.
- Do not thread callbacks through several layers of otherwise uninvolved subviews. Reconsider the composition boundary or use an existing state owner; environment closures or a callback container are not automatic fixes.
- If callbacks multiply, review the component's responsibilities first. A typed event enum is useful only for a coherent event family; do not hide unrelated actions behind a catch-all `onEvent` or an `Actions` bag.
- Document when an event fires and its payload. Avoid feedback loops where a callback writes the state that triggered it; never emit callbacks while evaluating `body`.

```swift
// Redundant: parent already owns and observes the editable text.
SearchField(text: $query, onTextChanged: { query = $0 })

// Meaningful: submission is a distinct user action.
SearchField(text: $query, onSubmit: { runSearch(query) })
```

Treat these as API design checks, not a numeric callback limit. Builder closures for custom content and focused strategy closures serve different purposes and should be reviewed separately.

## Do not

- Put parent business rules inside the component (e.g. “user is premium”) — pass required values and focused actions.
- Use `@ObservedObject` for simple controls unless the component truly wraps a long-lived model.

Apple references: [Binding](https://developer.apple.com/documentation/swiftui/binding), [State](https://developer.apple.com/documentation/swiftui/state).
