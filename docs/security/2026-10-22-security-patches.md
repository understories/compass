# Security patches, 2026-10-22

## Summary

`npm audit` on `app/` reported 25 vulnerabilities (2 critical, 20 high, 2 moderate, 1 low). This change upgrades Next.js within major 15, refreshes transitive dependencies with `npm audit fix`, and adds one `overrides` entry for the PostCSS copy that Next.js pins. After the change, 5 high findings remain. All 5 trace to one advisory in `braces`, a lint-time dependency, which has no patched release yet.

No application code changed.

## Changes (`app/`)

| Package | From -> To | Advisories | Why it matters here |
| --- | --- | --- | --- |
| next (direct) | 15.3.3 -> 15.5.28 | Critical: GHSA-9qr9-h5gf-34mp (RCE in React flight protocol), GHSA-2xp9-vwfh-vxw4, GHSA-p293-qw3h-jr36 (unauthenticated RCE: AVIF image optimization, Windows hosts). High: Server Components DoS, middleware bypasses, SSRF in Server Actions and rewrites, and others (34 GHSA IDs total in the audit report) | The app uses the App Router and server API routes, so these are reachable on a deployed instance. |
| eslint-config-next (direct, dev) | 15.3.3 -> 15.5.28 | Kept in step with next | Lint config only. |
| postcss (via next, `overrides`) | 8.4.31 -> 8.5.31 | GHSA-6g55-p6wh-862q, GHSA-r28c-9q8g-f849, GHSA-fxqj-rqcc-2cmp, GHSA-qx2v-qp2m-jg93 | Next.js 15 pins postcss 8.4.31. Used at build time to process CSS. The override is scoped to `next`. |
| tar | 7.4.3 -> 7.5.22 | Critical: GHSA-23hp-3jrh-7fpw (decompression DoS); High: GHSA-34x7-hfp2-rc4v, GHSA-8qq5-rm4j-mr97, GHSA-r6q2-hw4h-h46w and others (path traversal, hardlink/symlink escape) | Install and build tooling. |
| sharp | 0.34.2 -> 0.35.5 | GHSA-f88m-g3jw-g9cj, GHSA-rgj7-g3m4-5g8c, GHSA-wq5f-xc86-pv6w (libvips, librsvg, libheif CVEs) | Used by Next.js image optimization at runtime. Version allowed by next 15.5.28 (`^0.34.3 \|\| ^0.35.5`). |
| nanoid | 3.3.11 -> 3.3.20 | GHSA-28wg-ghj8-5hjv, GHSA-2v37-7h3g-55p8, GHSA-xwg4-73v4-xw9w | Transitive. |
| source-map-js | 1.2.1 -> 1.2.2 | GHSA-68fv-2mgg-jv7q | Build tooling. |
| js-yaml | 4.1.0 -> 4.3.2 | GHSA-2883-xcg3-v3hh, GHSA-52cp-r559-cp3m, GHSA-5p4m-2wfm-xmqj, GHSA-h67p-54hq-rp68, GHSA-mh29-5h37-fv8m | Lint tooling. |
| lodash | 4.17.21 -> 4.18.1 | GHSA-r5fr-rjxr-66jc, GHSA-f23m-r3pf-42rh, GHSA-xxjr-mmjv-4gpg | Transitive. |
| flatted | 3.3.3 -> 3.4.4 | GHSA-25h7-pfq9-p65f, GHSA-rf6f-7fwh-wjgh | Lint tooling. |
| minimatch | 3.1.2 -> 3.1.5, 9.0.5 -> 10.2.6 | GHSA-23c5-xmqv-rm74, GHSA-3ppc-4f35-3m26, GHSA-7r86-cg39-jmmj | Lint tooling. 10.x is the range declared by @typescript-eslint 8.72. |
| brace-expansion | 1.1.11 -> 1.1.21, 2.0.1 -> 5.0.12 | GHSA-v6h2-p8h4-qcjw, GHSA-f886-m6hf-6m8v, GHSA-3jxr-9vmj-r5cp, GHSA-mh99-v99m-4gvg, GHSA-rgw5-rvv9-x895, GHSA-6j4f-fj2g-mc7p, GHSA-qhr7-859c-m2p7, GHSA-q2hr-2g5m-vwhr | Lint tooling. |
| picomatch | 2.3.1 -> 2.3.2, 4.0.2 -> 4.0.7 | GHSA-c2c7-rcm5-vvqj, GHSA-3v7f-55p6-f55p | Lint tooling. |
| @typescript-eslint/* | 8.33.0 -> 8.72.0 | Via minimatch | Lint tooling. |
| ajv, @eslint/plugin-kit, @humanfs/node | 6.12.6 -> 6.15.0, 0.3.1 -> 0.3.5, 0.16.6 -> 0.16.8 | GHSA-2g4f-4pwh-qvx6, GHSA-xffm-g5w8-qvg7, GHSA-p498-v437-472g | Lint tooling. |

## Verification

Run in `app/` with Node 26.12.0, npm 11.20.0, before and after the change, from a clean `npm ci`:

| Check | Before | After |
| --- | --- | --- |
| `npm ci` | pass | pass |
| `npx next lint` | fails, 17 errors | fails, same 17 errors (unchanged, pre-existing) |
| `npx tsc --noEmit` | fails, 1 error in `components/ProblemForm.tsx` | same 1 error (pre-existing) |
| `npm run build` (with `OPENAI_API_KEY` set) | pass | pass, same route list |
| `next start` + `GET /` | not run | 200 |

`npm run build` fails without `OPENAI_API_KEY` because `app/api/generate-problem` creates the OpenAI client at import time. This is pre-existing. Set the variable (any value works for a build) when building.

`npm audit` after the change: 0 critical, 5 high, 0 moderate, 0 low.

## Not fixed

- `braces` <= 3.0.3 (GHSA-vfj7-8cjw-p6xm, stack exhaustion DoS on deeply nested patterns). No patched release exists. It is pulled in by `fast-glob` -> `micromatch` under `@next/eslint-plugin-next`, so the 5 remaining findings (`braces`, `micromatch`, `fast-glob`, `@next/eslint-plugin-next`, `eslint-config-next`) are one issue. npm suggests `eslint-config-next@14.2.35`, a major downgrade, which we did not apply. It only runs during `next lint` on the project's own files, not in the deployed app.
- The root `package.json` (outside `app/`) lists `next@14.1.0` and other packages but has no lockfile and no source that uses it. It was not audited or changed. If you install from the repo root, those versions are vulnerable. Use `app/`.

## What deployers should do

1. Pull this change, run `npm ci` in `app/`, rebuild, and redeploy. A running instance keeps the old Next.js until it is rebuilt.
2. Run Node.js 22.23.4, 24.22.1, or 26.12.1 or later. The October 2026 Node.js security releases fix a Brotli decoder RCE reachable through `fetch()` and two TLS validation bypasses. Node 20 is end of life and has no fix.
3. If you host on Vercel or another platform, check the Node.js version setting in the project, since the repo does not pin one.
