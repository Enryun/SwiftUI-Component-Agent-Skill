# Component checks

Use with [SKILL.md](skills/swiftui-sdk-component/SKILL.md). Review principles rather than requiring identical types and files for every component.

| Foundation | Review |
|------------|--------|
| Behavior and appearance | Standard styling first; grouped Config when useful; no app-brand dependency |
| Ownership | Values for inputs, Binding for edits, focused callbacks for events; one source of truth |
| Focused customization | Required inputs explicit; modifier names and scope clear; environment only for inheritance |
| Composition | Suitable system controls; subviews around distinct responsibilities |
| Public API | Minimal surface, compatible names/signatures, complete consumer example |
| Defaults | Basic usage works without styling setup |

Apply preferred patterns when their conditions hold and briefly justify meaningful alternatives. Parameter counts and body length are review signals, not hard limits.

## Implementation review

- [ ] Every callback serves a concrete consumer need; no redundant state notifications or workflow closure collections.
- [ ] No mirrored bindings or constant editable fallbacks.
- [ ] Every customization option has an observable effect.
- [ ] The public form fits the responsibility; a realistic appearance/content variation is supported without duplication or unrelated flags and callbacks, where that variation belongs in the component.
- [ ] Inherited modifiers have scoped names, documented defaults, and working local overrides.
- [ ] Relevant interaction states behave correctly; presentation preserves underlying state.
- [ ] Platform, deployment, and Swift settings are preserved; availability decisions are localized.
- [ ] Public docs describe contracts; implementation comments explain non-obvious reasons rather than narrating code.
- [ ] Samples follow host conventions and demonstrate actual supported behavior.
- [ ] Appropriate target builds, separate consumer compilation, and relevant behavioral/visual checks ran; gaps are reported.
- [ ] No unsolicited breaking changes, version bumps, tags, or publishing.

## Workflow

Inspect and design, implement the authorized scope, then verify. Stop for proposal-only requests or material unresolved decisions; existing authorization does not need repeating.

## References

- [Foundations](skills/swiftui-sdk-component/references/six-foundations.md)
- [Workflow](skills/swiftui-sdk-component/references/scaffold-workflow.md)
- [Data flow and callbacks](skills/swiftui-sdk-component/references/state-ownership.md)
- [API and modifier scope](skills/swiftui-sdk-component/references/api-design.md)
- [Compatibility and verification](skills/swiftui-sdk-component/references/platform-versioning.md)
- [Documentation and comments](skills/swiftui-sdk-component/references/documentation.md)
- [Shipping](skills/swiftui-sdk-component/references/shipping.md)
- [Templates bundled with the skill](skills/swiftui-sdk-component/templates/README.md)
