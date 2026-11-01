# Multi-Agent Triage Engine

The Multi-Agent AI Triage Engine implements autonomous security reasoning over normalized, redacted vulnerability findings.

## Architecture

```
                       Orchestrator (orchestrator.ts)
                                   │
                                   ▼
                       1. Enrichment Agent (enrichment.ts)
                          - Calls NVD REST API v2 & local CVE cache
                          - Validates CVE ID existence & CVSS vector
                                   │
                                   ▼
                       2. Prioritizer Agent (prioritizer.ts)
                          - Applies SSVC decision tree & contextual weighting
                          - Factors in asset exposure and sensitivity
                                   │
                                   ▼
                       3. Remediation Agent (remediation.ts)
                          - Generates structured remediation steps & ticket markdown
                          - Employs Provider-Agnostic LLM Adapter
                                   │
                                   ▼
                       4. Evaluator Agent (evaluator.ts)
                          - Cross-model verification pass (different provider)
                          - Checks CVE hallucination & fix correctness
                          - Triggers retry loop (max 2 retries) or human review flag
```

## Agents Breakdown

1. **`orchestrator.ts`:** Manages batch scheduling, agent execution order, state passing, and fallback handling.
2. **`enrichment.ts`:** Fetches authoritative vulnerability metrics (CISA KEV, EPSS, NVD CVSS v3.1).
3. **`prioritizer.ts`:** Produces a 1-100 contextual risk score with explicit reasoning justifications.
4. **`remediation.ts`:** Drafts actionable patch instructions and ticket summaries conforming to strict JSON schema.
5. **`evaluator.ts`:** Performs adversarial critique and cross-model validation.
6. **`providers/adapter.ts`:** Unified LLM interface supporting Claude (Anthropic), Gemini (Google), and OpenAI.
