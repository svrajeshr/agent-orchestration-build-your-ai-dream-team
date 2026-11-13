# Project Pulse dashboard implementation plan

## Summary
Build Mona’s Project Pulse as a small static dashboard using the four files specified in `.github/project-pulse-brief.md`. The `app/` directory currently has no files, and the repository has no existing app framework or build convention to follow. The brief and Step 3 instructions define the expected content, CSS hooks, and preview behavior.

Use the Designer for the visual and accessibility direction and `app/styles.css`; use the Coder for the HTML, JSON data, and VS Code launch configuration. Render project data from JSON in the browser, and use a local HTTP server for preview rather than opening the page directly as a file.

## Ordered implementation steps

### 1. Agree on the interface contract, then work in parallel

**Designer — owns `app/styles.css`**
- Define a clear information hierarchy for the dashboard and responsive project-card layout.
- Style status and priority so they are distinguishable without relying on color alone; include readable contrast, spacing, and visible keyboard focus.
- Provide the CSS hooks for the Coder to use: `.dashboard`, `.project-card`, and suitable status/priority badge hooks.
- Include the required polished card treatment, including `border-radius` and `box-shadow`.
- Do not edit the HTML, data, or launch configuration.

**Coder — owns `app/project-data.json` and `.vscode/launch.json`**
- Create a top-level `projects` array. Every project must include `name`, `owner`, `status`, `recentActivity`, and `priority`. Include a short `summary` per project to meet the brief’s contributor-friendly-summary goal.
- Use clearly labeled example content; the repository does not supply authoritative project records or status values.
- Create strict JSON for the **Run Project Pulse Dashboard** configuration. Set `cwd` to `${workspaceFolder}/app`, run `python3 -m http.server 5500`, and configure `serverReadyAction` to open `http://localhost:%s/index.html`. Do not add a task or support file unless the Orchestrator approves a scope change.

These tasks can run in parallel because the data contract and CSS hooks are specified here and the files do not overlap.

### 2. Build the page after the contract and parallel work are ready

**Coder — owns `app/index.html`**
- Set the page title to **Project Pulse** and reference `styles.css` and `project-data.json`.
- Use semantic, accessible markup and render one `.project-card` per project, displaying its name, owner, status, recent activity, priority, and summary.
- Load the JSON from the served app origin. Since the repository has no JavaScript file or framework, keep the small rendering logic in `index.html`; do not introduce additional files.
- Provide an understandable empty state and a visible loading or error message if the data cannot be loaded.
- Insert data as text rather than interpreting it as HTML.

This step depends on the agreed CSS hooks and the JSON field contract from Step 1. It must finish before end-to-end preview validation.

### 3. Integrate and validate

**Orchestrator — review only; route fixes to the owner of the affected file**
- Check that the stylesheet, data, and launch setup work together; avoid editing specialist-owned files directly.
- Run the preview and verify it opens the dashboard page, not a directory listing. Stop the preview server afterward.

## Dependencies and work ordering
- **Can run in parallel:** Designer styling and Coder data/launch setup, after agreeing on the required CSS hooks and data fields.
- **Must run sequentially:** Coder builds `app/index.html` after those contracts are set; integration and preview validation follow the completed HTML and launch configuration.
- **File ownership:** Designer owns only `app/styles.css`. Coder owns `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`. Orchestrator reviews and delegates corrections to the relevant owner.

## Edge cases to handle
- Missing, empty, malformed, or unavailable project data: show a useful message rather than a blank dashboard.
- Empty `projects` array or incomplete/unexpected field values: avoid broken cards and show safe fallback text where appropriate.
- Narrow screens, keyboard navigation, visible focus, and status or priority distinctions that remain understandable without color.
- Port 5500 already in use, `python3` unavailable, or VS Code failing to recognize the server-ready output.
- File-based opening may prevent JSON loading; validate through the configured HTTP server.

## Validation expectations
- Confirm all four assigned files exist.
- Parse `app/project-data.json` and `.vscode/launch.json` as JSON; confirm the data has a top-level `projects` array and the required fields.
- Check `app/index.html` for the exact title, references to the stylesheet and data, project-card rendering, and display of status, `recentActivity`, and priority.
- Check `app/styles.css` for `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`, then review responsiveness, contrast, and focus states.
- Start **Run Project Pulse Dashboard** in VS Code. Confirm the browser reaches `http://localhost:<captured-port>/index.html`, shows rendered project cards, and does not show a directory listing. Test the data-load error or empty state if practical, then stop the server.
- Repository Step 3 checks files, required phrases, and JSON parsing, but does **not** run the app; the manual preview check is therefore required.

## Open questions and uncertainties
- The repository does not specify actual project names, owners, activity, summaries, priority values, or status vocabulary. The Coder should use clearly identified example data unless Mona supplies real records.
- The dev-container extension list does not explicitly include a Python debugger. Confirm the chosen VS Code launch mechanism is supported in the Codespace; the required command, working directory, port, and browser URL should remain as specified.
- The exact visual palette is not prescribed; the Designer should choose an accessible, coherent treatment.
