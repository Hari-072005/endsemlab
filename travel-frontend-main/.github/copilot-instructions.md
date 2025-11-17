## Purpose
Short, actionable guidance for AI coding agents working on this repository (React + Vite SPA).

## Quick facts
- Framework: React (v19) + Vite
- Router: `react-router-dom` (routes declared in `src/App.jsx`)
- HTTP client: `axios` (used directly in pages)
- Main entry: `src/main.jsx` → renders `App` (`src/App.jsx`)
- Pages: `src/pages/*` (Login.jsx, Signup.jsx, Dashboard.jsx)
- UI components: `src/components/*` (e.g., `Navbar.jsx`)

## Dev / build / lint commands
- Install: `npm install` (project root)
- Dev server: `npm run dev` (runs `vite`)
- Build: `npm run build` (Vite build)
- Preview build: `npm run preview`
- Lint: `npm run lint` (runs `eslint .`)

Note: There are no tests configured in this repo. Expect manual testing via the browser and console.

## Important app flows & integration points
- Authentication flow (client-only):
  - `src/pages/Login.jsx` posts to `http://localhost:8081/auth/login` and stores the returned token with `localStorage.setItem('token', res.data)` and `username`.
  - `src/pages/Signup.jsx` posts to `http://localhost:8081/auth/signup`.
  - `src/pages/Dashboard.jsx` checks `localStorage.getItem('token')` in a `useEffect` and redirects to `/` if missing.
  - `src/components/Navbar.jsx` reads `localStorage.getItem('token')` to toggle links and implements logout by clearing `localStorage` and navigating to `/`.

Implication: a backend expected at `http://localhost:8081`. Agents should not change hard-coded backend URLs without centralizing them first.

## Project-specific conventions / patterns
- API calls are made inline inside page components (e.g., `axios.post("http://localhost:8081/auth/login", form)` in `Login.jsx`). Prefer minimal, targeted changes: if refactoring, create a single `src/api.js` with an axios instance and replace occurrences.
- Auth storage: token and username are stored in `localStorage` keys `token` and `username`.
- Routing: `App.jsx` defines three top-level routes: `/` → Login, `/signup` → Signup, `/dashboard` → Dashboard.
- Styling: app-level styles live in `src/App.css` and `src/index.css`. Components import these directly.
- Error handling: pages use `try/catch` and `alert()` for UX; changes to error UX should be done consistently across pages.

## Safe, high-value edits agents can make
- Centralize API base URL: add `src/api.js` exporting an axios instance with a single `baseURL` variable, then update `Login.jsx` and `Signup.jsx` to use it. Example (conceptual):

  - Find occurrences: `axios.post("http://localhost:8081/auth/login", ...)` and `axios.post("http://localhost:8081/auth/signup", ...)`.

- Improve auth check: extract the `localStorage` token-check into a small `useAuthRedirect` hook used by `Dashboard.jsx` and others.
- Small UX fixes: replace `window.location.href = "/"` in `Navbar.jsx` logout with `useNavigate()` to keep SPA behavior.

## When to ask for clarification
- If a change touches a backend contract (request/response shapes, status codes) — ask for the backend API spec or sample responses.
- If adding global state (Redux/Context) — ask whether to introduce a new dependency or keep minimal hooks.

## Files to look at for examples
- Routing + entry: `src/App.jsx`, `src/main.jsx`
- Login flow: `src/pages/Login.jsx`
- Signup flow: `src/pages/Signup.jsx`
- Auth gating + dashboard: `src/pages/Dashboard.jsx`
- Nav + logout: `src/components/Navbar.jsx`
- Build/dev configuration: `package.json`, `vite.config.js`

## Non-goals / avoid
- Don't attempt to launch or change backend URLs without creating a central config (avoid multiple hard-coded edits).
- Don't modify global app structure (e.g., change router to server-side routes) without explicit instruction.

---
If any of these environment details are incomplete (backend location, expected token shape, or test scripts), tell me and I will update the instructions. Ready for review — tell me what to clarify or add.
