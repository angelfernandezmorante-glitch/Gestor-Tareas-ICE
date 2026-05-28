# React + TypeScript Editing Instructions

## Purpose
This file defines the preferred patterns, structure, and conventions for editing React and TypeScript files in this workspace.

## When to use
- When adding or changing React components, hooks, context, services or utilities.
- When refactoring UI logic or TypeScript typings.
- When proposing new file structure for the frontend.

## Key conventions
- Use functional components and React hooks.
- Keep each component in its own folder with co-located files:
  - `Component.tsx`
  - `Component.types.ts`
  - `Component.styles.ts`
  - `Component.test.tsx`
- Prefer simple components under 200 lines.
- Use strict typing and avoid `any`.
- Name components in `PascalCase`, hooks as `useCamelCase`, types in `PascalCase`, constants in `UPPER_SNAKE_CASE`.
- Place reusable types in `types/`, pure helpers in `utils/`, API calls in `services/`, and custom hooks in `hooks/`.

## State and architecture
- Use `TaskContext` plus `useReducer` for application state when needed.
- Keep business logic out of component render functions.
- Separate UI rendering from service and validation logic.

## Validation and UI standards
- Title: 1-100 characters.
- Description: 10-500 characters.
- ICE values must be in [1, 10].
- Use Material UI components for form controls, cards, buttons, and notifications.
- Disable submit buttons while API calls are in progress.
- Show inline validation messages and meaningful error feedback.

## API and error handling
- Use `fetch` for HTTP calls from the frontend.
- Handle timeouts, network failures, invalid JSON, and API errors gracefully.
- Do not create a task if the IA API call fails.

## File editing guidance
- Prefer editing existing files only when necessary.
- If a new component or hook is needed, create it in the recommended folder structure.
- Keep comments concise and relevant to the React/TypeScript code.

## Notes
This file is specific to React/TypeScript edits. Do not duplicate general repository guidance that belongs in `.github/copilot-instructions.md`.
