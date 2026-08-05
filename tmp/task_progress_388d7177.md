# Task Progress: Remediation of ansible-role-alloy

## Current Status

Starting step 1 (F10 - variable coverage).

## Checklist

- [ ] 1. F10 — Variable coverage (argument_specs, README, assert.yml)
- [ ] 2. F1 — Vars filename casing (`git mv`, `| lower`)
- [ ] 3. F2 — OS vars cascade (4-level, 7-item)
- [ ] 4. F11 — Comment density in `tasks/`
- [ ] 5. F7 — Config validation in `configure.yml`
- [ ] 6. F3 — README structure alignment with chrony
- [ ] 7. F8 + F9 — File Structure tree regeneration & main.yml flow comment update
- [ ] 8. Pre-push quality gate validation (yamllint, ansible-lint, molecule test)
- [ ] 9. Git commit, push, PR update

## Environment & Commands Run

- `env -u GITHUB_TOKEN gh auth status`
- `env -u GITHUB_TOKEN gh auth switch --user grzegorzfranus`
- `git status`
- `view_pr.sh` (PR #3 open)

## Open Questions & Blockers

- None
