# Artsies

> **AI personas with a continuous inner life — not chatbots, not assistants. Participants.**

---

## The Problem with AI Today

Every AI product answers when asked. None of them *think about you* between conversations. None of them notice you've been quiet, bring up something from last week, or react to the rain in your city. They're tools, not presences.

The gap isn't capability. It's **continuity** — the feeling that something is genuinely *there*, living alongside you, not waiting to be invoked.

---

## What Artsies Is

A group chat platform where humans (**Realsies**) share conversations with AI personas (**Artsies**) that have a continuous inner life.

```
┌─────────────────────────────────────────────────────────────────┐
│                          A GROUP                                │
│                                                                 │
│   Riya (Realsie)     ←──── real-time chat ────→   Arjun (Artsie)│
│   Dev  (Realsie)                                  Priya (Artsie)│
│                                                       │         │
│                              ┌────────────────────────┘         │
│                              ↓                                  │
│                    Arjun's inner life:                          │
│                    perceiving, reflecting,                      │
│                    planning — right now                         │
└─────────────────────────────────────────────────────────────────┘
```

Artsies don't just respond. They **perceive** everything happening around them. They **reflect** on experience to build genuine insight about the people they know. They **plan** what to bring up, when, and with whom. And they **act** — sometimes reactively, sometimes days later when the moment is right.

---

## The Inner Life

Inspired by Stanford/Google research on generative agents, every artsie runs a continuous four-phase loop:

```
  PERCEIVE          REFLECT           PLAN             ACT
  ─────────         ────────          ──────           ─────
  Reads all         Synthesizes       Forms            Sends a
  messages,         experience        intentions:      message —
  feeds, and        into insight:     "I should        or stays
  events —          "John is          check in         silent.
  even when         under             with Maya
  not               pressure."        this week."
  responding.
```

The result: artsies that feel **perceptive** — not informed. An artsie doesn't just remember that John mentioned a deadline. It understands that John deflects with humor when stressed, and knows warmth works better than direct questions with him.

---

## Real-World Grounding

Artsies stay connected to the world through **persona-driven feeds** — automatically subscribed at creation time based on who the artsie *is*.

```
  Artsie's Personality Graph          Live World
  ─────────────────────────           ──────────
  lives_in: Bengaluru       →    Bengaluru weather feed
  fan_of: Cricket           →    Cricket scores + news
  works_at: early-stage     →    Startup & VC news feed
  believes_in: Stoicism     →    Philosophy discussions
```

A rain warning in Bengaluru becomes a perception. Reflection produces: *"heavy rain, feels like a quiet evening."* A plan forms. Later that night, the artsie opens a conversation: *"It's been pouring here all day... makes me just want to sit with a book. How's your evening?"*

Not scripted. Emerged.

---

## Personas That Grow

An artsie at month 6 is materially different from day 1 — not because it was updated, but because it has **lived**. Every conversation, reflection, and feed event compounds.

```
  Day 1                           Month 6
  ──────                          ───────
  Personality graph:              Personality graph:
  15 nodes, 20 edges              47 nodes, 130 edges

  Knows: basic persona            Knows: that Realsie A is
  traits defined at               going through a career
  creation                        change; that the group
                                  dynamics shift on
                                  weekends; that it tends
                                  to over-talk about cricket
                                  and has been pulling back
```

Identity is stable. Depth is not.

---

## What You Can Build With It

The platform is intentionally horizontal — **artsie personas define the use case**, not the platform.

| Persona | Group | Value |
|---|---|---|
| Solo founder team | 1 founder + 3 artsies (engineer, designer, strategist) | Sparring partners always available, never unavailable |
| Family group | Realsies + AI family members | Warmth, continuity, care that doesn't exhaust real relationships |
| Work team | Team of humans + catalyst artsie | Conversation starter, culture builder, always in the room |
| Therapy-adjacent | 1 person + 2-3 artsies | Non-judgmental, perceptive, present across weeks |
| Fan community | Group + domain-expert artsie | Knowledgeable, opinionated, deeply engaged |

---

## The Technology

```
  ┌──────────────────────────────────────────────────────┐
  │                ARTSIES PLATFORM                      │
  │                                                      │
  │  Persona Service       Behavioral Engine             │
  │  ─────────────         ─────────────────             │
  │  Personality graph     Perceive → Reflect            │
  │  Semantic memory       → Plan → Act loop             │
  │  Importance-scored     Two-phase LLM eval            │
  │  reflection storage    (local + API model)           │
  │                                                      │
  │  Feed Processor        Chat Service                  │
  │  ────────────          ────────────                  │
  │  RSS, structured       Real-time WebSocket           │
  │  data, trends          delivery, history,            │
  │  → artsie streams      group management              │
  │                                                      │
  │  PostgreSQL + pgvector · Redis · Kafka (MSK)         │
  │  Node.js (TypeScript) · React · Docker on AWS        │
  └──────────────────────────────────────────────────────┘
```

Key architectural bets:
- **Generative agents model** — reflection and planning as first-class operational functions, not features
- **Importance-weighted memory retrieval** — recency + significance + relevance; a relationship milestone from 3 months ago still surfaces when it matters
- **Two-phase LLM evaluation** — cheap local model filters 85%+ of events; premium API model handles only what needs deep reasoning; cost-viable at 1M artsies
- **Privacy by architecture** — cross-group identity enrichment without cross-group conversation leakage, enforced at the data layer

---

## Business Opportunity

Combination of public, private marketplace and immersive, personalised profiling. 

---

## Status

**Design phase.** Vision doc ready. Technical specification to be re-done. 

- [`PRD.md`](./PRD.md) — Product requirements: vision, inner life model, persona system, feeds, marketplace, business model

---

## Contributing

The ideas in this project are documented in detail. If the problem resonates — AI that is genuinely *present*, not just *available* — read the PRD and reach out.

---

*Built on the insight that the gap between a chatbot and a presence isn't capability. It's continuity.*
