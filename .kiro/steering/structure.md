# Layout Resurrection — Project Structure and Conventions

## Target repository layout
```text
layout-resurrection/
├── .kiro/
│   ├── steering/
│   │   ├── product.md
│   │   ├── tech.md
│   │   └── structure.md
│   └── specs/
│       └── layout-resurrection-mvp/
│           ├── requirements.md
│           ├── design.md
│           └── tasks.md
├── converter/
│   └── src/
│       ├── main/kotlin/.../converter/
│       └── test/kotlin/.../converter/
├── plugin/
│   └── src/main/
│       ├── kotlin/.../plugin/
│       └── resources/META-INF/plugin.xml
├── sample-app/                # optional after plugin MVP is working
├── docs/
├── settings.gradle.kts
├── build.gradle.kts
├── gradle.properties
└── README.md
```

## Responsibilities
- `converter/`: XML parser, layout IR, Compose emitter, diagnostics, unit and golden-file tests. **Must remain independent of IDE SDK**.
- `plugin/`: IDE actions, XML/document observation, output-path resolution, safe write operations, user notifications and settings.
- `sample-app/`: minimal host Android app for validating generated Compose code, once the baseline plugin is operational.
- `.kiro/specs/`: feature requirements, design decisions, and an ordered task list. Keep actual implementation status accurate.
- `docs/`: architecture, supported conversion matrix, known limitations, compatibility/testing notes, demo script.

## Implementation order
1. Inspect local tools and Android Studio compatibility; set up Gradle multi-module project.
2. Create a loadable plugin with `Layout Resurrection: Hello` action and a minimal converter test. **Stop for review.**
3. Implement standalone XML → IR → Compose converter with tests.
4. Add IDE **Generate Compose** action and safe file writes.
5. Add verified XML change detection, debounced auto-sync and diagnostics.
6. Demonstrate drag/drop → generated Kotlin → running Compose screen; package plugin ZIP.

## Coding and collaboration conventions
- Use descriptive Kotlin names and small, testable components; do not mix IDE APIs into the converter.
- Keep generated code deterministic and side-effect-free where possible.
- Make changes in small reviewed increments; avoid broad scope creep during the hackathon.
- Before any significant implementation, check whether a corresponding task exists in `.kiro/specs/` and update its completion status only after verification.
- Do not ignore or remove `.kiro/` from version control; contest documentation must reflect **actual Kiro usage**.
