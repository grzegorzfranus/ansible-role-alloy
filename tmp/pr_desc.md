Closes #7

## 1. ✨ What this PR does

- Updates role license in `meta/main.yml` from `MIT` to `Apache-2.0`.

## 2. 🔍 Why

- Aligns `ansible-role-alloy` with reference roles in the catalogue (`chrony`, `loki`, `docker`) which all use `Apache-2.0`.
- Provides explicit patent grant protections and trademark reservations consistent with Red Hat CoP standards for infrastructure automation.

## 3. 💡 Value

- Guarantees 100% license consistency across all Comlia Ansible roles.
- Adopted industry standard for enterprise IaC modules.

## 4. ✅ Local Verification

- `yamllint .` (passed cleanly)
- `ansible-lint --profile production .` (passed with 0 failures, 0 warnings)
- `molecule test` (passed cleanly with 100% success across all scenario stages)
