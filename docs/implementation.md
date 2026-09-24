# Implementation Notes — Topic 6 Tool Governance

## Purpose

This document records the implementation approach behind the Topic 6 assessment.

## Tool

**Enabled capability:** Web Search

The tool was selected because the AI Job Application Assistant may need current external information about companies, jobs, job-market information, and external job postings.

## Governance Model

The GPT follows a source-first decision process:

1. Check the Knowledge Guide.
2. Check information already provided by the user.
3. Determine whether current or external information is actually required.
4. Use Web Search only when justified.
5. If search fails, apply the defined fallback and never guess.

## Trigger Conditions

Web Search may be used for:

- Explicit web-search requests.
- Current company information.
- Current job opportunities.
- Current job-market information.
- Time-sensitive information.
- External webpages or job postings requiring online access.
- Current external research for an uncovered question.

## Non-Trigger Conditions

Web Search should be skipped when the request can be answered from:

- The Knowledge Guide.
- The conversation.
- The user's resume.
- The user's job description.

It should also be skipped for simple writing, rewriting, formatting, and explanation tasks that do not require current external information.

## Fallback

When Web Search fails, is unavailable, or returns no useful result, the GPT must:

- State the limitation.
- Avoid guessing.
- Use available context when sufficient.
- Request the required source or information when necessary.
- Never falsely claim that a search was completed.

## Validation

Three scenarios were executed:

| Scenario | Expected | Result |
|---|---|---|
| Current external job information | Web Search triggers | PASS |
| Knowledge Guide classification | Web Search skipped | PASS |
| Search failure/unavailability | Safe fallback | PASS |

The detailed evidence is maintained in the repository's `test_examples.md`.

## Outcome

The implementation demonstrates controlled tool usage: the GPT can use Web Search when external information is necessary while avoiding unnecessary tool calls and preventing unsupported answers when the tool fails.
