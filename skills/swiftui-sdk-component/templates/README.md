# Swift scaffolds

These files are bundled inside the installed skill. Resolve paths relative to the skill directory, not the user's working directory.

The three Swift templates form one example: an editable text field with inherited character-count visibility. They demonstrate standard styling, an environment setting that actually affects rendering, and a local override. They are starting points, not required architecture for every component.

Replace `{ComponentName}` with the PascalCase type name, `{componentName}` with its lowerCamelCase equivalent, and `{ModuleName}` with the library's importable module name. Put Component and EnvironmentKey in the library target and SampleTestView in a consumer target. Adapt user-facing text and localization to the host.

For a plain field without inherited count settings, omit the environment file, the environment property and count branch in the component, and the count examples in the sample. Do not leave no-op configuration or unused state behind. Add Config only when actual grouped customization needs it.

The examples use PreviewProvider and avoid navigation dependencies. Check the host platform and toolchain before generating source, then compile the library and separate consumer example. Inspect rendering and interaction separately; a successful compile is not visual verification.
