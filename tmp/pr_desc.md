Closes #2

## 1. ✨ What this PR does

- Removes `alloy_become` variable and replaces all 22 task-level and handler-level `become: "{{ alloy_become }}"` declarations with literal `become: true`.
- Removes `alloy_become` from `defaults/main.yml`, `meta/argument_specs.yml`, `tasks/assert.yml`, and `README.md`.
- Updates `molecule/default/converge.yml` by removing `alloy_become: false`, aligning Molecule test execution with role catalogue standards.
- Updates public variable count in `README.md` from 17 to 16, maintaining 100% 4-surface coverage.

## 2. 🔍 Why

- The `alloy_become` variable was non-functional as `false` because system package installation, repository configuration, GPG keyring creation, systemd hardening overrides, and service management all require root privileges.
- Exposing privilege escalation as a user variable provided false configurability and obscured privilege surface audits (`grep "become: true"`).
- Aligns `ansible-role-alloy` with reference role `chrony` and standard catalogue conventions.

## 3. 💡 Value

- Simplifies role interface by eliminating an unviable variable knob.
- Restores auditability of privilege escalation via literal `become: true` statements.
- Keeps Molecule container testing green without needing auxiliary privilege knobs.

## 4. ✅ Local Verification

- `yamllint .` (passed with 0 errors)
- `ansible-lint --profile production .` (passed with 0 failures, 0 warnings)
- `molecule test` (passed cleanly with 100% success across all scenario stages)
