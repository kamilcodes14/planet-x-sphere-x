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

## Implemented preview work

Two draft pull requests now hold the reversible integration changes:

- Planet-X: [PR #1](https://github.com/kamilcodes14/planet-x-sphere-x/pull/1)
- SpaceMind: [PR #8](https://github.com/kamilcodes14/Spacemind/pull/8)

The Planet-X branch injects a small Research dock into the HTML after the original compressed scene is decompressed. It does not rewrite the five scene-part assets. The dock opens the existing full SpaceMind frontend in a large overlay and offers a separate-tab fallback. For safe preview testing, both the iframe and separate-tab link currently point to the SpaceMind integration preview deployment; after the SpaceMind framing change is accepted and merged, switch those two URLs to the stable `https://spacemind-frontend.vercel.app/` alias before merging the Planet-X PR.

The SpaceMind branch removes the blanket `X-Frame-Options: DENY` header and adds a restrictive `frame-ancestors` policy allowing only same-origin and `https://kamilcodes14.github.io`. The existing `nosniff` and `Referrer-Policy` headers remain. No Supabase schema, corpus, Edge Function, provider secret, or production alias was changed.

## Validation status

- [x] SpaceMind Vercel preview deployment built successfully and reached READY for commit `7a2175e20eec073eb41b37b867e82f2db06f464f`.
- [x] Planet-X loader JavaScript passed `node --check` locally.
- [x] The Planet-X scene-part assets were not modified in this branch.
- [ ] Verify the deployed preview's actual HTTP response headers and iframe rendering in a browser.
- [ ] Verify sign-in/session continuity, especially Google OAuth and browser third-party storage behavior in an embedded context. Use the separate-tab fallback if required.
- [ ] Run full SpaceMind tests and scientific workflow acceptance tests.
- [ ] Validate the Planet-X preview in a browser on desktop and mobile.
- [ ] Merge only after both previews and acceptance checks pass.

The SpaceMind Vercel preview is available at:
`https://spacemind-frontend-git-integration-planet-x-embed-syed-kamil.vercel.app`

The current Planet-X repository does not expose a branch preview deployment through the connected hosting setup, so its dock still needs browser validation before merge. The SpaceMind preview is READY, but the separate Vercel project named `spacemind` reported an unrelated deployment error; this integration uses the existing `spacemind-frontend` project. No production deployment or Supabase change has been made.
