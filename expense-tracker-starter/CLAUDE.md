# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev      # Start dev server at http://localhost:5173
npm run build    # Production build
npm run preview  # Preview production build
npm run lint     # Run ESLint
```

There are no tests in this project.

## Architecture

Single-page React app (Vite + React 19). All logic lives in `src/App.jsx` — one monolithic component with no sub-components.

**State in `App.jsx`:**
- `transactions` — array of `{ id, description, amount, type, category, date }`. `amount` is stored as a **string**, which causes a known bug: `reduce` string-concatenates instead of summing, so the Income/Expenses/Balance summary values are incorrect.
- Form state: `description`, `amount`, `type`, `category`
- Filter state: `filterType`, `filterCategory`

**Data flow:** filters are applied inline during render (no derived state). The `handleSubmit` adds new transactions without parsing `amount` to a number, perpetuating the bug for newly added entries.

**Styling:** `src/App.css` uses plain CSS classes (`.income-amount`, `.expense-amount`, `.balance-amount`, `.summary-card`, etc.) — no CSS framework.
