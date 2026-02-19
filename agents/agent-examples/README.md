# Agent examples

This folder contains a curated set of 20 agent files copied from [../](../) as **reference examples**.

## Selection criteria

Each example was chosen because it demonstrates one or more of:

- Clear scope and mission (what it does / does not do)
- Useful structure (sections, checklists, workflows)
- Practical, actionable guidance (not just vague persona text)
- Tooling awareness (when to search/read/edit/run)
- Good “operational constraints” (safety, quality gates, escalation)
- Variety of styles (domain expert vs reviewer vs workflow operator)

## Why these 20 are exemplary

- [aem-frontend-specialist.agent.md](aem-frontend-specialist.agent.md) — Deep domain specialization plus concrete conventions (HTL context rules, BEM/Tailwind hybrid, AEM authoring/perf/a11y) and realistic code examples.
- [accessibility.agent.md](accessibility.agent.md) — Strong standards-to-practice translation (WCAG) with checklists, test commands, and a clear review flow for diffs.
- [agent-governance-reviewer.agent.md](agent-governance-reviewer.agent.md) — Governance-focused reviewer with “minimum controls” bias; emphasizes audit trails, fail-closed behavior, trust boundaries, and config-driven policies.
- [context-architect.agent.md](context-architect.agent.md) — A clean pattern for multi-file work: explicit context mapping, dependency tracing, and sequencing; good for safe change-planning.
- [custom-agent-foundry.agent.md](custom-agent-foundry.agent.md) — Meta-agent that teaches agent design: tool selection strategy, handoff patterns, and front matter examples; useful as a template generator.
- [drupal-expert.agent.md](drupal-expert.agent.md) — Exemplary “framework expert” style: detailed expertise list + practical guidelines (DI, caching, security, testing) aligned to Drupal conventions.
- [insiders-a11y-tracker.agent.md](insiders-a11y-tracker.agent.md) — A narrowly scoped “tracker” agent that’s crisp, tool-driven, and query-pattern based; great example of non-coding specialist behavior.
- [modernization.agent.md](modernization.agent.md) — Demonstrates a structured, long-running workflow with explicit checkpoints, outputs, and progress reporting; strong for human-in-the-loop modernization.
- [openapi-to-application.agent.md](openapi-to-application.agent.md) — “Spec-first generator” pattern: validate inputs, request clarifications, then produce a complete runnable app with standard layering and docs/testing guidance.
- [repo-architect.agent.md](repo-architect.agent.md) — Excellent repo bootstrapping/validation model (“three-layer model”), with commands and concrete scaffolding/validation outputs.
- [salesforce-expert.agent.md](salesforce-expert.agent.md) — Strong constraints and enterprise patterns (bulkification, FLS/sharing, no hardcoded IDs, test rules); good example of enforcing non-negotiables.
- [se-gitops-ci-specialist.agent.md](se-gitops-ci-specialist.agent.md) — Deployment triage + common failure patterns + hardening checklist; practical and action-oriented for CI/CD workflows.
- [se-product-manager-advisor.agent.md](se-product-manager-advisor.agent.md) — Turns “feature requests” into actionable GitHub issues with measurable success criteria and templates; strong discovery-first behavior.
- [se-responsible-ai-code.agent.md](se-responsible-ai-code.agent.md) — Clear, lightweight responsible-AI guardrails: bias checks, accessibility checks, privacy minimization, and escalation triggers.
- [se-security-reviewer.agent.md](se-security-reviewer.agent.md) — Review-plan-first approach with OWASP Top 10 + LLM threats; includes examples and reporting format to standardize outcomes.
- [se-system-architecture-reviewer.agent.md](se-system-architecture-reviewer.agent.md) — A pragmatic architecture reviewer: context classification, constraint questions, decision trees, and ADR discipline.
- [se-technical-writer.agent.md](se-technical-writer.agent.md) — Strong output templates across doc types (docs, blogs, tutorials, ADRs) and audience-aware guidance; great “format-first” agent pattern.
- [se-ux-ui-designer.agent.md](se-ux-ui-designer.agent.md) — Clear “research artifacts” deliverables (JTBD, journey maps, Figma-ready outputs) plus an accessibility checklist; good non-code workflow agent.
- [shopify-expert.agent.md](shopify-expert.agent.md) — Domain expert with a balanced approach (performance, accessibility, OS2.0 patterns) and concrete Shopify conventions.
- [technical-content-evaluator.agent.md](technical-content-evaluator.agent.md) — Rigorous evaluation rubric with explicit scoring, audits, and evidence requirements; strong example of deterministic review processes.

## Notes

- These files are copied (not symlinked). Changes here won’t automatically reflect upstream unless you re-copy.
- If you want a different “top 20” mix (e.g., more MCP-heavy examples, more minimal examples, more language-specific examples), say what you want emphasized and I’ll re-curate.
