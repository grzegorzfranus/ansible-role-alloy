# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.5](https://github.com/grzegorzfranus/ansible-role-alloy/compare/v1.0.4...v1.0.5) (2026-08-07)


### Bug Fixes

* align role tags, handlers and documentation with standards ([#23](https://github.com/grzegorzfranus/ansible-role-alloy/issues/23)) ([58234fe](https://github.com/grzegorzfranus/ansible-role-alloy/commit/58234fed737547e8f2ada4835802289ea81812cd))

## [1.0.4](https://github.com/grzegorzfranus/ansible-role-alloy/compare/v1.0.3...v1.0.4) (2026-08-07)


### Code Refactoring

* **debian:** replace deprecated apt_repository with deb822_repository ([#18](https://github.com/grzegorzfranus/ansible-role-alloy/issues/18)) ([2388fb5](https://github.com/grzegorzfranus/ansible-role-alloy/commit/2388fb54bd52bbffbe45d74ee3a89cf1ad69178d))

## [1.0.3](https://github.com/grzegorzfranus/ansible-role-alloy/compare/v1.0.2...v1.0.3) (2026-08-05)


### Documentation

* complete file structure tree and CI/CD section in README ([#13](https://github.com/grzegorzfranus/ansible-role-alloy/issues/13)) ([262c7f8](https://github.com/grzegorzfranus/ansible-role-alloy/commit/262c7f84dbb6e9d8c9235d8de82206cf0d004c2f))
* replace internal hostnames and IP addresses with generic example values ([#16](https://github.com/grzegorzfranus/ansible-role-alloy/issues/16)) ([9044a4e](https://github.com/grzegorzfranus/ansible-role-alloy/commit/9044a4ef97cd3f48deedc092a546cc1076dac141))

## [1.0.2](https://github.com/grzegorzfranus/ansible-role-alloy/compare/v1.0.1...v1.0.2) (2026-08-05)


### Miscellaneous

* update role license to Apache-2.0 ([#10](https://github.com/grzegorzfranus/ansible-role-alloy/issues/10)) ([f048712](https://github.com/grzegorzfranus/ansible-role-alloy/commit/f048712a1330518c7935b81694f4b34baf5632e4))

## [1.0.1](https://github.com/grzegorzfranus/ansible-role-alloy/compare/v1.0.0...v1.0.1) (2026-08-05)


### Bug Fixes

* align alloy role with catalogue conventions and complete variable documentation ([#4](https://github.com/grzegorzfranus/ansible-role-alloy/issues/4)) ([86e58ee](https://github.com/grzegorzfranus/ansible-role-alloy/commit/86e58ee5114c3cf2ab8568a2c654cdf8e1a0dc23))

## [1.0.0] - 2026-08-05

### Features

- Initial release of `grzegorzfranus.alloy` (`ansible-role-alloy`).
- Native system package deployment (`apt`/`yum`) managed by systemd.
- Log collection support for systemd journald and Docker containers.
- Enforced mandatory `alloy_tenant_id` (`X-Scope-OrgID` header) preflight assertions.
- Deployed systemd unit security sandboxing override (`override.conf`).
