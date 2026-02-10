# Copilot Instructions — Soc Ops Bingo

## Development Checklist
Before committing changes:
- [ ] `npm run lint` passes (ESLint, React hooks, TypeScript strict)
- [ ] `npm run build` succeeds (TypeScript + Vite bundle)
- [ ] `npm test` passes (21 unit tests in `bingoLogic.test.ts`)

## Quick Start
```bash
npm install       # Install dependencies
npm run dev       # Start Vite dev server (http://localhost:5173)
```

## Project Overview

**Soc Ops** is a React + TypeScript + Tailwind CSS v4 social bingo game. Players tap squares with "find someone who..." prompts to get 5 in a row. Auto-deploys to GitHub Pages.

### Architecture
- **State**: `useBingoGame` hook manages game state (`'start'` | `'playing'` | `'bingo'`), 25-square board, and localStorage persistence
- **Logic**: Immutable pure functions in `utils/bingoLogic.ts` (generateBoard, toggleSquare, checkBingo) with 21 unit tests
- **Components**: App → StartScreen/GameScreen → BingoBoard → BingoSquare; BingoModal for victory
- **Data**: 24 hardcoded questions in `data/questions.ts` + free space at center (index 12)

## Key Patterns

**Immutability**: Always create new arrays with `.map()`, never mutate. Example: `toggleSquare()` returns new board array, enabling proper React reconciliation.

**TypeScript Strictness**: Domain types in `src/types/` (BingoSquareData, BingoLine, GameState as union type). Props use explicit destructuring interfaces.

**LocalStorage Validation**: Validates version, shape, and board integrity on load; gracefully falls back to new game if corrupted.

**Conditional Styling**: BingoSquare concatenates state-based Tailwind classes. References custom colors: `bg-marked`, `bg-accent` (defined in tailwind config if present).

## When Modifying

**Adding questions**: Edit `src/data/questions.ts` (must have exactly 24 items).

**Changing colors**: Update Tailwind custom properties. BingoSquare references: `bg-marked`, `border-marked-border`, `bg-accent`, `text-accent-light`.

**Changing board size**: Update `BOARD_SIZE` and `CENTER_INDEX` in `bingoLogic.ts`, modify `generateBoard()`, `getWinningLines()` row/col/diagonal logic, CSS grid in `BingoBoard.tsx`, and test suite.

**New features**: Add unit tests, follow immutability pattern, use TypeScript strict mode, validate with manual browser testing.

## Deployment
GitHub Actions auto-deploys on push to main. Sets `VITE_REPO_NAME` env var for GitHub Pages base path (e.g., `https://user.github.io/repo-name`).

## References
- Lab guide: `.lab/GUIDE.md` | Frontend design: `.github/instructions/frontend-design.instructions.md` | Tailwind: `.github/instructions/tailwind-4.instructions.md`
