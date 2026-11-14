# Planet-X × SpaceMind Integration

## Status

This branch is the reversible integration workstream. It does not replace either original repository, alter the production Supabase project, rotate secrets, or trigger a production deployment.

## Source baselines

| System | Repository / project | Baseline |
|---|---|---|
| Planet-X observatory | `kamilcodes14/planet-x-sphere-x` | `main` at `ced7a7c5817e64c515e54fed6a8df7f9aea22674` |
| SpaceMind research application | `kamilcodes14/Spacemind` | `main` at `18aa72350d386f35a2d0a047a082f23aabd737f1` |
| SpaceMind Supabase | project `aqblctjucyxqjmylojgc` | Existing project; preserve in place |

The Planet-X entry point currently reconstructs the observatory from five compressed scene parts under `assets/`. SpaceMind is a separate, full application with its own frontend, build, science modules, API integration, Supabase client, Supabase Edge Functions, migrations, tests, and research workspace.

## Non-negotiable preservation rules

SpaceMind remains the complete scientific application and the RAG/knowledge engine of Planet-X. Do not replace it with a thin chat widget or a single retrieval endpoint.

Preserve these capability groups:

- Research: Groq model routing, Tavily web research, Semantic Scholar, NASA ADS, Auto/Web/Papers modes, hybrid retrieval, citations, evidence checks, confidence warnings, and follow-up prompts.
- Scientific data: NASA NeoWs and DONKI, NOAA solar data, JPL Horizons, MAST, Gaia, XMM-Newton, SIMBAD, VizieR, Exoplanet Archive, PDS, and launch information.
- Workspace: authentication, private chat history, saved papers and PDFs, citation networks, collections/projects, notes, calculations, exports, and account/privacy settings.
- Mission Lab: Pyodide execution, orbit/transfer calculations, rocket equation and delta-v, light curves/transits, HR diagrams, Gaia queries, and SPICE/Horizons workflows.
- Infrastructure: Supabase Auth/Postgres, vector corpus and ingestion, Edge Functions, scheduled digests/alerts, RLS/ownership checks, rate limiting/caching, and regression tests.

## Existing data and services to reuse

The audited Supabase project contains the existing SpaceMind data and must be reused rather than recreated. The last read-only inventory recorded 6,104 `paper_chunks` using the existing `gte-small` embedding model, plus chat, message, saved-paper, project, alert, and metrics records. Six existing Edge Functions were active at that inspection. These are a snapshot, not a migration target.

Do not:
- create a second production database or re-import the corpus;
- change the embedding model/dimension without a deliberate migration;
- expose provider API keys in browser code;
- weaken RLS or ownership checks;
- delete or silently disable existing modules, functions, migrations, tests, or user data;
- overwrite the existing SpaceMind deployment during integration.

## Target architecture

- **Planet-X shell:** the persistent observatory, spatial navigation, visual identity, and scene lifecycle.
- **SpaceMind application:** the complete research and Mission Lab experience, exposed as a first-class workspace inside the Planet-X universe.
- **SpaceMind backend:** existing Supabase Auth, Postgres, vector retrieval, Edge Functions, scheduled jobs, and server-side provider routing.
- **Scientific providers:** remain behind the existing SpaceMind server-side integration layer.

The observatory should remain available while users move between Research AI, Mission Lab, NASA Science, and Paper Library. Research workspace state and authentication should remain owned by SpaceMind, not duplicated in the scene renderer.

## Integration sequence

1. Freeze and retain the source baselines above.
2. Establish the SpaceMind frontend's actual hosting origin, allowed embedding/navigation behavior, auth redirect URLs, and CSP/frame policy. Do not assume an iframe is allowed until verified.
3. Choose the deployment topology: preferably keep SpaceMind's backend and app deployment intact, and integrate its full frontend into the Planet-X shell through a tested same-origin route or an explicitly supported embedded workspace.
4. Preserve all existing SpaceMind frontend assets, science modules, API routes, Supabase functions, migrations, tests, and build steps. Do not manually copy only `app.js` or only the chat UI.
5. Add observatory navigation and lifecycle hooks without replacing the scene loader or breaking the five scene parts.
6. Validate auth/session continuity, private data isolation, RAG retrieval, citations, provider fallbacks, paper workflows, Mission Lab, and all scientific-data routes.
7. Run the current test/check/build scripts and record actual results. Resolve the documented test-count discrepancy against the executable suite.
8. Publish a preview and obtain acceptance before any production promotion or merge.

## Acceptance checklist

- [ ] Observatory loads unchanged and remains available across workspace navigation.
- [ ] Research AI supports Auto, Web, and Papers modes.
- [ ] Existing RAG corpus is queried in place; no destructive re-ingestion occurs.
- [ ] Citations, evidence summaries, and confidence warnings render correctly.
- [ ] Existing sign-in, Google auth, password recovery, and private chat history work.
- [ ] Paper library, PDF extraction, collections, projects, notes, and exports work.
- [ ] Mission Lab tools execute and preserve reproducibility outputs.
- [ ] NASA/NOAA/ESA/JPL data routes and graceful provider fallbacks work.
- [ ] RLS and ownership boundaries are verified for multiple accounts.
- [ ] Existing SpaceMind tests, build, and checks pass; failures are documented.
- [ ] Preview is accepted before production deployment.

## Current limitations of this first pass

This commit records the audited baseline and integration contract only. The source repositories are separate, and the current Planet-X GitHub Pages loader replaces the document with the decompressed observatory scene. A safe persistent workspace cannot be wired by blindly adding a static iframe or copying a few frontend files: the actual SpaceMind hosting origin, frame policy, route behavior, and authentication redirects must first be verified. No end-to-end tests have been run as part of this baseline commit.
