# ADR-0005 — Diagram tool

**Area:** docs · **Status:** active · **Date:** 2026-06-16

## Context

The project needs versioned diagrams in Markdown (process flows, architecture, sequences, git
history). The main requirement is native rendering on GitHub, where the public repository lives,
and diagrams-as-code (versionable, diff-reviewable, no binary committed to the repo).

## Decision

**[Mermaid](https://mermaid.js.org/)** for all diagrams in this repository.

Only diagram types with a GA (stable) designation in the Mermaid documentation are used freely.
Experimental types without an empirically confirmed render on GitHub are not used. Style rules to
survive both light and dark GitHub themes:

- Color nodes by their **border** (`stroke`), not by `fill` alone — a light `fill` without an
  explicit `color:` renders unreadable on dark themes.
- If filling a node, set `color:` explicitly with ≥ 4.5:1 contrast.
- Use `<br>` for line breaks, not `\n` in regular strings.
- Set the theme via YAML frontmatter (`config.theme`); the deprecated `%%{init}%%` directive is
  not used.
- `click`/href links are disabled by GitHub's renderer (`securityLevel: strict`) and are not used.

## Rejected alternatives

- **D2, PlantUML, Graphviz** — absent from the formats GitHub renders natively; they would require
  pre-rendered SVG/PNG committed as binary, which is not versionable diagram-as-code.
- **draw.io** — a GUI editor that exports a binary image, not text.
- **Excalidraw** — a GUI whiteboard, not structured diagram-as-code.

## Consequences

- GitHub's documentation lists the formats it renders natively as a closed enumeration
  (`mermaid`, `geojson`, `topojson`, `stl`); Mermaid is the only diagram-as-code option in it.
- Mermaid also renders natively on GitLab and Azure DevOps wikis.
- If GitHub changes its Mermaid version, the empirically confirmed types must be re-verified.
