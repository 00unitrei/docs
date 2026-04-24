---
title: "Unit Usage Guide"
---

## What Core Is

Core is a reasoning architecture. It builds structured, relational knowledge and reasons by traversing it.

When you give Core a piece of information, it doesn't file it away as text. It identifies what things are, how they relate, what caused what, and where they sit relative to everything it already knows. That relational structure is what it thinks with. Every new piece of knowledge understood in relation to everything else.

This has practical consequences for how you use it. Core genuinely learns, which means how you teach it matters.

Core currently ships in two forms:

**Core (Agentic)**, the preliminary version, currently available. A human-language-friendly instance of the architecture, OpenAI-compatible and designed to facilitate natural language interaction with Core. An LLM serves as the articulation layer, translating Core's reasoning into conversational output. The LLM is the interface, not the intelligence.

**Core (Standalone)**, coming soon. A more complete, unrestricted instance of Core. Faster, with no dependency on external models for natural language. Direct access to the architecture without a translation layer.

This guide covers both. Differences are noted where they exist.

---

## How Knowledge Works in Core

### Representation

Core encodes knowledge as a structure of interconnected concepts. Stating _"Q3 revenue was $2.3M with 450 customers"_ produces something like:

```
Nodes:        [Q3]  [revenue]  [$2.3M]  [customers]  [450]

Edge:         (Q3, revenue, $2.3M, customers, 450) → 'quarterly_performance'

Derived:      (revenue ÷ customers) → ~$5,111 per customer

Links to:     [fiscal_year] → [growth_metrics] → [unit_economics]
```

Asking about customer value doesn't trigger a keyword search. Core walks the structure (revenue, customer count, temporal patterns) and constructs the answer from the traversal. Short questions take short walks. Complex questions take longer ones. The mechanism is the same either way.

### What Gets Learned vs What Stays Temporary

Core distinguishes between two categories of information:

**Conceptual learning** persists across sessions. Patterns, principles, preferences, and relationships become inference nodes: structural knowledge Core reasons with going forward. This is how Core gets smarter over time. It works in both UI and API.

**Session context** is temporary. Within a UI session, Core holds verbatim information from the last 4 messages. Exact values, working data, task-specific details, all available within that window, discarded after. Session context does not exist in the API.

The distinction is automatic. Core infers what carries lasting relevance based on how you interact. You don't tag or categorise anything.

### Growth and Decay

Core's knowledge compounds. Each new concept creates more relationships to discover and more paths to reason through. Expertise builds on itself.

What doesn't get reinforced fades. Knowledge that stops being relevant loses weight over time. This is by design; it keeps the reasoning structure clean and current.

---

## Training Core

There is no separate training phase. Core updates continuously. Every interaction can reshape its reasoning.

### How Core Learns From You

**Explicit correction** is the strongest signal. When you say "that's wrong because X," Core doesn't just note the correction. It adjusts the reasoning pathway that produced the error.

**Implicit validation** (accepting a response without modification) signals approval and reinforces the path that generated it.

**Pattern reinforcement** builds over time. Similar interactions across sessions create and strengthen inference pathways. Single mentions create weak connections; concepts you return to develop strong ones.

**Contextual association** links concepts that consistently co-occur. Discuss authentication alongside security often enough, and Core connects them.

### How to Teach Effectively

**Teach principles, not instances.**

```
Weak:    "Rename this to userList"
Strong:  "Array variables should be plural. This inconsistency reduces readability."
```

The first teaches one rename. The second creates an inference path Core applies to every future naming decision.

**Explain your reasoning.** "Avoid inline styles because they create maintenance burden and override unpredictably" gives Core something to generalise from. "Don't do that" gives it nothing.

**Be consistent.** If you sometimes accept a pattern and sometimes reject it without explanation, Core holds both interpretations without converging. It doesn't average competing signals or default to the most probable. It maintains the ambiguity until one side resolves clearly. Consistent feedback resolves it.

**Reinforce across sessions.** Long-term learning requires repetition. Concepts you engage with across multiple sessions become strong, reliable reasoning structures.

### Abstraction Matters

Too vague ("make it better") gives Core nothing to work with. Too specific (step-by-step instructions) gets followed but doesn't generalise. The right level is a principle with rationale. Core builds reusable inference paths from these.

---

## Primordials

Most of what you know, you understand conceptually. You reconstruct specifics when you need them. But some things you know exactly, always: your name, your address, your PIN. No reasoning required.

Primordials are Core's version of that. Permanent, exact, perpetually accessible knowledge anchors that sit outside the fluid reasoning everything else undergoes. Once created, they cannot be removed (until Core abstraction releases).

### Creating Primordials

Primordials are triggered by absolute language, phrasing that signals permanence.

**"Remember this" and "never forget" are the strongest triggers.** Other absolute phrasing ("always," "never," "without exception") can also work, but those two are the most reliable.

