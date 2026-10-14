# Security of the release channel

Every installed copy of Samay updates itself from this repository's
Releases, so whoever can publish here can run code on every employee's
laptop (security review, 14 Oct 2026, findings H3 and L4). Releases are
built and published only by the `Samay desktop release` workflow in the
private `rcs3` repo; nobody uploads installers by hand.

## How an installed app checks an update

- **Windows:** the installer must be signed by the publisher named in the
  app (`SAMAY_PUBLISHER_NAME`); electron-updater refuses anything else.
- **macOS (Developer ID builds):** Squirrel.Mac installs only an update
  signed by the same Apple team.
- **macOS (older ad hoc builds):** the downloaded DMG must match the SHA-512
  in the release's `latest-mac.yml`, and, once the app has a Developer ID,
  the app inside must be signed by the same team, before Samay opens it.

The build refuses to publish unsigned installers unless the
`SAMAY_ALLOW_UNSIGNED` repository variable is `true`.

## Keeping this repository safe

- Two-factor authentication on every GitHub account with access, ideally a
  passkey or security key; as few collaborators as possible (ideally none:
  the workflow's token publishes).
- `SAMAY_PUBLISH_TOKEN`: a fine-grained token for this repository only,
  Contents read/write, with an expiry date; rotate it when it expires or
  anyone with access leaves. Keep it as an environment secret of the
  `samay-release` environment in `rcs3`, which needs a reviewer's approval.
- Settings → Rules: protect tags (`v*`) and the default branch so releases
  and history can't be rewritten; turn on "Immutable releases" if offered.
- Watch this repository's releases: a release you didn't expect is an
  incident. Delete it, revoke the token, and follow the incident steps in
  `rcs3/SECURITY.md`.

## Reporting a problem

Email the Renocrew administrators rather than opening a public issue.
