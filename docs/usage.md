# Ω-SCAN Usage Guide

## Overview

Ω-SCAN is a **framework legitimacy scanner**. It evaluates whether a governance framework is legitimate enough to govern anything at all, by enforcing five non-negotiable audits.

This is a **prompt-based tool** (v0.1). You provide a framework document to an LLM along with Ω-SCAN's system prompt and schema, and the LLM returns a structured verdict.

---

## Quick Start

### Prerequisites
- Access to an LLM that supports:
  - System prompts
  - JSON schema-constrained output
  - Reasoning over long text
- The framework document you want to scan

### Files You'll Need
1. `prompts/omega-scan.system.txt` - The scanner's instructions
2. `schema/omega-scan-v0.1.schema.json` - Output format specification
3. Your framework document (markdown, text, or any readable format)

---

## Step-by-Step Scanning Process

### Step 1: Prepare Your Framework Text

Save the governance framework you want to scan as plain text. Examples:
- A deployment policy
- A risk management framework
- An AI safety protocol
- Any document that prescribes governance, rules, or decision procedures

**Tip:** Use complete documents when possible. Excerpts may lack context needed for accurate audit results.

---

### Step 2: Set Up the System Prompt

Copy the contents of `prompts/omega-scan.system.txt` into your LLM's **system message** field.

This prompt:
- Defines the five audits (A1-A5)
- Sets global rules (reject-by-default, no guessing, evidence-based)
- Specifies output requirements

**Do not modify the system prompt** unless you're intentionally changing audit criteria.

---

### Step 3: Construct the User Prompt

Use `prompts/omega-scan.user-template.txt` as your template:

```
Return JSON that matches the following JSON Schema exactly:
<<<
[PASTE CONTENTS OF schema/omega-scan-v0.1.schema.json HERE]
>>>

Framework text:
<<<
[PASTE YOUR FRAMEWORK DOCUMENT HERE]
>>>
```

**Example:**
```
Return JSON that matches the following JSON Schema exactly:
<<<
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://unai.example/schemas/omega-scan-v0.1.schema.json",
  "title": "OmegaScanResult",
  ...
}
>>>

Framework text:
<<<
# My Deployment Policy

Section 1: We will deploy when risks are acceptable...
>>>
```

---

### Step 4: Run the Scan

Send both prompts to your LLM:
- **System:** Contents of `omega-scan.system.txt`
- **User:** Formatted template with schema + framework text

The LLM will analyze your framework against all five audits and return structured JSON.

---

### Step 5: Interpret the Results

The response will be valid JSON matching the schema. Key fields:

#### `verdict`
- **PASS**: Framework satisfies all five audits and may be instantiated (with appropriate covenant)
- **REJECT**: Framework has critical failures and must not be used to govern

#### `audits`
Shows per-audit status:
```json
"audits": {
  "A1_definition_lock": { "status": "PASS", "rationale": "..." },
  "A2_authority_declaration": { "status": "FAIL", "rationale": "..." },
  ...
}
```

#### `violations`
Lists concrete failures with evidence:
```json
"violations": [
  {
    "code": "AUTH-IMPLICIT",
    "severity": "CRITICAL",
    "audit": "A2",
    "message": "Authority is exercised by internal committees without revocation...",
    "evidence": {
      "quotes": ["A Safety Advisory Group reviews..."],
      "locations": ["section 3, line 12"]
    },
    "suggested_fix": "Add explicit legitimacy source..."
  }
]
```

#### `extracted`
Shows what the scanner found:
- **normative_terms**: Elastic terms and whether they're defined
- **authorities**: Who has power and what constraints exist
- **irreversible_actions**: Actions that can't be undone
- **amendment_clauses**: How the framework can change itself
- **external_dependencies**: What the framework relies on but doesn't define

#### `minimal_remediation`
Smallest set of changes needed to reach PASS:
```json
"minimal_remediation": [
  "Add explicit authority declaration with revocation and conflict rules.",
  "Enumerate governance failure modes.",
  "Route amendments to an external covenant."
]
```

---

## What to Do With Results

### If VERDICT = PASS

1. **Review extracted dependencies** - Ensure all external dependencies actually exist
2. **Verify authority chain** - Confirm the declared authority holders are legitimate
3. **Implement with covenant** - Connect framework to an Ω-Layer covenant (sovereign human root authority)
4. **Monitor for drift** - Periodically re-scan as framework evolves

**PASS does not mean "perfect"** - it means "legitimate enough to govern with appropriate oversight."

---

### If VERDICT = REJECT

1. **Read violations** - Understand which audits failed and why
2. **Review minimal_remediation** - These are the minimum fixes required
3. **Choose a path:**
   - **Fix:** Remediate violations and re-scan
   - **Reject:** Don't use this framework to govern anything
   - **Escalate:** Flag as illegitimate governance attempt

**Do not deploy or use REJECT frameworks.** They contain authority laundering, undefined terms, or recursive containment failures.

---

## Example Workflow

### Scanning the Incident Freeze Protocol (Passing Example)