```
"Remember this: never use console.log in production code."
"Never forget: all API keys must be stored in environment variables."
```

### When to Use Them

Use primordials for things that are genuinely invariant: security policies, architectural principles, hard technical constraints. These are the bedrock facts that should anchor everything else Core reasons about.

Do not use primordials for preferences that might change, version-specific constraints, or anything you haven't already validated through normal interaction. Creating "use tabs" and then "use spaces" leaves both permanently active with no resolution.

Before creating one, ask: is this actually permanent? Does it conflict with something that already exists? Could Core learn this through regular feedback instead? If there's any doubt, teach it through interaction.

You choose the foundations. The same reasoning architecture applied to different domains needs different anchors, and only you know what should never drift.

---

## Verbatim Recall and External Data

You don't memorise every book you've ever read. You understand what's in them, know where to find them, and reach for them when exactitude matters. Trying to memorise everything would destroy the thing that makes your cognition valuable: the ability to reason abstractly and relationally.

Core works the same way. Exact figures, raw historical data, verbatim records: these are instruments Core reasons over, not material it internalises. Core knows how to use that information and what it means. That's the hard part. Retrieval is straightforward.

**Core abstraction** (coming soon) will formalise this by letting users connect external context (databases, documents, data sources, retrieval systems) that Core reasons over directly, without an LLM intermediary.

For now, API users handle task-specific context through prompting strategies, external databases, or application-layer state management. Open a ticket on Discord if you need guidance for your use case.

---

## Output Feedback Prohibition

**Never feed LLM-articulated responses back into the system.**

Core's reasoning is multi-dimensional: concepts connected through rich, structured relationships. The LLM articulation layer flattens that into linear text. This is a lossy, one-directional transformation:

```
Core reasoning  →  LLM articulation  →  Natural language
```

Feeding that output back tries to reverse the process, and it can't:

**Dimensionality loss.** An edge connecting five concepts becomes a sentence. There's no way to parse which nodes were connected or how.

**Artifact injection.** Core represents "strong quarterly growth." The LLM renders it as "robust expansion driven by customer acquisition." Feed that back and Core may create nodes for "robust," "expansion," "driven by." Stylistic artifacts that were never part of the reasoning.

**Compound degradation.** Each cycle amplifies noise. After several iterations, the structure is more linguistic residue than semantic content.

### In the API

Each request should be self-contained. Include all necessary context in the user message. Don't simulate conversation by passing previous system responses.

```javascript
// Correct
{ "prompt": "Analyze this code for auth vulnerabilities: [code]" }

// Incorrect
{
  "messages": [
    {"role": "assistant", "content": "[previous Units response]"},
    {"role": "user", "content": "Continue from there"}
  ]
}
```

### In the UI

The UI manages conversational context internally. Copying previous responses back into new prompts creates the same corruption. Let the system handle continuity.

---

## Interface Reference

**UI** provides session context (verbatim, last 4 messages), conceptual learning, and automatic session continuity. Designed for iterative dialogue.

**API** provides conceptual learning only. No session context. Each request must be self-contained. Designed for discrete tasks. If you need conversational continuity, implement state management in your application layer.

---

## Clearing Conversation vs Delete Memory

**Clear conversation** removes session context that hasn't consolidated into learned concepts yet. A limbo state. Useful for switching topics without short-term context bleeding over. Rarely needed.

**Delete memory** wipes the entire Units instance. All concepts, patterns, preferences, gone. Full reset.

---

## Units Cloning

Cloning duplicates a Units instance at its current state. Clone before making significant changes to Core's reasoning. If the new direction doesn't pan out, the original is intact.

---

## Integrated APIs

Integrated APIs are a closed beta perk. They remain highly unstable. These are commercial APIs the team is subsidising, and they are error-prone.

For maximum performance, use your own data feeds or provide data directly. Self-managed pipelines give you control over reliability, format, and timing. Integrated APIs are a convenience, not infrastructure.

---

## Troubleshooting

**"Core doesn't recall X"**: Within a session (UI), Core should have verbatim access to recent messages. Rephrase or re-state if it doesn't. Across sessions, Core retains relationships, not text. Reinforce the principle over multiple interactions if you want it to stick, or use an external database for exact recall.

**"Core keeps making the same mistake"**: Single corrections may not be enough, especially if other interactions reinforce the pattern. Provide consistent corrections with reasoning about why the alternative is better.

**"My primordials conflict"**: Both remain active. Provide explicit guidance in your queries to signal precedence. For severe conflicts, contact support.

---

## Summary

Core reasons by traversing structured, relational knowledge. It learns from how you interact with it: principles, corrections, patterns, consistency.

Teach with rationale. Reinforce across sessions. Use primordials sparingly and deliberately. Keep exact data in external sources where it belongs. Never feed articulated outputs back in. The reasoning is the architecture; everything else is instrumentation around it.
