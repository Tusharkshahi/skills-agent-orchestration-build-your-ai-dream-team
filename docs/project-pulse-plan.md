# Project Pulse dashboard implementation plan

## Summary

Build Mona's Project Pulse as a dependency-free static dashboard for contributors. It will present multiple projects in a responsive card layout and make each project's owner, status, recent activity, priority, and summary easy to scan.

The implementation will use HTML, CSS, JSON, and browser JavaScript only. No package installation or frontend framework is required.

## Goals and acceptance criteria

- Display the exact title **Project Pulse**.
- Use `app/index.html` as the dashboard entry point.
- Reference `styles.css` and `project-data.json` from the page.
- Load project records from `app/project-data.json`.
- Render multiple visible project cards using the `project-card` class.
- Show each project's `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Use a top-level `projects` array in the JSON data file.
- Provide a polished responsive layout with `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
- Use semantic and accessible markup, readable contrast, visible focus states, and status/priority treatments that do not rely on color alone.
- Provide `.vscode/launch.json` with a configuration named **Run Project Pulse Dashboard**.
- Serve from the `app/` directory with `python3 -m http.server 5500`.
- Open `http://localhost:%s/index.html`, not the server directory listing.

## File assignments

| File or area | Owner | Assignment |
|---|---|---|
| `docs/project-pulse-plan.md` | Planner, coordinated by Orchestrator | Record the plan, ownership boundaries, dependencies, ordering, edge cases, and validation expectations. |
| `app/index.html` | Coder | Create the semantic page structure, link the stylesheet and JSON data, and render project cards from fetched data. |
| `app/styles.css` | Designer | Define the visual system, responsive layout, dashboard hierarchy, card styling, badges, focus states, and accessibility-oriented presentation. |
| `app/project-data.json` | Coder, informed by Designer | Create representative project records under the required top-level `projects` array. |
| `.vscode/launch.json` | Coder | Create strict JSON for the **Run Project Pulse Dashboard** launch configuration, including the app working directory, Python server command, and `index.html` browser target. |
| Integration review | Orchestrator | Confirm that HTML hooks, CSS selectors, data fields, and launch configuration work together without unrelated changes. |

## Designer responsibilities

The Designer will:

- Establish the information hierarchy for the page header, dashboard summary, project grid, and project cards.
- Implement `app/styles.css`.
- Define visual treatments for status, priority, owner information, recent activity, and summaries.
- Include:
  - A `.dashboard` layout selector.
  - A `.project-card` selector.
  - Responsive grid or flex behavior.
  - Clear typography and spacing.
  - Rounded cards using `border-radius`.
  - Elevation and separation using `box-shadow`.
  - Accessible status and priority badges.
  - Keyboard `:focus-visible` states for interactive elements.
  - Usable behavior on narrow screens.
  - Optional reduced-motion handling if transitions are added.
- Keep styling independent of external frameworks and assets.
- Communicate the expected HTML hooks and badge states before HTML integration.

## Coder responsibilities

The Coder will:

- Implement `app/project-data.json` with multiple representative projects.
- Implement `app/index.html` using semantic elements such as a page header, main content region, section heading, and project-card articles.
- Fetch `project-data.json` and render the project records into the dashboard.
- Use safe DOM APIs such as `textContent` for data values.
- Ensure rendered markup visibly includes status, recent activity, and priority.
- Provide loading, empty, and explicit error states.
- Validate that the fetched payload is an object with an array of project records and required fields.
- Create `.vscode/launch.json` as strict JSON with no comments.
- Use `${workspaceFolder}/app` as the launch working directory and open `index.html` explicitly.
- Validate the implementation before handing it to the Orchestrator.
- Avoid modifying Designer-owned styling unless an integration fix is explicitly assigned.

## Data contract

`app/project-data.json` should use this shape:

```json
{
  "projects": [
    {
      "name": "Project name",
      "owner": "Contributor or team",
      "status": "Active",
      "recentActivity": "Short description of the latest update",
      "priority": "High"
    }
  ]
}
```

Include at least three records. Each required field must contain a non-empty string. Use a small, consistent vocabulary for status and priority values so CSS classes and visual labels remain predictable.

## Implementation phases

### Phase 1: Confirm scope and contracts

The Orchestrator reviews the brief, agent definitions, existing VS Code files, and validation workflows. Confirm:

- Required file paths.
- JSON field names and object shape.
- Required CSS hooks.
- Launch command, working directory, and URL.
- Ownership boundaries between Designer and Coder.

This phase must finish before implementation begins.

### Phase 2: Parallel design and preparation

The following work can run in parallel because the file scopes do not overlap:

1. **Designer:** Implement `app/styles.css`, including the agreed layout and HTML hooks.
2. **Coder:** Implement `app/project-data.json`.
3. **Coder:** Create `.vscode/launch.json` with the required launch settings.