**Setup:**
```bash
# 1. Copy system prompt
cat prompts/omega-scan.system.txt

# 2. Copy schema
cat schema/omega-scan-v0.1.schema.json

# 3. Copy framework
cat fixtures/passing/incident_freeze_protocol.md
```

**LLM Configuration:**
- System: [system prompt contents]
- User: [template with schema + framework]
- Temperature: 0 (for consistency)
- JSON mode: Enabled

**Result:**
```json
{
  "verdict": "PASS",
  "audits": { ... all PASS ... },
  "violations": [],
  "minimal_remediation": []
}
```

**Interpretation:**
- ✓ All five audits satisfied
- ✓ No violations detected
- ✓ Framework is legitimate (with appropriate covenant)
- → Safe to instantiate

---

### Scanning OpenAI Preparedness Excerpt (Failing Example)

Same process, but with `fixtures/openai_preparedness_v2.txt`

**Result:**
```json
{
  "verdict": "REJECT",
  "audits": {
    "A1_definition_lock": { "status": "PASS", ... },
    "A2_authority_declaration": { "status": "FAIL", ... },
    "A3_irreversibility_control": { "status": "FAIL", ... },
    "A4_failure_enumeration": { "status": "FAIL", ... },
    "A5_recursive_containment": { "status": "FAIL", ... }
  },
  "violations": [
    {
      "code": "AUTH-IMPLICIT",
      "severity": "CRITICAL",
      "audit": "A2",
      ...
    }
  ],
  "minimal_remediation": [
    "Add explicit authority declaration with revocation and conflict rules.",
    "Enumerate governance failure modes.",
    "Route amendments to an external covenant."
  ]
}
```

**Interpretation:**
- ✗ 4 of 5 audits failed
- ✗ CRITICAL violation: implicit authority
- → Framework must not be used to govern
- → Requires remediation before re-scan

---

## Tips & Best Practices

### For Accurate Scans

1. **Use complete documents** - Partial excerpts may appear worse than they are
2. **Include referenced appendices** - If framework says "see Appendix B for definitions," include it
3. **Don't editorialize** - Provide the framework text as-is; don't add explanatory notes
4. **Run multiple times** - LLM outputs can vary; check consistency across runs

### For Framework Authors

1. **Scan early** - Don't wait until deployment to discover legitimacy failures
2. **Fix violations before features** - A REJECT framework with great features is still illegitimate
3. **Make definitions explicit** - "Appropriate", "reasonable", "safe" need operational definitions
4. **Declare authority upfront** - Who decides, why, and what happens if they're wrong
5. **Enumerate failures** - Don't just plan for success; plan for governance failure modes

### Common Pitfalls

**"It's voluntary, so authority doesn't matter"**
→ Wrong. If it governs a decision, it needs explicit authority semantics.

**"We'll define that later"**
→ REJECT. Missing definitions cause immediate failure.

**"The committee has discretion"**
→ Authority laundering. Who legitimizes the committee? What if they disagree?

**"We update as needed"**
→ Recursive containment violation. Route amendments to external covenant.

---

## Advanced Usage

### Batch Scanning

To scan multiple frameworks:
1. Create a script that formats each framework with the template
2. Send to LLM API with consistent parameters
3. Collect JSON results
4. Compare across frameworks

### Custom Audits

Ω-SCAN v0.1 ships with five fixed audits. To add custom checks:
1. Fork the system prompt
2. Add your audit to the checklist
3. Update the schema to include your new audit
4. Document divergence from canonical Ω-SCAN

**Warning:** Custom audits create incompatible scan results.

### Integration with CI/CD

Add Ω-SCAN to governance pipelines:
```bash
# Pseudo-code
if scan_framework(policy.md).verdict == "REJECT":
    block_deployment()
    notify_governance_team()
```

---

## Troubleshooting

**LLM returns prose instead of JSON**
→ Ensure JSON mode is enabled or use an LLM that supports schema-constrained output

**Output doesn't match schema**
→ Verify you pasted the complete schema; some LLMs truncate long system prompts

**All frameworks REJECT**
→ This is expected. Most real-world frameworks fail Ω-SCAN audits.

**Scan results inconsistent across runs**
→ Lower temperature (use 0 for deterministic results)

**Framework seems legitimate but still REJECT**
→ Review violations; legitimacy ≠ well-intentioned. Ω-SCAN enforces structural requirements.

---

## What Ω-SCAN Is NOT

- **Not a policy generator** - It scans, doesn't write
- **Not an ethics judge** - It checks structure, not morality
- **Not a model evaluator** - It checks governance, not AI capabilities
- **Not optional** - If you're governing, you need legitimacy

---

## Next Steps

- See `fixtures/` for examples of PASS and REJECT frameworks
- See `reports/` for detailed scan outputs
- See `docs/audit-spec.md` for audit definitions
- See `docs/glossary.md` for terminology

Questions or found a legitimacy hole in Ω-SCAN itself? File an issue.

---

**Remember:** Ω-SCAN enforces *necessary* conditions for legitimacy, not *sufficient* ones. A PASS framework still requires a sovereign covenant and ongoing oversight.
