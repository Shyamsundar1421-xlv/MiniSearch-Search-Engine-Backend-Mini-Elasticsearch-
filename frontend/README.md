# MiniSearch Frontend — Enhanced Edition

A responsive, vanilla HTML/CSS/JavaScript frontend for the MiniSearch search-engine project. This release improves the existing interface while preserving the documented backend contract. No backend routes, request parameters, ranking logic, or response formats are changed.

## What is included

### Existing MiniSearch features retained
- Public product landing page.
- Search workspace with OR/AND mode, result limits, pagination, snippets, matched-term highlighting, correction notice, and empty/error states.
- Full document reader with copy action.
- Live index metrics from `GET /stats`.
- Browser-session analytics, latency chart, popular queries, activity table, and CSV export.
- Responsive navigation and light/dark theme.

### Frontend-only upgrades
- Search suggestions based on locally stored recent and saved searches (no backend autocomplete request).
- Save and remove frequently used searches using browser `localStorage`.
- Persistent recent-search history and theme preference.
- Client-side filtering of the currently loaded result page.
- Client-side sorting by relevance order, title, or returned score.
- Keyboard navigation for suggestions; `Ctrl/Cmd + K` focuses search and `Ctrl/Cmd + Enter` repeats the current query.
- Search request sequencing so a slower earlier response cannot overwrite a newer query.
- Skeleton loading states and clearer action feedback.
- Modal focus handling, Escape-to-close, and focus restoration.
- Improved API response/error handling and safer CSV download cleanup.
- Reduced-motion support, visible focus styles, and improved dark-theme contrast.

## Project structure

```text
minisearch-frontend/
├── index.html
├── css/
│   └── styles.css
├── js/
│   ├── api.js
│   ├── app.js
│   └── config.js
├── assets/
│   └── favicon.svg
└── README.md
```

## Run locally

1. Start your existing MiniSearch backend using the command in its backend README. By default, the API should be reachable at `http://localhost:8000`.
2. Open a terminal in this frontend folder and serve the static files:

   ```bash
   python -m http.server 5500
   ```

3. Open `http://localhost:5500` in your browser.

Do not open `index.html` directly using `file://`; use a local static server so JavaScript modules and API requests work correctly.

If the backend is served on port `8000`, the frontend uses that same origin. Otherwise, the development configuration in `js/config.js` points to `http://localhost:8000`.

## Backend API contract used (unchanged)

- `GET /search?query=...&mode=OR|AND&k=10&offset=0`
- `GET /stats`
- `GET /doc/{id}`

The search response is expected to provide `query`, `mode`, `k`, `offset`, `totalHits`, `correctedQuery`, `latencyMs`, and `results[]` containing `docId`, `title`, `score`, `snippet`, and `matchedTerms`.

## Data and feature boundaries

- Recent searches, saved searches, and theme choice are stored in this browser using `localStorage`.
- Search analytics are stored only in the current browser tab's `sessionStorage`; they are not server-wide or permanent analytics.
- Autocomplete uses only local recent/saved searches. It does not claim to provide backend-powered suggestions.
- Result filtering and sorting apply only to the currently loaded result page. They do not change backend retrieval or ranking.
- Index metrics are live API data. Index rebuild remains a backend CLI operation because the documented v1 API has no rebuild endpoint.
- “Open workspace” is navigation, not authentication.
- API content is rendered as text and safe text nodes; response content is not injected as HTML.

## Notes

The frontend intentionally does not add or fake authentication, persistent server analytics, index rebuild, document browsing, or other backend-only capabilities. If the backend contract changes, update the API client only after the backend team confirms the new route and response shape.
