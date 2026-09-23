# Agent Instructions

## Commands

- Use Node.js 22+ and npm; `package.json` is the executable source for the Node engine requirement.
- Start development with `npm run dev` on port 4321. The component library is available at `/component-docs/` and its builder at `/component-docs/component-builder/`.
- Run `npm run check` for the normal pre-commit check: ESLint (JS, Astro, and YAML), Stylelint (CSS), then Prettier check.
- Run `npm run build` for the production build; it sets `DISABLE_COMPONENT_LIBRARY=true`, so component-docs routes are excluded. Use `npm run build:with-library` when those routes must be built and verified.
- Run `npm run deps:sync` when changing dependencies, especially on macOS; it recreates the lockfile with Linux, Windows, and macOS resolutions. Verify with `npm run deps:check`.

## Structure

- This is a single Astro site. Runtime page routes are in `src/pages/`; content-driven pages use `src/content/pages/*.md`, blog posts use `src/content/blog/*.mdx`, and component documentation lives under `src/component-docs/`.
- `src/content.config.ts` defines the content collections and schemas. Update schemas when changing frontmatter or content block data rather than bypassing typed collections.
- Components are grouped under `src/components/building-blocks/`, `src/components/page-sections/`, and `src/components/navigation/`. Shared rendering and component lookup are handled by `src/components/utils/renderBlock.astro` and `MainComponent.astro`.
- Prefer the configured aliases such as `@components`, `@page-sections`, `@content`, `@layouts`, and `@styles`; their source-of-truth mappings are in `astro.config.mjs` and `tsconfig.json`.
- Design tokens and global styles are in `src/styles/variables/`, `src/styles/themes/`, and `src/styles/`; update tokens rather than scattering site-wide values through components.

## Components And Content

- A CloudCannon-editable component normally requires the Astro component plus same-folder `<name>.cloudcannon.inputs.yml` and `<name>.cloudcannon.structure-value.yml`; keep these definitions aligned when adding or changing exposed props.
- Page sections are selected from content block data using the `_component` field. Preserve the component path and prop names in Markdown/MDX data when renaming or moving components.
- CloudCannon collection paths and upload locations are defined in `cloudcannon.config.yml`; pages, blog posts, and data files are not interchangeable (`.md`, `.mdx`, and `.json` respectively).

## Formatting

- Follow the repository Prettier config: two spaces, semicolons, double quotes by default, 100-column width, and Astro-specific formatting. Do not hand-format around the formatter; run `npm run format:fix` only when intentionally formatting the whole repository.
- YAML config files are checked for double quotes and required key ordering by `eslint.config.js`; run the focused `npm run lint:yml` after changing CloudCannon YAML.
