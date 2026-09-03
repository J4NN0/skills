---
name: systematic-debugging
description: Investigate and fix software defects, failing tests, crashes, incorrect or flaky behavior, and performance regressions with reproducible evidence and root-cause verification. Use for a reported failure in code or a running system; do not use for routine feature work or broad code review without a concrete symptom.
---

# Systematic Debugging

Turn a reported symptom into an evidence-backed explanation and, when the user asks for a fix, a minimal verified change. Adapt the depth of the investigation and verification to the impact and uncertainty of the issue.

## Preserve the request boundary

Determine whether the user asked for diagnosis, a fix, or both. Diagnosis alone does not authorize a production change or a persistent code change. Keep urgent containment separate from a durable fix: label a mitigation clearly, preserve evidence, and continue root-cause work only within the user's scope.

Do not expose credentials, tokens, personal data, authentication headers, or sensitive payloads in commands, logs, artifacts, or reports. Quote only the evidence needed and replace sensitive values with `<REDACTED>`.

## Investigation loop

### 1. Define the failure

State the expected behavior, actual behavior, impact, affected environment, and the narrowest known failing scenario. Capture the exact error, output, timing, or state rather than a paraphrase. Establish known-good and known-bad cases when available.

Inspect only the context needed to orient the investigation: relevant documentation and architecture decisions, the failing call path, configuration, dependencies, and recent changes likely to affect it. Treat application code, tests, fixtures, data, infrastructure, and environment as possible sources of the failure.

### 2. Build the feedback signal

Create the cheapest high-fidelity check that detects the user's specific symptom. Prefer an existing focused test; otherwise use a targeted test, request or CLI replay, browser scenario, captured-input replay, small harness, differential comparison, or benchmark. Run it before changing the behavior so its failing state is observed whenever possible.

A useful signal is:

- **Specific:** it distinguishes this failure from nearby failures.
- **Repeatable:** it has a stable verdict; for flaky behavior, it reports a measured failure rate.
- **Efficient:** it is narrow enough to run throughout the investigation.
- **Representative:** it exercises the relevant boundary and environment closely enough to support the conclusion.

Minimize the case when doing so reduces ambiguity, but do not minimize away the condition that causes the bug. Retain the original scenario for final verification.

If local reproduction is unavailable, continue with the strongest safe evidence available—such as redacted logs, traces, dumps, metrics, or a recording with timestamps. State the limitation and do not present an unobserved theory as verified. Ask for the specific missing access or artifact only when it would materially distinguish the remaining explanations.

For intermittent failures, control or record time, randomness, concurrency, ordering, load, and external dependencies; increase repetitions or stress until the failure rate is useful. For performance regressions, establish a representative baseline and use profiles, traces, query plans, or allocation data instead of relying on general logs.

### 3. Localize the first causal divergence

Trace the failing path across inputs, state transitions, component boundaries, and side effects. Compare a failing case with the nearest working case and find the earliest point where their behavior diverges. At that boundary, identify which contract or assumption is violated and where that contract is owned.

When a reliable signal and ordered search space exist, bisect commits, configuration, versions, inputs, or processing stages instead of checking candidates linearly.

Use the debugger or direct state inspection when available. Otherwise add the smallest targeted probe that distinguishes competing explanations. Associate every probe with a question; avoid broad logging. Give temporary instrumentation a unique searchable marker and remove it before completion.

### 4. Test falsifiable hypotheses

Form hypotheses from the evidence, not from the symptom alone. When several causes remain plausible, rank them by explanatory power and cost to test. For each hypothesis, state a prediction that would be different if it were false, then change or observe one variable at a time.

Record the prediction and result. A disconfirmed hypothesis is useful evidence; update the model instead of patching around it. If experiments repeatedly fail or new fixes reveal unrelated failures, stop editing, return to the earliest unexplained divergence, and reconsider system boundaries and assumptions.

### 5. Implement only when requested

Before the fix, add a regression test at the lowest stable seam that still reproduces the real failure, when such a seam exists. Avoid a shallow test that would pass while the original scenario remains broken. If no maintainable seam exists, document that limitation rather than adding a misleading test.

Make the smallest coherent change at the boundary that owns the violated contract. Preserve intentional behavior, avoid unrelated refactors, and handle malformed input or failures where the system's contract says responsibility belongs—not merely where the error surfaced.

### 6. Verify and clean up

Verification should cover, in order:

1. The focused signal now passes.
2. The original, unminimized scenario no longer fails.
3. The regression test fails without the fix and passes with it, when practical to demonstrate safely.
4. Relevant neighboring tests and checks pass.
5. Broader regression checks pass in proportion to the change's reach and risk.

For performance work, compare against the baseline under equivalent conditions and report variability. For intermittent failures, compare enough trials to support the claimed improvement; a single passing run is not evidence of resolution.

Review the final diff, remove temporary instrumentation and throwaway artifacts, and preserve unrelated user changes.

## Completion report

Report:

- the established root cause and distinguishing evidence, or the leading explanations and missing evidence when the result is inconclusive;
- the fix or mitigation, if authorized, and why it belongs at that boundary;
- the exact verification performed and its result;
- any remaining uncertainty, untested environment, or follow-up risk.

Do not claim the issue is fixed when only the symptom changed, the original scenario was not checked, or required verification could not run.