The launch file does not depend on final visual design, and the data file can be prepared independently of CSS.

### Phase 3: Sequential HTML integration

After the styling hooks and data schema are fixed:

1. Coder implements `app/index.html`.
2. Coder connects the page to `styles.css` and `project-data.json`.
3. Coder renders data-driven project cards and loading, empty, and error states.
4. Coder confirms that `.dashboard` and `.project-card` are used consistently.

This phase is sequential because the HTML depends on both the data and styling contracts.

### Phase 4: Integrated review and browser validation

The Orchestrator reviews all implementation files together and verifies:

- HTML selectors match CSS selectors.
- Data fields match the rendering code.
- The launch working directory matches the server target.
- The launch URL opens `index.html`.
- The page displays cards when served over HTTP.
- No unnecessary dependencies or unrelated changes were introduced.

Any integration correction is made by the agent owning the affected file, followed by another integrated review.

## Dependencies

- `app/index.html` depends on the field names and object shape in `app/project-data.json`.
- `app/index.html` depends on the CSS hooks and layout contract established for `app/styles.css`.
- `app/styles.css` depends on stable class names such as `.dashboard` and `.project-card`.
- `.vscode/launch.json` depends on `app/index.html` existing at the configured server root, although it can be authored in parallel with the app files.
- Browser data loading depends on serving the `app/` directory over HTTP; opening `index.html` directly with a `file://` URL may block `fetch()`.
- No npm package, framework, build step, or external runtime dependency is required.
- Python 3 is the prescribed server command and should be available in the Codespace.

## Parallel and sequential work decisions

### Work that can run in parallel

- Designer implementation of `app/styles.css`.
- Coder preparation of `app/project-data.json`.
- Coder preparation of `.vscode/launch.json`.
- Documentation of validation commands based on the repository workflows.

### Work that must run sequentially

- Repository research before finalizing the plan.
- Data schema and design-hook agreement before final HTML integration.
- HTML rendering implementation after the JSON contract is fixed.
- Integrated browser validation after all four implementation files exist.
- Any visual or integration fixes followed by revalidation.

Do not assign the same file to Designer and Coder in the same phase.

## Edge cases and risks

- **Missing or malformed JSON:** Show an explicit error message rather than leaving a blank page.
- **Empty `projects` array:** Show a clear empty-state message.
- **Missing fields:** Report invalid records and use readable fallback text where appropriate.
- **Unknown status or priority:** Render the value safely with a neutral fallback style.
- **Long content:** Ensure names and activity text wrap without overflowing cards.
- **Small screens:** Test narrow widths and avoid horizontal scrolling.
- **Accessibility:** Use semantic headings, labeled metadata, sufficient contrast, keyboard-visible focus, and text labels in addition to badge colors.
- **Direct file opening:** Serve the dashboard through the launch configuration or an HTTP server because the data is fetched at runtime.
- **Directory listing regression:** Confirm that the launch URL ends in `/index.html`.
- **Port conflicts:** Report a conflict on port `5500` explicitly rather than silently changing the prescribed configuration.

## Validation expectations

### Automated and structural validation

Run the repository's existing checks after implementation:

```bash
python3 -m json.tool app/project-data.json
python3 -m json.tool .vscode/launch.json
bash scripts/validate-exercise.sh
```

Confirm:

- All four required files exist.
- `app/index.html` contains `Project Pulse`, `styles.css`, `project-data.json`, and `project-card`.
- The HTML visibly references `status`, `recentActivity`, and `priority`.
- `app/styles.css` contains `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
- The JSON contains `projects`, `name`, `owner`, `status`, `recentActivity`, and `priority`.
- `.vscode/launch.json` parses as JSON and contains `Run Project Pulse Dashboard` and `index.html`.

### Runtime validation

Use the VS Code Run and Debug panel to start **Run Project Pulse Dashboard**. Confirm:

- The server starts with `app/` as its working directory.
- The browser opens `http://localhost:5500/index.html`.
- The browser displays the dashboard rather than a directory listing.
- Multiple project cards are visible.
- Every card shows the project name, owner, status, recent activity, and priority.
- No browser console errors occur during data loading.
- Loading, empty, and error states behave as designed.
- The layout remains readable on mobile, tablet, and desktop widths.
- Keyboard navigation and focus states are visible.

Stop the preview server after runtime validation.

## Assumptions

- Inline browser JavaScript in `app/index.html` is acceptable because no separate JavaScript file is assigned.
- Project data uses simple string values for the required fields.
- The Designer chooses the color palette and exact status vocabulary while preserving contrast and semantic clarity.
- The launch configuration uses VS Code terminal launch support and Python's built-in HTTP server.
