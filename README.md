# Apple CI

Shared, secret-free signing and release automation maintained in
`oh-my-infra/apple-ci` for Apple apps owned by `oh-my-brew` and `oh-my-app`.

This public repository contains:

- reusable GitHub Actions workflows for Developer ID and TestFlight releases;
- shared Fastlane lanes imported by each app repository;
- an idempotent App Store Connect Bundle ID/capability helper.

The explicit `$apple-release-onboarding` Codex Skill is maintained in the
private `assassinor/github` workspace repository under `.agents/skills`; this
public automation repository remains secret-free and contains the human guides
that the Skill follows.

Signing assets are stored separately in the private
`assassinor/apple-signing` Match repository. Never commit certificates,
provisioning profiles, API keys, passwords, or deploy keys here.

Start with [the onboarding guide](docs/onboarding.md). Credential bootstrap and
rotation are documented in [the credential runbook](docs/credentials.md).

## Versioning

Releases use immutable, signed `vYYYY.MM.DD.N` tags. `N` starts at `1` for each
date and is never zero-padded. Historical `v1.0.x` tags remain valid and are not
renamed or moved. App callers pin the shared Fastfile to the calendar tag and
the reusable workflow to its corresponding full commit SHA.

For tag-triggered publishing, the shared lanes query GitHub's tag object API and
require a valid cryptographic signature before accessing Match or Apple
credentials. Annotated but unsigned tags are rejected.

Current release: `v2026.09.12.1`.

## Namespace migration

Consumers use `oh-my-infra/apple-ci` for both the Fastfile import URL and the
reusable workflow reference. Pin the import to the current signed calendar tag
and the workflow to its corresponding full commit SHA; GitHub Actions does not
redirect the former `assassinor/apple-ci` workflow references. Internal Actions
are pinned to the full commits resolved from their existing major tags.

Homebrew-distributed apps belong to `oh-my-brew`, with the distribution tap at
`oh-my-brew/tap`. Shared source-release automation lives separately in
`oh-my-infra/brew-ci`. This namespace migration preserves app Bundle IDs,
signing identities, and the private `assassinor/apple-signing` repository.
