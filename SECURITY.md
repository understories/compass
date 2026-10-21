# Security Policy

## Supported versions

Only the `main` branch is supported. Security fixes are applied there.

## Reporting a vulnerability

Please do **not** open a public issue for a security problem. Report it privately through GitHub's
private vulnerability reporting: open the repository's **Security** tab and choose **Report a vulnerability**.
Include steps to reproduce and the affected component (`app/`, `vlayer/`, etc.).

## Dependency maintenance

Dependencies are audited periodically with `npm audit`. Patched versions are applied within the current major
version where possible, and anything that cannot be fixed is documented. See the dated records in [`docs/`](docs/),
for example [`docs/SECURITY_UPDATE_2026-10-21.md`](docs/SECURITY_UPDATE_2026-10-21.md).
