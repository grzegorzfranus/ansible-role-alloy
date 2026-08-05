# Task Spec: Remediation of ansible-role-alloy

- **Ticket/Issue**: #2
- **Branch**: 2-feat-initial-release-of-ansible-role-alloy
- **Workspace**: /Users/grzegorzfranus/Work/ansible/roles/ansible-role-alloy

## Goal & Business Context

Remediate review findings for `ansible-role-alloy` to align with canonical Ansible role catalogue conventions (reference: `ansible-role-chrony`) and complete variable documentation & validation.

## Architectural Assumptions & Rules

1. NO `alloy_role_action` lifecycle selector.
2. NO logrotate configuration.
3. NO second Molecule scenario.
4. Mandatory tenant ID assertion (`alloy_tenant_id`) and Jinja2 headers must be preserved.
5. All 17 `alloy_*` variables must be documented and validated across all 4 surfaces (`defaults/main.yml`, `meta/argument_specs.yml`, `tasks/assert.yml`, `README.md`).

## Definition of Done (DoD)

- [ ] F10: `alloy_apt_repo`, `alloy_repo_gpg_key`, `alloy_yum_repo_baseurl` added to `argument_specs.yml` and `README.md`; non-empty and `https://` asserted in `assert.yml`.
- [ ] F10: Booleans and mapping type-asserted in `assert.yml`.
- [ ] F1: `vars/Debian.yml` & `vars/RedHat.yml` renamed to lowercase via `git mv`; `| lower` in lookup.
- [ ] F2: OS vars cascade deepened to 4-level/7-item pattern matching chrony; stale `default.yml` reference resolved.
- [ ] F11: Comment density in `tasks/` reduced by removing redundant subsection dividers while preserving headers & justification notes.
- [ ] F7: Config validation folded into `configure.yml` or clean justification provided if unsupported offline.
- [ ] F3: README section set and ordering aligned with chrony.
- [ ] F8 & F9: File Structure tree regenerated from `find`; flow comment in `tasks/main.yml` updated.
- [ ] Validation: `yamllint .`, `ansible-lint --profile production .`, `molecule test` all green.
- [ ] PR updated / open, not merged.
