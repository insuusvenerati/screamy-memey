# Screamy-Memey Project Instructions

## Architecture Overview

This monorepo consists of two workspaces:

- `cms/`: Strapi v5 headless CMS for managing content and APIs
- `frontend/`: Astro static site generator with React components for the user-facing site

Data flows from Strapi's REST API to Astro pages/components. Strapi handles content types like site settings (singleType) and serves data via `/api/` endpoints.

## Key Developer Workflows

- **Start CMS development**: `npm run cms:dev` (launches Strapi with auto-reload)
- **Start frontend development**: `npm run frontend:dev` (launches Astro dev server at localhost:4321)
- **Build frontend for production**: `npm run frontend:build` then `docker build -t <tag> frontend/`
- **Deploy frontend**: Run the Docker container with nginx serving static files on port 8080

## Project-Specific Conventions

- **TypeScript**: Enforced in both workspaces; use strict configs for type safety
- **Strapi Content Types**: Define schemas in `content-types/`, use factory functions for controllers/services/routes (e.g., `factories.createCoreController`)
- **API Fetching**: Always use `frontend/src/lib/strapi.ts` `fetchApi` function with `wrappedByKey: "data"` to unwrap Strapi v5 responses
- **Global Settings**: Implement as singleType content types (e.g., site-setting) for site-wide config
- **Path Aliases**: Frontend TSConfig includes `@cms/*` for potential CMS imports, though currently unused
- **Docker**: Multi-stage build for frontend; nginx config in `frontend/nginx/nginx.conf` for SPA routing

## Examples from Codebase

- Site settings fetched in layout for meta tags: [frontend/src/layouts/Layout.astro](frontend/src/layouts/Layout.astro#L4-L10)
- Content type definition: [cms/src/api/site-setting/content-types/site-setting/schema.json](cms/src/api/site-setting/content-types/site-setting/schema.json)
- API utility pattern: [frontend/src/lib/strapi.ts](frontend/src/lib/strapi.ts#L10-L25)
- Strapi controller/service boilerplate: [cms/src/api/site-setting/controllers/site-setting.ts](cms/src/api/site-setting/controllers/site-setting.ts)
