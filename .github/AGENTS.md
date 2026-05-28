# AI Agent Instructions for this workspace

## Purpose
This repository contains a mostly-documentation workspace for a React/TypeScript MVP called "Gestor-Tareas-ICE". There is currently no application source code in the root or in `Gestor-Tareas-ICE` beyond a README and `.git` metadata.

## What agents should know
- The main focus is the `Gestor-Tareas-ICE` project and supporting docs in the workspace root.
- There is no `package.json`, `tsconfig.json`, or other build manifest present in this workspace, so do not assume build or test commands exist unless added later.
- The workspace is organized around planning, UI design, and implementation notes, not a complete deployable codebase yet.

## Custom agents and files
- Use `mcp-github.agent.md` and `mcp-github.agent.json` for workspace-specific MCP/GitHub guidance.
- `mcp-github.agent.md` is the local agent definition with restrictions: no file editing, no search, no browser; output in Markdown and shell commands in PowerShell.
 - `AGENT.md` (in this `.github` folder) describes expected agent behaviour for repository-wide tasks.
 - `.github/react-typescript-instructions.md` contains focused guidance for editing React + TypeScript files.

## Recommended behavior
- Prefer concise guidance and shell commands for PowerShell on Windows.
- If asked to modify files, return diffs or command suggestions instead of applying changes directly.
- When the workspace lacks explicit structure, ask the user for the intended target folder or project details before making assumptions.

## Useful files
- `README.md` — project title and high-level MVP description.
- `Gestor-Tareas-ICE/README.md` — same MVP description for the subproject.
- `REACT_CONTROL_GUIDE.md` — design and React control guidance.
- `IMPLEMENTATION_TASK_PLAN.md` — implementation plan notes.
- `MVP_Intelligent_Task_Manager_ICE.md` — MVP definition and scope.
- `UX_UI_DESIGN_COMPONENT_FLOWS.md` — UX flows and component design.

## What to do next
- If source files are added later, update these instructions with the new build/test commands and project structure.
- For automation tasks, prefer creating skills or agents that operate on the documented project conventions.
