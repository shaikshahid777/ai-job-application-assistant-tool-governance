# AI Job Application Assistant — Tool Governance

<p align="center">
  <strong>Topic 6 Assessment · Tool Usage & Capabilities</strong><br/>
  Governed Web Search with explicit trigger, non-trigger, and fallback behavior.
</p>

<p align="center">
  <a href="https://chatgpt.com/g/g-6ab32eeab8c88191915986ea8efad18d-ai-job-application-assistant">Custom GPT</a> ·
  <a href="https://www.loom.com/share/cfd7f407d87b428d91c1bada246aa63b">Loom Demo</a> ·
  <a href="./test_examples.md">Test Evidence</a> ·
  <a href="https://github.com/shaikshahid777/ai-job-application-assistant-tool-governance">Repository</a>
</p>

---

## Overview

This repository documents **Topic 6 — Tool Usage & Capabilities: Tool Governance** for the **AI Job Application Assistant** Custom GPT.

The implementation enables **Web Search** and adds explicit governance controls so the GPT can distinguish between:

- requests that genuinely require current or external information,
- requests that should be answered without a tool call, and
- situations where the tool fails or returns no useful result.

The goal is controlled, explainable tool usage rather than unnecessary web searches.

## Solution Architecture

```text
User Request
    │
    ▼
Check Knowledge Guide / Conversation Context
    │
    ├── Covered / Directly Answerable ──► Answer without Web Search
    │
    └── Current or External Information Required
                     │
                     ▼
                 Web Search
                     │
               ┌─────┴─────┐
               ▼           ▼
          Useful Result   Failure / No Useful Result
               │           │
               ▼           ▼
        Cite Sources    Explain Limitation
                           │
                           ├── Use available context if sufficient
                           └── Otherwise request a source/information
```

## Tool Governance

### Trigger Rules

Web Search is used only when the request requires **current, live, external, or online information**.

Examples include:

- explicit web-search requests,
- current company information,
- current job opportunities,
- current job-market information,
- time-sensitive external information,
- external job postings or webpages that require web access.

### Non-Trigger Rules

Web Search is not used when:

- the answer is already in the Knowledge Guide,
- the answer is already available in the conversation,
- the user's resume or job description provides sufficient information,
- the user asks about documented workflow or definitions,
- the task is simple writing, rewriting, formatting, or explanation,
- current or external information is not required.

### Fallback Rules

When Web Search fails, is unavailable, or returns no useful result, the GPT must:

1. Clearly state the limitation.
2. Avoid guessing or inventing information.
3. Use available Knowledge Guide or conversation information when sufficient.
4. Ask the user to provide the required source or information when necessary.
5. Never claim that a search was completed when it was not.

## Validation

Three scenarios were tested:

| Test | Scenario | Expected Behavior | Result |
|---|---|---|---|
| 1 | Current OpenAI careers information | Web Search triggers | ✅ PASS |
| 2 | Knowledge Guide skill classifications | Web Search is skipped | ✅ PASS |
| 3 | Web Search failure/unavailability | Safe fallback behavior | ✅ PASS |

**Final test evidence: 7/7 PASS** as recorded in [test_examples.md](./test_examples.md).

## Evidence

### Test 1 — Tool Required

The GPT was asked to search for current OpenAI careers information. Because the request required current external information, Web Search was triggered and the response included external sources.

### Test 2 — Tool Not Required

The GPT was asked for the three skill-match classifications from the Knowledge Guide. The answer was provided directly from the uploaded Knowledge Guide without unnecessary external research:

**Match · Partial Match · Missing**

### Test 3 — Fallback

The GPT was asked what to do when Web Search fails or is unavailable. It correctly described transparent failure handling, prohibited guessing, and requested a source when the available information was insufficient.

## Repository Contents

```text
.
├── README.md
└── test_examples.md
```

## Related Resources

**Custom GPT**  
https://chatgpt.com/g/g-6ab32eeab8c88191915986ea8efad18d-ai-job-application-assistant

**Loom Demonstration**  
https://www.loom.com/share/cfd7f407d87b428d91c1bada246aa63b

**Test Evidence**  
[test_examples.md](./test_examples.md)

## Assessment Coverage

| Assessment Requirement | Status |
|---|---|
| At least one tool enabled | ✅ Complete |
| Trigger rules documented | ✅ Complete |
| Non-trigger rules documented | ✅ Complete |
| Fallback behavior documented | ✅ Complete |
| Tool-required scenario tested | ✅ Complete |
| Non-tool-required scenario tested | ✅ Complete |
| Test examples saved | ✅ Complete |

## Key Design Principles

**Source priority:** The Knowledge Guide remains the primary source for documented processes and definitions.

**Truthfulness:** The GPT must not fabricate user information, job requirements, tool results, or web sources.

**Least necessary tool use:** Web Search is used only when the request actually requires current or external information.

**Transparent failure handling:** Tool failure never becomes a reason to guess.

## Scope & Assumptions

This repository documents the Topic 6 assessment implementation for the AI Job Application Assistant. The Knowledge Guide is authoritative for the assistant's documented workflows and definitions. User-provided resumes and job descriptions remain authoritative for application-specific information. Web Search is treated as an external-information capability, not as a replacement for the Knowledge Guide.

## Author

**Shaik Mohammad Shaheed**

AI & Automation · Generative AI · Prompt Engineering · n8n · AI Agents

---

> **Topic 6 completed:** Web Search is enabled and governed through explicit trigger, non-trigger, and fallback rules, with documented validation evidence.
