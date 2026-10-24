# Apple CI maintenance

## Read first

- Read `README.md`, `docs/onboarding.md`, `docs/credentials.md`, and the workflow or Fastlane lane being changed. Check Git status and preserve concurrent edits; do not reset, clean, stash, force-push, or switch branches without instruction.
- This public repository owns reusable Apple signing/release workflows, shared Fastlane lanes, and the Bundle ID/capability helper. App behavior, app-side triggers and release approval belong to the caller repository.

## Contracts and credentials

- Keep caller inputs, outputs, explicit secret mappings, upload/publish controls, tag validation, and readonly Match defaults compatible. Document intentional contract changes and the caller migration required.
- Preserve immutable signed calendar tags and full-SHA workflow pins. Do not move tags, replace existing releases, weaken signature checks, or update downstream callers as an incidental change.
- Never commit or print signing assets, secret values, production configuration, or recovery copies. Match assets belong in the private signing repository; secret handling and rotation follow `docs/credentials.md`.
- Match writes, credential creation/revocation, capability changes with `--apply`, TestFlight uploads, notarization and publication require authorization for that specific operation. A documentation or workflow edit does not authorize them.

## Validation and handoff

- For relevant code changes use the checks in `.github/workflows/ci.yml`: Ruby syntax and unit tests, Fastlane lane discovery, actionlint, shellcheck, and `scripts/check-secrets.sh`. Inspect their prerequisites before installing tools; do not install or upgrade dependencies merely to edit documentation.
- Keep tests synthetic. A non-publishing workflow can still access Apple credentials or services; do not dispatch one as an automatic documentation check.
- If checks are explicitly skipped, report that fact. Passing static checks is not a signing, notarization or upload receipt.
- Update the onboarding or credential guide when its procedure changes, and the README when entry points change. Handoff includes the commit, checks actually run, compatibility impact, and any authorized rollout still outstanding.
