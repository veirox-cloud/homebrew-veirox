# homebrew-veirox — Homebrew tap

> **DO NOT hand-edit the Formula files.** They are auto-pushed by GoReleaser
> on every CLI release tag from `veirox-cloud/veirox-cli`.

## Repo purpose

Homebrew tap for the [Veirox CLI](https://veirox.com/cli.html).

## Why this repo is public

Homebrew taps must be public — `brew tap` cannot read private repos
without auth. This is one of only two intentionally-public repos in
the Veirox org (the other is `veirox-scoop`). The CLI source
itself stays private at `veirox-cloud/veirox-cli`.

## How releases work

1. A maintainer cuts a tag in the `veirox-cli` repo: `git tag v0.5.0 && git push --tags`.
2. The `veirox-cli` GitHub Actions workflow runs `goreleaser release`.
3. GoReleaser builds the binaries, signs them with cosign, and pushes the
   updated formula directly to **this repo** via the `HOMEBREW_TAP_TOKEN`
   secret (a fine-grained PAT scoped to this repo only).
4. The next `brew install veirox-cloud/veirox/veirox` on any machine
   picks up the new version.

See [`veirox-cli/.goreleaser.yaml`](https://github.com/veirox-cloud/veirox-cli/blob/main/.goreleaser.yaml)
for the full release pipeline config (the `brews:` block).

## What lives here

- `Formula/veirox.rb` — the auto-generated Homebrew formula (ONE file).
- `LICENSE` — Apache 2.0.
- `NOTICE` — boilerplate.
- `README.md` — install instructions.

That's it. The whole repo is intentionally tiny and immutable from
outside the GoReleaser pipeline.

## If something breaks

If `brew install veirox-cloud/veirox/veirox` fails, the diagnostic
order is:

1. Did GoReleaser actually push the formula? Check this repo's commit
   log for a recent commit by `goreleaserbot`.
2. Did the CLI release actually upload binaries to GitHub Releases?
   Check `veirox-cli` releases page.
3. Are the SHA-256 checksums in `Formula/veirox.rb` matching the
   uploaded archives? GoReleaser handles this, but a partial release
   can leave inconsistency. Re-run the `cli` release workflow.

## Rebrand-readiness

If we ever rebrand, **this repo's name cannot follow the `<brand>-`
prefix convention** because Homebrew protocol mandates the
`homebrew-` prefix. The new equivalent would be
`homebrew-<newbrand>` and we'd cross-deprecate per the rebrand
runbook (`veirox-backend/docs/runbooks/rebrand-runbook.md`).

## License

Apache 2.0 — see `LICENSE`.
