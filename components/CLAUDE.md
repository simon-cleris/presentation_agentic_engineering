# Components

Reusable Vue single-file components used by the slides.

## Conventions

- Props are declared explicitly and every component has a single visual responsibility.
- Colors and spacing come from the global stylesheet variables, never hard-coded, except for data-encoding colors inside a chart-like component.
- Styles are scoped to the component.

## Families

- Cards: `Card`, `CardOutline` and `StatCard` wrap markdown content as accented, outlined or figure-led blocks.
- Charts and diagrams: `EnergyChart`, `CitationCloud`, `InstrumentToAircraft`, `FeedbackLoop`, `VerificationSpectrum` and `Timeline` draw one figure each.
- `Timeline` lays out dated events along a horizontal line and reveals them one click at a time, starting at a configurable click index.
- `IncrementalContext` animates the incremental loading of CLAUDE.md files: a project tree on the left, the agent context on the right, driven by the slide clicks, one card click followed by one animation click (global file, folder file on access describing only its own scripts plus a one-line entry per sub-folder, generated files, context reset and rebuild for the next task).
- Slide structure: `ApproachRules` and `SlideSummary` render the rules list and the table of contents.
- Media: `AutoPlayVideo` plays a looping video from the public assets.
