# Security dependency update — 2026-10-21

## Summary

Routine dependency security pass across every npm manifest in the repository:

| Manifest | Lockfile | Audit before | Audit after |
| --- | --- | --- | --- |
| `app/package.json` (the Next.js app) | `app/package-lock.json` | 25 (2 critical, 20 high, 2 moderate, 1 low) | 5 (5 high, all from one unfixable `braces` advisory) |
| `package.json` (root; no source code next to it) | none committed | 17 (1 critical, 13 high, 3 moderate) | 9 (1 critical, 5 high, 3 moderate) |
| `vlayer/vlayer/package.json` (vlayer email-proof example, run with bun) | none committed | 48 (14 high, 34 moderate) | 29 (11 high, 18 moderate) |

Audit counts for the two manifests without a lockfile come from `npm audit` run against a temporary lockfile
(`npm install --package-lock-only`) that was **not** committed. Those numbers are for dependencies as they resolve today.

All updates stay within the current major version. No major version upgrade was made.

## Advisories fixed

### `app/`

| Package | Severity | Advisories | From → To |
| --- | --- | --- | --- |
| next (direct) | critical | GHSA-9qr9-h5gf-34mp, GHSA-p293-qw3h-jr36, GHSA-2xp9-vwfh-vxw4 (critical); GHSA-267c-6grr-h53f, GHSA-26hh-7cqf-hhc6, GHSA-36qx-fr4f-26g5, GHSA-89xv-2m56-2m9x, GHSA-8h8q-6873-q5fj, GHSA-c4j6-fc7j-m34r, GHSA-h25m-26qc-wcjf, GHSA-m99w-x7hq-7vfj, GHSA-mg66-mrh9-m8jx, GHSA-mwv6-3258-q52c, GHSA-p9j2-gv94-2wf4, GHSA-q4gf-8mx6-v5v3 (high); plus 20 moderate/low Next.js advisories (34 in total) | 15.3.3 → 15.5.28 |
| eslint-config-next (direct, dev) | — | kept in lock-step with `next` | 15.3.3 → 15.5.28 |
| tar (transitive) | critical | GHSA-34x7-hfp2-rc4v, GHSA-8qq5-rm4j-mr97, GHSA-83g3-92jg-28cx, GHSA-qffp-2rhf-9h96, GHSA-9ppj-qmqm-q256, GHSA-r6q2-hw4h-h46w, GHSA-vmf3-w455-68vh, GHSA-w8wr-v893-vjvp, GHSA-23hp-3jrh-7fpw, GHSA-8x88-c5mf-7j5w, GHSA-gvwx-54wh-qm9j, GHSA-r292-9mhp-454m | 7.4.3 → removed from the tree |
| sharp (via next) | high | GHSA-f88m-g3jw-g9cj, GHSA-rgj7-g3m4-5g8c, GHSA-wq5f-xc86-pv6w | 0.34.2 → 0.35.5 |
| postcss (via next, @tailwindcss/postcss) | high | GHSA-qx2v-qp2m-jg93, GHSA-6g55-p6wh-862q, GHSA-fxqj-rqcc-2cmp, GHSA-r28c-9q8g-f849 | 8.4.31 / 8.5.4 → 8.5.31 (`overrides.next.postcss`) |
| source-map-js | high | GHSA-68fv-2mgg-jv7q | 1.2.1 → 1.2.2 |
| nanoid | high | GHSA-28wg-ghj8-5hjv, GHSA-2v37-7h3g-55p8, GHSA-xwg4-73v4-xw9w | 3.3.11 → 3.3.20 |
| lodash | high | GHSA-r5fr-rjxr-66jc, GHSA-f23m-r3pf-42rh, GHSA-xxjr-mmjv-4gpg | 4.17.21 → 4.18.1 |
| js-yaml | high | GHSA-2883-xcg3-v3hh, GHSA-52cp-r559-cp3m, GHSA-5p4m-2wfm-xmqj, GHSA-h67p-54hq-rp68, GHSA-mh29-5h37-fv8m | 4.1.0 → 4.3.2 |
| flatted | high | GHSA-25h7-pfq9-p65f, GHSA-rf6f-7fwh-wjgh | 3.3.3 → 3.4.4 |
| minimatch | high | GHSA-23c5-xmqv-rm74, GHSA-3ppc-4f35-3m26, GHSA-7r86-cg39-jmmj | 3.1.2 / 9.0.5 → 3.1.5 / 10.2.6 |
| brace-expansion | high | GHSA-3jxr-9vmj-r5cp, GHSA-6j4f-fj2g-mc7p, GHSA-mh99-v99m-4gvg, GHSA-qhr7-859c-m2p7, GHSA-rgw5-rvv9-x895, GHSA-f886-m6hf-6m8v, GHSA-q2hr-2g5m-vwhr, GHSA-v6h2-p8h4-qcjw | 1.1.11 / 2.0.1 → 1.1.21 / 5.0.12 |
| picomatch | high | GHSA-c2c7-rcm5-vvqj, GHSA-3v7f-55p6-f55p | 2.3.1 / 4.0.2 → 2.3.2 / 4.0.7 |
| ajv | moderate | GHSA-2g4f-4pwh-qvx6 | 6.12.6 → 6.15.0 |
| @humanfs/node | moderate | GHSA-p498-v437-472g | 0.16.6 → 0.16.8 |
| @eslint/plugin-kit | low | GHSA-xffm-g5w8-qvg7 | 0.3.1 → 0.4.1 |

