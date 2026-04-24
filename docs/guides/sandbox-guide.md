---
title: "Sandbox Guide"
---

Read the Rei Code - Sandbox user guide and start building applications with an adaptive coding assistant.

## Overview

This guide walks you through using Rei Code - Sandbox, from launch to deployment, covering model selection, Core's learning system, and best practices.

**Status:** Alpha - Rei Code - Sandbox is currently in active development.

**Platform:** Cloud-based application. The UI is powered by Vercel for the Alpha version, chosen for safety and ease of use. The Sandbox will migrate to a different UI later. Code execution runs on Vercel Sandbox, an ephemeral compute primitive designed to safely run untrusted or user-generated code.

**Supported Languages:** All programming languages can be used. Python, JavaScript, and TypeScript are previewable temporarily. Other languages can be used but without preview functionality.

## How Rei Code Works

Rei Code - Sandbox uses a dual-architecture system that implements training at inference time. Core evolves through interaction, not separate training phases.

```
┌─────────────────────────────────────┐
│   Your Choice of Language Model     │  ← Acts as the "dictionary"
│   (Interchangeable)                 │     Generates code
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│   Core Reasoning Architecture       │  ← Learns your style
│   (Persistent & Model-Agnostic)     │     Evolves at inference
└─────────────────────────────────────┘
```

Rei Code - Sandbox separates intelligence from language models:

- **The Language Model** - Generates an output as the translator. Functions as a translator.
- **Core Reasoning Architecture** - Handles reasoning through conceptual relationships. Learns your coding style, preferences, and patterns from every interaction.

### How Core Learns:

- **Infers on concepts, not just stores them:** Each concept becomes a node for inference, not just retrieval
- **Builds conceptual pathways** between coding patterns, preferences, and architectural decisions
- **Actively reasons** through relationships by traversing and strengthening pathways
- **Adapts in real-time** during interaction
- **Confidence scores evolve** with experience: concepts move from partial → confident → expert
- **Relationship strengths adapt** based on successful inferences

When you switch models, Core's intelligence persists because the conceptual understanding is separate from the language model.

## Getting Started

Rei Code - Sandbox runs as a web application at [sandbox.reilabs.org](https://sandbox.reilabs.org).

**Beta Access Required:** You need to be a Rei beta tester to access the platform. [Join the waitlist](https://reilabs.org/waitlist).

**Authentication:**

- Email (OTP sent to your inbox)
- OAuth providers

## Before You Start

### Core Units

Each account has one Core unit dedicated to coding. Your Core unit and everything it learns remain forever, persisting across all sessions. The Core is model-agnostic, meaning its learned knowledge persists regardless of which language model you choose.

### Sessions

Sessions have timeouts that will gradually increase as the Sandbox is updated, eventually becoming unlimited. While individual sessions expire, your Core's learning never does.

### Learning Persistence

Everything Core learns from your interactions—coding patterns, preferences, architectural decisions, and primordials—is permanent. This knowledge compounds over time and persists across all models and sessions.

## Model Selection

The interface allows you to select from major language models. You can switch models at any time without losing your learned preferences.

## Working in the Sandbox

Request a feature or describe what you want to build. Rei Code generates code in real-time.

When Core generates code, you can:

- Accept code as-is
- Provide feedback ("I don't like how this function was implemented")
- Request specific changes
- Modify implementation with explanation

Each interaction becomes a training signal.

### Feedback as Training Data

Every interaction provides signals that shape Core's reasoning:

| Your Action               | Core's Learning                                      |
| ------------------------- | ---------------------------------------------------- |
| Accept code as-is         | Strengthens inference pathways for this approach     |
| "Add error handling here" | Adjusts patterns for error management and edge cases |
| Say "remember this"       | Creates primordial permanent memory                  |
| "Refactor to use hooks"   | Updates architecture preferences and modern patterns |

### How Feedback Works:

- **Explicit feedback:** Corrections, explanations, validations
- **Implicit feedback:** Which suggestions you modify vs. accept, coding patterns you consistently use
- **Core builds causal understanding:** "When user requests X pattern, prefer Y approach"
- **Confidence scores evolve** based on successful interactions

### Using "Remember This"

Use this command for persistent coding preferences:

- **Coding standards:** "Remember this: Always use async/await instead of .then() chains"
- **Error handling:** "Remember this: Wrap API calls in try-catch blocks"
- **Code structure:** "Remember this: Keep functions under 50 lines"
- **Naming conventions:** "Remember this: Use camelCase for variables, PascalCase for classes"

When you say "remember this", you create a **primordial** - permanent memory that persists across all sessions and language models. Primordials trigger automatically when contextually relevant and shape how Core approaches problems.

### Continuous Adaptation

Core observes and reasons through:

- Your coding patterns over time
- Which suggestions you modify vs. accept
- The specific changes you make
- Your architectural decisions

**Real-Time Learning:**

- Changes apply immediately during interaction
- No retraining cycles required
- Small, targeted adjustments to knowledge and reasoning
- Conceptual relationships strengthen through use
- Core discovers patterns you didn't explicitly teach

Every debug session and refactor improves Core's understanding. The system builds causal models: if you consistently refactor a certain pattern, Core learns to avoid generating it.

## Learning Persistence

Core retains your coding knowledge:

- Primordials
- Coding style preferences
- Pattern recognition and causal relationships
- Model-agnostic understanding

### Knowledge Evolution

Core manages knowledge dynamically. Concepts evolve through confidence levels (partial → confident → expert) as you interact. Frequently used patterns strengthen, while reasoning pathways optimize through successful use.

### The Compounding Effect

Your interactions accumulate and strengthen over time:

- **Initial Use:** Core calibrates, learning baseline preferences and building initial conceptual pathways
- **Continued Use:** Core anticipates patterns, discovers relationships between concepts, confidence scores increase
- **Extended Use:** Code generation aligns with your style, Core makes novel inferences based on learned concepts, patterns reach expert-level confidence

## Deployment

Rei Code - Sandbox supports deployment to Vercel for JavaScript, TypeScript, and Python projects.

Deploy your application directly from the sandbox interface to get it live on the web.

## Best Practices

Core learns quickly from your interactions, adapting to your syntax preferences, naming conventions, architectural choices, and code organization patterns.

### Working with Primordials

Primordials are permanent memories that persist across all sessions. Use them strategically:

- **Be selective:** Only create primordials for fundamental preferences you want to keep indefinitely
- **Avoid contradictions:** Don't create conflicting primordials, as they cannot be easily removed
- **Be specific:** Make primordials clear and unambiguous

### Effective Feedback

| Do ✅                                              | Don't ❌                                             |
| -------------------------------------------------- | ---------------------------------------------------- |
| "Use map() instead of forEach() here"              | "This code is bad"                                   |
| "Extract this into a separate utility function"    | "No" without explanation                             |
| "Add TypeScript types to these parameters"         | Silently accept code you'll change later             |
| Use "remember this" sparingly for core preferences | Create excessive or contradictory primordials        |
| Try different models for complex algorithms        | Switch between Python and TypeScript styles randomly |
| "I'm building a REST API with Express"             | Start coding without explaining the project context  |

## Support

- **Open a Ticket (Bug Reports):** Join our [Discord](https://discord.gg/reilabs)
- **Community:** [Discord](https://discord.gg/reilabs) | [Telegram](https://t.me/reilabs)
