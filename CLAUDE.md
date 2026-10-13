# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

"PokémonDeta" — a Laboratoria *Data Lovers* bootcamp project (package name `data-lovers`). A static, no-build, vanilla-JS site that lists the 151 Gen-1 Pokémon (Pokémon Go data) and lets the user search by name/number, sort by number, filter by type or weakness, and open a detail card. README and code comments are in Spanish; UI text is in English.

## Commands

Dependencies are not installed by default — run `npm install` first.

- `npm start` — serve `src/` as a static site (`serve`). Must be served over HTTP because the scripts are ES modules.
- `npm test` — runs `pretest` first (ESLint on `src/` and `test/`, then HTMLHint on `src/*.html`), then Jest with coverage. A lint failure aborts the test run.
- `npm run eslint` / `npm run htmlhint` — run the linters alone.
- Single test file / test name: `npx jest test/data.spec.js` or `npx jest -t "searchPokemon"`.
- `npm run deploy` — publishes `src/` to GitHub Pages (`gh-pages`).

## Architecture

- `src/index.html` loads `main.js` and `menu.js` as `type="module"`; there is no bundler. Babel (`@babel/plugin-transform-modules-commonjs`) exists only so Jest can import the ES modules.
- `src/data.js` — **pure** data functions (`searchPokemon`, `orderPokeData`, `filterPokeTypes`, `filterPokeWeakness`). This is the only unit-tested module; keep DOM code out of it.
- `src/main.js` — imports the dataset from `src/data/pokemon/pokemon.js` (a JS module `export default { pokemon: [...] }`), wires DOM events to the `data.js` functions, renders cards into `#root`, and builds the detail view into `#infoCard` via an `innerHTML` template.
- `src/menu.js` — only UI toggles (mobile/desktop menu, About/Contact panels, logo click plays `audio/pokemonsong.mp3`) by toggling CSS classes (`appear`, `appearTwo`, …).
- Styles: `style.css` (main), `styleheader.css` (header/menu), `respstyle.css` (responsive breakpoints).
- `src/data/lol` and `src/data/rickandmorty` are unused alternative datasets from the bootcamp template. `.gitignore` lists `src/data`, but these files are already tracked.

## Gotchas

- `num` is a zero-padded string (`"025"`): `searchPokemon` treats numeric input as a substring match on `num`, and sorting relies on string-to-number coercion.
- `orderPokeData` uses `Array.prototype.sort`, which mutates the shared `pokeDex` array in `main.js`; it returns `undefined` for values other than `"ascend"`/`"descend"`.
- ESLint (`eslint:recommended`, browser env, ES2018 modules) warns on `console`; `test/.eslintrc` adds the Jest env. `.editorconfig`: 2-space indents, trailing whitespace trimmed, and a newline at the end of each file.