### Root `package.json`

| Package | Severity | From → To |
| --- | --- | --- |
| next (direct) | critical/high | 14.1.0 → 14.2.35 (latest 14.x; fixes the advisories that have a 14.x patch) |
| eslint-config-next (direct, dev) | high | 14.1.0 → 14.2.35 |
| postcss (via next) | high | 8.4.31 → 8.5.31 (`overrides.next.postcss`) |
| glob (via @next/eslint-plugin-next) | high, GHSA-5j98-mcp5-4vw2 | 10.3.10 → 10.5.0 (`overrides["@next/eslint-plugin-next"].glob`) |
| minimatch, @typescript-eslint/* | high | updated within range |

### `vlayer/vlayer/package.json`

| Package | Severity | Advisories | From → To |
| --- | --- | --- | --- |
| playwright, @playwright/test (direct, dev) | high | GHSA-7mvr-c777-76hp | 1.52.0 → 1.64.1 |
| ws (via viem) | high/moderate | GHSA-96hv-2xvq-fx4p, GHSA-58qx-3vcg-4xpx | 8.18.1 → 8.22.0 (`overrides.viem.ws`) |
| @reown/appkit, @reown/appkit-adapter-wagmi and dependents | moderate | (via viem/ws chain) | ^1.6.9 → ^1.8.24 |

The existing `viem` (2.27.0) and `wagmi` (2.14.16) pins and overrides were kept unchanged.

## Other dependency updates (same major)

- `app/`: every dependency raised to the latest version within its range. Declared ranges were raised to match: all `@radix-ui/*`, `@hookform/resolvers` 5.9.2, `openai` 5.23.2, `react`/`react-dom` 18.3.1, `react-hook-form` 7.90.0, `sonner` 2.0.8, `tailwind-merge` 3.7.0, `zod` 3.25.76, `tailwindcss`/`@tailwindcss/postcss` 4.3.3, `eslint` 9.39.5, `typescript` 5.9.3, `@types/*`, etc.
- Root: `openai` 4.104.0, `react-hook-form` 7.90.0, `tailwindcss` 3.4.19, `typescript` 5.9.3, `zod` 3.25.76, and others within range.
- `vlayer/vlayer`: `@tanstack/react-query` 5.104.5, `motion` 12.43.0, `react-router` 7.18.4, `uuid` 11.1.1, `vite` 5.4.21, `typescript` 5.9.3, and others within range.

## Verification

Node v22.22.0, npm 10.9.4. The same commands were run before and after the change.

### `app/`

| Command | Before | After |
| --- | --- | --- |
| `rm -rf node_modules .next && npm ci` | ok | ok |
| `npm audit` | 25 vulnerabilities | 5 high (one advisory) |
| `npm run lint` | fails, 17 errors (already there: `no-explicit-any`, `no-unescaped-entities`, `no-empty-object-type`) | fails, the same 17 errors |
| `npx tsc --noEmit` | fails, 1 error (already there: `components/ProblemForm.tsx(130,14)` TS2345) | fails, the same 1 error |
| `OPENAI_API_KEY=dummy npm run build` | passes | passes |
| `npm run build` without `OPENAI_API_KEY` | fails while collecting page data (OpenAI client built at module load) | fails the same way |

### Root `package.json`

There is no source code next to this manifest (the app lives in `app/`), so there is no build, lint or test to run.
It was checked only with `npm install` and `npm audit` against a temporary lockfile.

### `vlayer/vlayer`

Checked in a scratch copy using `npm install` (no lockfile is committed; the README uses bun):

| Command | Before | After |
| --- | --- | --- |
| `npx tsc --noEmit` | fails, 10 errors (already there: missing forge `../out/*` artifacts, wagmi/@wagmi/core type mismatch, motion `ease` typing) | fails, the same 10 errors |
| `npx vite build` (with stub `../out` artifacts) | fails: `Missing "./tempo" specifier in "viem" package` (pinned viem 2.27.0 is older than what freshly resolved connectors expect) | fails the same way |
| `npx solhint '../src/**/*.sol' --max-warnings 0` | fails, 1 error (`Counter.sol` compiler-version) | fails, the same 1 error |
| Playwright e2e (`test-web:*`) | not run (needs the vlayer devnet, docker and forge) | not run |

