# geosphere-thesaurus

## Technical Summary

Geosphere Thesaurus is a static, client-side web application for browsing and querying geoscience vocabularies published as SKOS datasets. The app is served from plain HTML pages (for example `index.html`, `network.html`, and ECharts-specific views) and relies on JavaScript modules in `js/` for data access, UI composition, multilingual text, and visualization behavior. There is no backend application in this repository; runtime data is loaded directly in the browser from a GraphDB SPARQL endpoint.

The main data integration layer is `js/ws.js`, which builds SPARQL requests and executes them against `https://resource.geosphere.at/graphdb/repositories/thes`. Query templates are parameterized via `js/config.js`, where project-specific settings define named datasets (`FROM` clauses), diagram preferences (tree, circle, sunburst), URI normalization rules, and metadata loading. At startup, `js/page.js` parses URL parameters (`uri`, `search`, `lang`, `embedded`), initializes localization, loads project metadata, and renders either overview cards, concept detail pages, or search results.

Search and discovery are implemented in `js/search.js` using Fuse.js for in-browser fuzzy matching over labels fetched from SPARQL. Detail views and graph navigation are supported by D3 and ECharts modules (`d3_*.js`, `echarts_*.js`) that transform SKOS broader/narrower relations into hierarchical structures. `js/d3_data.js` is the core hierarchy builder: it fetches concept edges, resolves labels and optional styling metadata, prevents graph loops, and prepares trees used by visual components.

Static assets are organized under `css/`, `img/`, `fonts/`, and `rdf/` (local RDF files for reference/data packaging). Tooling is minimal and focused on code quality and release automation: ESLint (`npm run lint`), Husky hooks (`prepare`), Commitlint, and Semantic Release (`npm run release`). This keeps deployment simple while preserving maintainability for a browser-first linked-data application.
