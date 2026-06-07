# Contribution 1: Add v1 finding fixtures for gcp-inspector

**Contribution Number:** 1  
**Student:** Chan Nyein Zin (Noel)  
**Issue:** https://github.com/GRCEngClub/claude-grc-engineering/issues/161  
**Status:** Phase I Complete

---

## Why I Chose This Issue

I chose this issue because it's hands-on cloud-security work with a crisp, mechanical definition of done, and it maps directly to my goal of becoming a cloud/security engineer. The `gcp-inspector` connector is missing its sample Finding fixtures, so the task is to create three JSON fixtures — a pass, a fail with severity and remediation, and an inconclusive — that model real GCP security findings (uniform bucket-level access, IAM least-privilege, API quota limits) and validate against the project's v1 Finding schema.

This fits my skills and learning goals well: I'm strongest in Python and comfortable with structured data and schemas, the scope is tight (~2-3 hours), and the acceptance check is objective — CI runs `tests/validate-contract-fixtures.sh`, so it either passes or it doesn't. I want to learn the `schemas/finding.schema.json` v1 contract directly, since it's the data backbone of the whole GRC toolkit, and to ground myself in how a real connector reports security evaluations against a control framework.

---

## Understanding the Issue

### Problem Description

The `gcp-inspector` connector has no sample Finding fixtures. Every other connector ships example Finding documents under `tests/fixtures/findings/<connector>/` that exercise the contract test, but `gcp-inspector` (and `okta-inspector`, filed separately) is missing them.

### Expected Behavior

`tests/fixtures/findings/gcp-inspector/` should contain at least 3 valid Finding fixtures covering the status mix the connector produces (pass, fail, inconclusive), and `bash tests/validate-contract-fixtures.sh` should report all fixtures valid.

### Current Behavior

No fixtures exist for `gcp-inspector`, so the connector's output format isn't exercised by the contract test.

### Affected Components

- `tests/fixtures/findings/gcp-inspector/` (new directory + files)
- `schemas/finding.schema.json` (the v1 contract the fixtures must satisfy)
- `plugins/connectors/gcp-inspector/scripts/` and `commands/collect.md` (reference for realistic resource types / SCF controls)
- `tests/fixtures/findings/aws-inspector/001-s3-unencrypted.json` (format reference)
- `tests/validate-contract-fixtures.sh` (the acceptance check)

---

## Reproduction Process

_To be completed in Phase II._

### Environment Setup

Plan: fork + clone, then run `npm install --no-save ajv-cli@5 ajv-formats@3` to install the schema validator.

### Steps to Reproduce

1. Clone the repo.
2. Run `bash tests/validate-contract-fixtures.sh`.
3. Confirm `gcp-inspector` has no fixtures listed (the gap this issue fills).

### Reproduction Evidence

- **Commit showing reproduction:** _TBD_
- **Screenshots/logs:** _TBD_
- **My findings:** _TBD_

---

## Solution Approach

### Analysis

This is a missing-artifact task, not a bug. The connector emits Findings but ships no examples to validate the format, so the fix is to author fixtures that conform to the v1 schema and represent the real status mix.

### Proposed Solution

Create 3 JSON fixtures under `tests/fixtures/findings/gcp-inspector/`, modeled on the existing `aws-inspector` fixture, each conforming to `schemas/finding.schema.json`.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** Add sample GCP Finding documents so the contract test exercises `gcp-inspector` output.

**Match:** Mirror the structure of `tests/fixtures/findings/aws-inspector/001-s3-unencrypted.json` and the Finding contract in `schemas/finding.schema.json`.

**Plan:**
1. Read the schema and the aws-inspector fixture for required fields.
2. Read the gcp-inspector scripts/`collect.md` for real resource types and SCF control IDs.
3. Create `001-bucket-uniform-access-pass.json` (pass).
4. Create `002-service-account-owner-role-fail.json` (fail, with severity + remediation).
5. Create `003-api-quota-inconclusive.json` (inconclusive).

**Implement:** _Links to branch/commits TBD._

**Review:** Self-check against CONTRIBUTING.md and acceptance criteria (filename pattern, required fields, gcp_* resource type, self-link uri, one remediation block).

**Evaluate:** Run `bash tests/validate-contract-fixtures.sh` and confirm `All N fixture(s) valid.`

---

## Testing Strategy

### Unit Tests

- [ ] `001-bucket-uniform-access-pass.json` validates (pass status)
- [ ] `002-service-account-owner-role-fail.json` validates (fail status, has remediation block)
- [ ] `003-api-quota-inconclusive.json` validates (inconclusive status)

### Integration Tests

- [ ] `bash tests/validate-contract-fixtures.sh` passes with all 3 new fixtures listed

### Manual Testing

_To be completed in Phase II/III._

---

## Implementation Notes

_To be completed as I build._

### Code Changes

- **Files modified:** _TBD_
- **Key commits:** _TBD_
- **Approach decisions:** _TBD_

---

## Pull Request

**PR Link:** _TBD_

**PR Description:** _TBD_

**Maintainer Feedback:**
- _TBD_

**Status:** Not yet submitted

---

## Learnings & Reflections

_To be completed at the end of the contribution._

---

## Resources Used

- Finding schema: `schemas/finding.schema.json`
- Format reference: `tests/fixtures/findings/aws-inspector/001-s3-unencrypted.json`
- Connector reference: `plugins/connectors/gcp-inspector/`
- Repo contribution guide: `docs/CONTRIBUTING.md`
