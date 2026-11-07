# Layout Resurrection — Product Context

## What we are building
**Layout Resurrection** is an Android Studio plugin that revives the **WYSIWYG (What You See Is What You Get) drag-and-drop UI design workflow** for modern **Jetpack Compose** development.

Jetpack Compose is modern and actively used; **we are not resurrecting Compose**. We are repurposing Android Studio's existing visual **XML Layout Editor** as a designer that produces usable Compose code.

## User workflow
1. Open a normal XML layout in Android Studio's built-in **Design** editor.
2. Drag and drop Views and edit them using the existing Palette, Component Tree, and Attributes panel.
3. The plugin reads changes to the underlying XML and generates/updates a **separate Kotlin `@Composable` file**.
4. Use the generated composable in a normal Android Compose app.

**Source of truth:** XML. **MVP synchronization:** one-way, XML → Compose. No Compose → XML editing in the MVP.

## Target users and value
- Android developers who prefer visual design or want to prototype Compose UIs rapidly.
- Android teams reusing familiar XML tools while adopting Compose.
- Clear benefit: visual layout creation without maintaining a separate design app or manually rewriting layouts into Compose.

## Competition context
- **Kiroween 2026**, planned category: **Frankenstein**. The name "Layout Resurrection" describes the restored WYSIWYG workflow; the category describes combining old XML visual-design tooling and new declarative Compose.
- Aim for a working demo by **December 4, 2026 (organizer deadline; verify timezone on submission page)**.
- Demo story: drag/drop in Android Studio → generated Compose code changes → a Compose screen runs.
- Keep `.kiro/` in the public repository and document how Kiro was actually used during development.
- Prepare an installable plugin ZIP, GitHub repository with an open-source license, and short demo video. JetBrains Marketplace publication is a **goal, not a required dependency** for the contest.

## MVP priority
Reliability and a convincing live demo matter more than supporting every XML widget. First support `LinearLayout` (horizontal/vertical), `TextView`, and `Button`; add other widgets only after the end-to-end workflow works.

## Non-goals for MVP
- Replacing Android Studio's existing designer or building a new canvas.
- Two-way synchronization.
- Runtime LLM/API calls for conversion.
- Pixel-perfect guarantees or arbitrary third-party custom Views.
