# Changelog

All notable changes are recorded here. One release number covers the image tag, the git tag and the prompt
bundle (`prompts/pin.json` `bundle_version`); the prompt and MCP server versions it ships are listed per release.
Versions follow [Semantic Versioning](https://semver.org/).

## [Unreleased]

## [1.2.0] - 2026-10-07

Ships: prompt **1.2.0** (active, `da2a2183ac50`), MCP **afyaplus-logistics 1.0.0**, model
`gpt-4o-mini-2024-07-18` (model_version 2026.10.0). First release built and gated by the Week 8 pipeline.

### Added
- Versioned prompt bundle: `prompts/triage_system_v1.2.0.txt` (the Week 6/7 production prompt, previously
  `triage-v1`, put under semver), `prompts/pin.json` SHA-256 lockfile, `config/triage.yaml` validated at startup.
- `/health` reports release, image tag, git SHA, prompt versions and SHA-256, config SHA, model, model_version and
  MCP version; `/ready` runs the MCP probe; `X-Prompt-Version` / `X-Prompt-SHA` on every triage response.
- Golden-set regression gate (`eval_prompts.py`, 75 cases incl. a 15-case holdout) over live recordings keyed by
  prompt SHA; fails closed on missing or stale recordings.
- Logistics MCP server 1.0.0 (Week 6 clinic server) reporting its own version in the handshake; tool schemas locked
  in `mcp_schema.lock.json`; `scripts/check_mcp_health.py` probe and a contract stub with the same exit codes.
- Canary flag (`ab_prompt_flag.py`), shared counters (`tool_metrics.py`), canary gate, dashboard sketch.
- CI: GitHub Actions `lint-test -> eval + mcp-health -> build-deploy`, Azure DevOps twin, local runner,
  CODEOWNERS, PR template with the Definition of Done; retrain stub workflow.

### Known issues
- The first gate run scored prompt 1.2.0 at **0.747** urgency agreement and **0.79** emergency recall (threshold
  0.85 / 1.0): it under-triages URGENT cases as ROUTINE. Shipped under a clinical_ops waiver expiring
  2026-11-30 while a fix is canaried. Code-level danger-sign rules remain on.
- Keyword safety rules match "fits" in "no fits so far" (holdout H04): a known over-triage false positive.

[Unreleased]: compare v1.2.0...HEAD
[1.2.0]: first gated release
