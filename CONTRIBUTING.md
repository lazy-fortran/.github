# Contributing

Follow the [group contribution guidance](https://github.com/itpplasma/.github/blob/main/CONTRIBUTING.md)
for fixture provenance, tracked `.repo-hygiene-allow` reasons and squash/stack
handling. The shared checker and reusable workflow live in
[itpplasma/.github](https://github.com/itpplasma/.github).

Private repositories use the global repository hygiene hook. Run their
validation on clusters and record job IDs, revisions, commands and results
in PRs; do not add an Actions-based hygiene check to a private repository.
