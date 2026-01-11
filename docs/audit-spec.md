# Ω-SCAN Audit Spec (v0.1)

Ω-SCAN is a *framework legitimacy* checker.

It enforces five audits. A framework may not be instantiated unless all pass.

## A1 — Definition Lock
Fail if any core normative/elastic term is used without:
- measurable criteria, OR
- a bounded taxonomy with thresholds, OR
- an explicit decision procedure.

Examples of elastic terms: safe, effective, appropriate, reasonable, trusted, responsible, robust, unacceptable, high-risk.

## A2 — Authority Declaration
Fail if the framework does not explicitly specify:
- who decides / enforces (authority holders),
- why they are legitimate (source of authority),
- revocation or rotation,
- conflict rule (what happens when authorities disagree).

## A3 — Irreversibility Control
Fail if the framework prescribes irreversible actions without:
- explicit identification of irreversible actions,
- an authorization checkpoint (human meaning authority or legitimate body),
- rollback conditions OR freeze behavior.

## A4 — Failure Enumeration
Fail if the framework does not enumerate governance failures:
- authority laundering,
- scope creep,
- adversarial misuse of the governance process,
- metric gaming,
- "paper compliance / real-world failure" gaps.

Minimum: 3 governance failure modes.

## A5 — Recursive Containment
Fail if the framework can update itself without external covenant constraints.
Amendments must route upward to a higher authority layer.

## Output Contract
Ω-SCAN outputs strict JSON matching schema/omega-scan-v0.1.schema.json.
