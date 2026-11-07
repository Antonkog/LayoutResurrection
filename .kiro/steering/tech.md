# Layout Resurrection — Technical Standards

## Architecture and platform
- Kotlin + Gradle, using the supported **IntelliJ Platform SDK / IntelliJ Platform Gradle Plugin 2.x** appropriate to the locally installed Android Studio version.
- Before pinning dependency versions, **inspect the actual Android Studio product/build number, JDK, Gradle wrapper, and available SDKs**. Consult compatible public documentation where available; do not invent tested versions.
- Separate modules: `converter` (pure Kotlin/JVM) and `plugin` (Android Studio/IntelliJ integration).
- Never depend on undocumented/private Layout Editor APIs or reflection into Android Studio internals. Reuse the UI unchanged, observing/editing its XML document via supported public IDE APIs.

## Conversion pipeline
`XML text → secure XML parser → layout intermediate representation (IR) → deterministic Compose code emitter → generated .kt file`

- The converter must not depend on IntelliJ SDK classes or on Android Studio runtime objects.
- Secure XML parsing: disable DTD / external entity resolution, and do not allow network access from parsed input.
- Start with `LinearLayout`, `TextView`, and `Button`; support `orientation`, basic text, padding, dimensions, and common resource references where practical.
- Implement a clear **unsupported widget/attribute diagnostic**; do not silently claim fidelity when unsupported properties affect output.
- Escape strings and generate stable, formatted, readable Kotlin and necessary imports. Prefer explicit Compose state/callback parameters rather than inventing view behavior.
- Generated code should be suitable for Compose with Material 3 dependencies configured in the consuming app; document expected host dependencies.

## IDE integration and safety
- First implement an explicit **Generate Compose** action; then add automatic regeneration after XML changes.
- Test that visual Layout Editor changes reach a document / file-change listener in the target Android Studio build; do not assume this without verification.
- Auto-generation must debounce bursty XML edits, work off the UI thread where appropriate, and write files safely under IDE write constraints.
- Temporarily invalid XML must **not destroy the last valid generated Kotlin**.
- Generated files live under Kotlin source directories, **not** `res/`.
- Do not overwrite hand-edited output silently. Use a clear generated-file policy (header, ownership check, warning or protected-region strategy).
- No runtime AI dependency or required cloud API keys; **Kiro is used to develop the plugin**, not as a runtime dependency.

## Quality gates
- Unit tests for parser and emitter: nesting, orientation, escaping, dimensions, resource references, unsupported tags, malformed XML, deterministic output.
- Gradle build + converter unit tests must pass before adding live-sync.
- Smoke-test plugin loading and action visibility in a compatible Android Studio sandbox where available.
- Clearly report what has been verified and what remains untested; do not claim working code, visual parity, or marketplace compatibility without running checks.
