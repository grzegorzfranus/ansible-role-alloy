# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-08-05

### Features

- Initial release of `grzegorzfranus.alloy` (`ansible-role-alloy`).
- Native system package deployment (`apt`/`yum`) managed by systemd.
- Log collection support for systemd journald and Docker containers.
- Enforced mandatory `alloy_tenant_id` (`X-Scope-OrgID` header) preflight assertions.
- Deployed systemd unit security sandboxing override (`override.conf`).
