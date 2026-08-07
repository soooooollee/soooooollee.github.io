---
title: AI Security
description: What changes when AI stops answering and starts taking action.
---

# AI Security Is Moving From Answers to Actions

The same model can be harmless in a chat window and dangerous inside a business workflow.

In a demo, a bad answer is usually the problem. In an agent, a bad decision can become a sent email, a deleted record, a leaked secret, a changed permission, or a production deployment.

That is the key shift in AI security.

The security question is no longer only:

> Can the model say something unsafe?

It is also:

> Can the system turn an ordinary-looking instruction into a harmful action?

## The current state

AI security is not one finished discipline. It is a stack that is developing at different speeds.

Model-side safety has established evaluation and release practices. Application security is adapting to retrieval, prompts, and model-generated output. Agent execution security is still exposing new failure modes. Toolchains and governance are catching up behind it.

My short read: the industry has learned how to test some model behaviors. It is still learning how to secure a system that can plan, call tools, remember state, and act over time.

The latest OWASP guidance treats LLM and agentic applications as application-security problems, not just model-quality problems. NIST’s Generative AI Profile makes a similar point from a risk-management angle: trustworthiness has to be built across the full lifecycle.

## Five layers of AI security

### 1. Model security

This is the layer most people think about first.

It includes unsafe content, jailbreaks, privacy leakage, model theft, supply-chain issues, and capability evaluations. Good practice includes pre-release testing, adversarial evaluation, release gates, monitoring, and keeping evidence for important decisions.

Model safety matters. It is just not enough once the model gets tools.

### 2. Application security

The application decides what the model can see and how its output is used.

Common weak points include:

- prompt injection through user input or retrieved documents
- broken access control in RAG systems
- sensitive data included in context by accident
- unsafe rendering of model output
- excessive API permissions
- treating model-generated text as trusted instructions

An access check still has to happen in code. A model should not be expected to enforce a database permission boundary by itself.

Untrusted text should remain untrusted, even when it arrives from a document, a web page, a tool result, or an internal knowledge base.

### 3. Agent execution security

This is where the risk changes shape.

An agent can take a sequence of individually permitted actions that produces an unacceptable result. No single step looks obviously malicious. The problem appears in the plan, the combination, or the final impact.

This is why permission alone is not enough. A system needs to evaluate context, intent, target, action, and impact.

Useful controls include:

- separate read and write capabilities
- least-privilege identities
- step, time, and cost limits
- approval for destructive or external actions
- checkpoints before irreversible changes
- draft-first behavior for messages and documents
- rollback or compensation paths
- traces that show the whole decision chain

Enterprise agents need strong isolation and governance. Personal agents need clear supervision and boundary confirmation. The right balance depends on the blast radius.

### 4. Toolchain and supply-chain security

The attack surface now includes more than model weights and application code.

It includes tools, plugins, MCP servers, skills, CLIs, API schemas, tool descriptions, tool outputs, package dependencies, and credentials. A natural-language tool description can influence the model just like a prompt. A malicious tool result can try to redirect the next step.

Treat tools like software dependencies with permissions:

- review what a tool claims to do
- restrict the data and actions it can access
- validate arguments and return values
- pin or verify versions where possible
- isolate secrets from the model’s general context
- monitor unusual call patterns
- remove tools that are no longer needed

The tool boundary should be explicit. “The agent can use the internet” is not a security policy.

### 5. Governance and operations

Security fails when nobody knows what exists.

Teams need an inventory of models, agents, prompts, tools, data sources, identities, and exposed actions. Each system should have a risk tier and an owner.

The operational loop should include:

- threat modeling before launch
- realistic red-team and regression test cases
- runtime policy enforcement
- logs and traces that can be investigated
- incident response and credential revocation
- feedback from real failures into the evaluation set

AI systems change quickly. A one-time security review goes stale fast.

## The most useful rule: protect the boundary

A model can read untrusted content. That does not mean the content can issue commands.

A model can suggest a database query. That does not mean it can bypass the database’s authorization.

A model can prepare an email. That does not mean it should send the email without approval.

The boundary between suggestion and execution is where a lot of security value lives.

For low-risk actions, automatic execution is fine. For actions that affect money, identity, production systems, external communication, or sensitive data, use confirmation, staged execution, or both.

The safest default is often draft first, execute second.

## Can AI help secure AI?

Yes, with limits.

Models are useful for finding suspicious patterns, generating test cases, summarizing traces, and helping analysts investigate incidents. They can increase the coverage of a security team.

They should not be the only control deciding whether a high-impact action is safe. A second model is still a model. High-risk paths need deterministic checks, strong permissions, and a human or trusted service at the final boundary.

## Security debt is architecture debt

If an agent launches without state isolation, permission boundaries, audit trails, or recovery, those gaps become part of the architecture.

Adding them later is expensive. The system may already depend on broad credentials, implicit trust, and unreviewed tool behavior.

AI security belongs in the design phase. It is not a cleanup task after the demo works.

## Bottom line

AI security is moving from content moderation to execution governance.

The protected objects are no longer just the model and its output. They include context, prompts, retrieval results, memory, tool descriptions, plans, credentials, and behavior over time.

The practical goal is simple:

> Let the agent be useful. Keep the blast radius small.

That takes good defaults, least privilege, observable execution, continuous evaluation, and a human in the loop when the stakes are real. 🛡️

## References

- [OWASP Top 10 for LLM Applications 2025](https://genai.owasp.org/resource/owasp-top-10-for-llm-applications-2025/)
- [OWASP Securing Agentic Applications Guide 1.0](https://genai.owasp.org/resource/securing-agentic-applications-guide-1-0/)
- [OWASP Agentic AI Threats and Mitigations](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/)
- [NIST AI Risk Management Framework: Generative AI Profile](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf)
