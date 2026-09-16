# Repository migration

The destination is the existing `The-Vibe-Company/companion` GitHub repository.
Its repository identity, stars, issues and historical pull requests remain attached to it.
The product and public service domain remain companions.build.

## Git history

The migration joins the previous Companion history at
`e2821e67b2cf4651e2c2ec36dae6558010b14cf5` and companions.build at
`aaf6f45296013072672ad5482eede50bf8981a3c`.
The resulting application tree comes from companions.build, with repository migration
documentation, the updated clone URL, and the destination security/dependency automation
adapted to Bun. Both histories remain reachable; old source links
using full commit IDs continue to resolve. Do not squash this migration or rewrite either history.

Preserve all source branch tips under `companions-build/<original-branch>` in the destination,
including `companions-build/main`. Keep the old destination tip as `archive/skills-hub-before-migration`.
No source tags or releases existed at inventory time.

Open pull requests in companions.build need individual disposition. Their discussion history
stays in that repository; do not delete the source repository after migration.

## Deployment boundaries

Keep the existing companions.build Railway project, PostgreSQL database, MinIO volume,
application service IDs, credentials, OAuth callbacks and `companions.build` domain.
Connect its API, executor and worker to `The-Vibe-Company/companion`, branch `main`,
using the root Dockerfile. Preserve each role's start command, the API migration pre-deploy
command and `/health` check. The image builds the distribution before agents start.

The separate Railway project `companion-v2` serves the previous Skills Hub from
`The-Vibe-Company/skillpack`. It must not receive this application's schema or image.
The legacy Vercel project named `companion` uses Next.js in `apps/web` and only has a
`vercel.app` domain. Disconnect its Git source before pushing this migration, retaining the
existing deployment. Skillpack has its own Vercel project and Git connection. The new
application uses Vite and is served by the Railway API. Preserve the old product's domains and data.

## CI protections

The Verify workflow retains the destination's pinned Gitleaks history scan and redacted
output. Existing audited historical fingerprints remain excluded, plus one synthetic
access-token fixture from the imported skill validation tests. Dependabot continues weekly
updates for GitHub Actions, uses the Bun ecosystem for the root and web lockfiles,
and tracks the npm lockfile in `tools/dev`.

## Acceptance and rollback

Run `CHROME_BIN=<compatible Chrome executable> python3 scripts/verify.py --postgres 18` locally, and verify the
GitHub Verify workflow on the destination commit. After reconnecting Railway, check all
three application deployments against that commit and check the public health endpoint.
Verify the repository ID and stars remain attached to the original destination repository.

This migration changes repository ownership of the code; it introduces no application schema
change relative to the imported companions.build commit. For an infrastructure rollback,
reconnect those three services to companions.build at the recorded source commit. Preserve
databases, volumes and companion machines. Restore repository content with a reviewed new
commit if needed, keeping both histories intact.
