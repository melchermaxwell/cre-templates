# Sports Resolution Follow-Ups

This tracks follow-up hardening items that are not fully addressed by the currently open review PRs for the Sports Resolution PoC.

Open review PRs already cover final score validation, settled-game guard reads, API key guidance alignment, post-init install instructions, fetch wording, index registration, Foundry path docs, EVM config validation, and basic fetch parser tests. This file intentionally does not duplicate those PRs.

## 1. Add True Multi-Provider Source Diversity

The current PoC config uses the ESPN public endpoint twice to demonstrate the 2-source aggregation shape. That is acceptable for a PoC, but it is not independent provider consensus.

Follow-up options:
- Add provider-specific adapters for at least one additional sports data provider.
- Keep ESPN as the runnable public default, but document the duplicate-source tradeoff explicitly.
- Add a config structure that supports per-provider URL patterns and per-provider game IDs.

## 2. Decide Whether Workflow Identity Should Be Enforced

The receiver supports workflow identity checks through `setExpectedAuthor`, `setExpectedWorkflowId`, and `setExpectedWorkflowName`. The current review PRs document this hardening path, but they do not enforce it in deployment scripts or contract defaults.

Follow-up options:
- Add a deployment helper or post-deployment instructions that resolve and set the expected workflow identity.
- Add contract tests for wrong workflow metadata, wrong author, wrong workflow ID, and Forwarder-only behavior.
- Decide whether Forwarder-only validation is sufficient for this starter template's PoC scope.

## 3. Add a Private Sports API Secrets Example

The default ESPN flow is public and does not need CRE Secrets. A separate private-provider example would still help users adapt the template safely.

Follow-up options:
- Add an optional example using CRE Secrets for an API key header.
- Add a sample `secrets.yaml` and `workflow.yaml` configuration for private API providers.
- Keep the default config secret-free and isolate private provider setup in a dedicated README section or example file.

## 4. Evaluate Parallel Source Execution

The current workflow fetches sources sequentially. The wording PR removes the parallel claim, but parallel/fan-out execution may still be desirable if CRE supports a clean pattern for this template.

Follow-up options:
- Confirm the recommended CRE TypeScript pattern for parallel HTTP capability calls.
- Implement parallel source fetches if the SDK pattern is stable and readable for a starter template.
- Otherwise keep the sequential implementation and document the tradeoff.

## 5. Add Contract and End-to-End Simulation Coverage

The open test PRs improve parser/config/guard coverage, but deeper contract and simulation coverage remains useful before upstreaming.

Follow-up options:
- Add contract tests for unauthorized sender, wrong workflow metadata, unsolicited reports, invalid outcomes, and duplicate settlement.
- Add a simulation fixture using a known ESPN event ID and expected final score.
- Add a no-consensus workflow test that verifies no onchain write is attempted.
