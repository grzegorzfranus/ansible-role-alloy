# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