## Remaining items

| Manifest | Package | Severity | Reason not fixed |
| --- | --- | --- | --- |
| app | braces (via eslint-config-next → @next/eslint-plugin-next → fast-glob → micromatch), GHSA-vfj7-8cjw-p6xm | high | No patched `braces` release exists (every version is affected). Dev/lint-only path. |
| root | next 14.2.35: GHSA-p293-qw3h-jr36, GHSA-2xp9-vwfh-vxw4 (critical), plus other Next.js advisories patched only in 15.5.x | critical | The fix needs Next 15 (a major upgrade). This manifest has no code to verify the upgrade against. It looks like a leftover from before the app moved into `app/` (which is already on 15.5.28). Recommend deleting the root `package.json` or aligning it with `app/`. |
| root | braces / micromatch / fast-glob / chokidar / tailwindcss 3 chain; postcss-selector-parser (GHSA-rj75-hqrm-r3gf) via postcss-nested; tailwindcss-animate | high/moderate | No patched braces; postcss-selector-parser fix is 7.x, which is outside postcss-nested 6's range. |
| vlayer | vite 5.4.21: GHSA-fx2h-pf6j-xcff, GHSA-587h-7wmj-h5pm (high), GHSA-4w7w-66w2-5vf9, GHSA-v6wh-96g9-6wx3, GHSA-698v-4r3x-3vcv, GHSA-r825-w8mm-5pxw; esbuild GHSA-67mh-4wv8-2f99 | high/moderate | Fixed only in vite ≥ 6.4.4 (major). Dev-server only, mostly Windows-specific. Not taken because the build cannot be verified (see above). |
| vlayer | url-regex (via @vlayer/sdk 1.0.2), GHSA-v4rh-8p82-6h5w | high | No patched url-regex. `@vlayer/sdk` is pinned to match the vlayer Solidity contracts (1.0.2). |
| vlayer | @metamask/sdk 0.32.0 (GHSA-qj3p-xc97-xw74), uuid 8/9 under @metamask/* (GHSA-w5hq-g745-h8pq), decode-uri-component 0.2.2 (GHSA-vcc3-ghjq-m6fr) | moderate | They come through the pinned `wagmi` 2.14.16 / `viem` 2.27.0. Fixing them needs wagmi ≥ 2.19 or overrides outside the parents' semver ranges. |
| vlayer | braces / postcss-selector-parser via tailwindcss 3 | high/moderate | As for root. |

### Major upgrades available but not taken

- app: next 16, eslint-config-next 16, eslint 10, typescript 7, react/react-dom 19, zod 4, openai 7, lucide-react 1.x, recharts 3, date-fns 4, react-day-picker 10, react-resizable-panels 4, @hookform/resolvers 6, @types/node 26.
- root: next 15/16, tailwindcss 4, eslint 9/10, React 19, zod 4, openai 5+.
- vlayer: vite 6/7, @sentry/react 9+, wagmi/viem newer than the pins, solhint 6, tailwindcss 4.
