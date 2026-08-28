# Project Pulse Dashboard Implementation Plan

## Objective

Build a static Project Pulse dashboard that presents project information in a polished, accessible, responsive card-based interface. The dashboard must be driven by JSON data, run without a package installation, and open directly to `index.html` when launched from the workspace.

## Workstream ownership

Ownership is intentionally non-overlapping. Each contributor may provide implementation feedback across the workstreams, but edits remain with the assigned owner unless the Orchestrator explicitly reassigns them.

| Owner | Assigned files | Responsibilities |
| --- | --- | --- |
| Planner | Planning and coordination input | Research the requested behavior, identify file boundaries and dependencies, define the data contract, and provide the implementation sequence and acceptance criteria. |
| Designer | `app/styles.css` | Create the visual system and responsive presentation: polished card-based layout, `.dashboard` and `.project-card` hooks, status and priority treatments, spacing, contrast, typography, keyboard and screen-reader accessibility support, and behavior across narrow and wide viewports. |
| Coder | `app/index.html`, `app/project-data.json`, `.vscode/launch.json` | Build semantic dashboard markup, load and render project data from JSON, define complete project records, and provide the strict JSON launch configuration. |
| Orchestrator | Coordination and integration review; no additional implementation ownership | Coordinate Planner, Designer, and Coder; enforce the file boundaries; resolve the CSS hook and data-schema contract; integrate the work; and perform the final review and validation. |

## Contracts and implementation details

### Data contract

`app/project-data.json` is the source of truth for the cards displayed by the dashboard. The Coder must define a valid JSON document containing a collection of projects. Every project record must include:

- `name`
- `status`
- `recentActivity`
- `priority`

The schema should be simple and stable enough for `app/index.html` to render without hard-coded project-specific markup. Values should be suitable for visible display and for status/priority styling hooks. If the implementation uses identifiers or additional fields, they must be documented by the structure itself and remain valid JSON.

### Markup and rendering

The Coder owns `app/index.html` and must:

- Use semantic document structure with a clear page heading and dashboard region.
- Reference `styles.css` from the document.
- Reference `project-data.json` from the rendering logic.
- Render project cards from the loaded data rather than duplicating project content in the HTML.
- Expose the agreed `.dashboard` and `.project-card` hooks, along with predictable hooks or attributes for status and priority treatments.
- Ensure each visible `.project-card` shows the project's `status`, `recentActivity`, and `priority`.
- Surface data-loading or rendering errors clearly rather than silently showing an empty success state.

### Visual and accessibility system

The Designer owns `app/styles.css` and must:

- Establish a polished card-based layout using `.dashboard` as the layout hook and `.project-card` as the card hook.
- Define clear visual treatments for project status and priority, while ensuring meaning is not conveyed by color alone.
- Provide intentional spacing, readable typography, sufficient contrast, visible focus states, and sensible hover behavior.
- Support keyboard navigation and assistive-technology-friendly presentation through styles that preserve readable structure and focus visibility.
- Adapt the dashboard and cards for responsive behavior across narrow, medium, and wide viewports without clipping or requiring horizontal scrolling.
- Keep selectors aligned with the markup contract so the Coder can integrate without renaming or duplicating styles.

### Launch configuration

The Coder owns `.vscode/launch.json`. It must be strict JSON and contain a deterministic configuration named `Run Project Pulse Dashboard` that:

- Uses the existing environment's Python 3 server command: `python3 -m http.server 5500`.
- Runs with `cwd` set to `${workspaceFolder}/app`.
- Explicitly opens `index.html`, so the dashboard appears instead of a directory listing.
- Targets the local server at the corresponding port and page.

Because the file is strict JSON, it must not contain comments or trailing commas.

## Dependencies and sequencing

The app has no package dependency and does not require a package install or build step. The only runtime dependency is the existing Python 3 environment used to serve the static files.

1. **Planner research and contract definition**
   - Confirm the requested dashboard behavior, file ownership, required project fields, CSS hooks, launch behavior, and validation criteria.
   - Record the data and markup contracts that Designer and Coder will use.

2. **Designer and Coder preparation**
   - Designer establishes the styling contract in `app/styles.css`, including `.dashboard`, `.project-card`, status, priority, responsive, and accessibility treatments.
   - Coder defines the initial records in `app/project-data.json` and prepares the strict VS Code launch configuration in `.vscode/launch.json`.
   - These styling and initial-data tasks may proceed in parallel because they are independent file changes. The launch configuration is likewise isolated to the Coder's ownership.

3. **Schema and hook alignment**
   - Before HTML integration, the Designer and Coder agree on the class names, status/priority hooks, and JSON field names.
   - The Coder's HTML work must wait for this agreement so the renderer uses the final CSS hooks and data schema rather than introducing parallel conventions.

4. **Coder integration**
   - Implement semantic markup and JSON-driven rendering in `app/index.html`.
   - Connect the agreed data fields to visible card content and styling hooks.
   - Keep the page usable if loading fails by providing an explicit error state.

5. **Orchestrator review and integration**
   - Confirm that each owner changed only assigned files and that the three contracts work together.
   - Review the dashboard visually and behaviorally, then run the validation checks below.
   - Resolve integration defects through the owning contributor, preserving the non-overlapping ownership model.

## Validation expectations

The Orchestrator must validate the completed implementation at both file and browser levels:

- **File existence:** Confirm `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
- **JSON validity:** Parse `app/project-data.json` and `.vscode/launch.json` as strict JSON. Confirm every project contains `name`, `status`, `recentActivity`, and `priority`.
- **HTML wiring:** Confirm `app/index.html` references `styles.css` and `project-data.json`.
- **Rendered content:** In a browser smoke test, confirm the loaded dashboard produces visible `.project-card` elements and that every visible card shows `status`, `recentActivity`, and `priority`.
- **CSS contract:** Confirm `app/styles.css` defines the `.dashboard` and `.project-card` selectors and includes the expected card layout, status/priority treatments, spacing, contrast, typography, focus/accessibility, and responsive behavior.
- **Launch contract:** Confirm `.vscode/launch.json` contains the configuration name `Run Project Pulse Dashboard`, uses `python3 -m http.server 5500`, targets `index.html`, and sets `cwd` to `${workspaceFolder}/app`.
- **Browser launch behavior:** Start the static server with `python3 -m http.server 5500` from the `app` directory and open the configured `index.html` URL. Confirm the dashboard opens directly rather than showing a directory listing.

The implementation is complete only when the static server and launch configuration expose the rendered dashboard, the data contract is satisfied, and the visual and accessibility requirements remain intact at responsive sizes.
