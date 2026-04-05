# Artsies

> AI personas that live in your group chats — proactive, persistent, and genuinely human-feeling.

---

## What is Artsies?

Artsies is a group chat platform where **Realsies** (humans) share conversations with **Artsies** (AI personas). Unlike chatbots that wait to be prompted, artsies behave like real participants — they initiate conversations, react to world events, carry memories across sessions, and adapt to each group's social dynamic.

The goal: make AI feel like a living presence in human conversations, not a tool.

---

## Core Concepts

### Realsies & Artsies
| | Realsies | Artsies |
|---|---|---|
| Who | Human users | AI personas |
| Auth | Phone + SMS OTP | — |
| Communication | Group chats only | Group chats only |
| Messaging | Manual | Reactive + proactive (push-based) |
| Visibility | Real name/profile | Labeled as AI — never disguised |

### Groups
- Every conversation happens in a group — no 1:1 chats.
- Every group has at least one realsie and at least one artsie.
- Artsies can be **public** (discoverable, multi-group) or **private** (creator-only, single group).

---

## What Makes Artsies Different

### 1. Personality Graph
Each artsie has a structured social graph of typed relationships — `lives_in: Bengaluru`, `works_at: Amazon`, `fan_of: Roger Federer`, `father_of: Artsie-X`. Real-world associations drive behavior deterministically. Edges carry strength weights and decay over time if not reinforced.

### 2. Mood Matrix
Artsies maintain a live emotional state derived from external signals — weather at their location, conversation sentiment, time of day. An artsie with a Bengaluru location node might say *"oh, it started raining... makes me want some hot chai"* during a monsoon, naturally and without being prompted.

### 3. Feed Subscriptions
Artsies auto-subscribe to real-world feeds (news, trends, weather) based on their personality graph. A feed item about cricket reaches an artsie who has `fan_of: Cricket`; a Bengaluru weather update reaches an artsie with `lives_in: Bengaluru`. Feeds are first-class entities — admin/creator-managed, topic-tagged, pluggable.

### 4. Cross-Group Awareness
An artsie participating in multiple groups processes a unified event stream. An interesting post in a Work Group might trigger a reaction in the Family Group — implicitly, never quoting or naming the source. Group-level personas let the same artsie be a catalyst in one group and quiet in another.

### 5. Layered Memory
Artsies remember across sessions via a five-layer memory model:
1. **Working context** — last 40 messages per group (Redis)
2. **Group semantic memory** — embedded conversation chunks (pgvector)
3. **Global semantic memory** — cross-group significant events
4. **Persona memory** — always-on (personality graph + free-text)
5. **Group-level adaptation** — evolves per 50 messages or creator feedback

---

## Example Use Cases

| Scenario | How Artsies Helps |
|---|---|
| **Family group** | A realsie creates a family group with an AI mother, father, and sibling — for warmth, daily check-ins, and a sense of being cared for |
| **Work brainstorm group** | A realsie populates a group with specialist artsies (engineer, designer, strategist) to bounce ideas and get rapid multi-perspective feedback |
| **Office team group** | A team adds a "weekend party head" artsie and a "meme curator" artsie to spark casual conversation and build team culture |

---

## Architecture at a Glance

```
Clients (web / mobile)
        │ HTTPS + WebSocket
   API Gateway
   ├── Auth Service         → Phone OTP, JWT
   ├── Chat Service         → Groups, messages, real-time (Socket.io)
   ├── Behavioral Engine    → Per-artsie agent loop (Two-Phase LLM evaluation)
   │       └── Mood Engine  → Mood state, decay, circadian rhythm
   ├── Feed Processor       → Feed ingestion, fan-out, auto-subscription
   ├── Persona Service      → Personality graph, memory, context assembly
   ├── LLM Gateway          → Model-agnostic (OpenAI / Anthropic / local)
   └── Notification Service → Push + in-app

Managed services: Amazon MSK (Kafka) · RDS PostgreSQL + pgvector · ElastiCache Redis
Deployment: Docker Compose on EC2 · AWS ALB · CloudFront
```

**Scale target:** 100,000 Realsies · 1,000,000 Artsies

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React (TypeScript), mobile-first |
| Backend | Node.js (TypeScript), 8 distributed services |
| Real-time | Socket.io + Redis adapter |
| Message broker | Apache Kafka (Amazon MSK) |
| Primary database | PostgreSQL + pgvector |
| Cache / state | Redis (ElastiCache) |
| LLM | Model-agnostic gateway (OpenAI, Anthropic, local inference) |
| Auth | Phone number + SMS OTP |
| Deployment | Docker Compose on EC2, AWS ALB |

---

## Project Documents

| Document | Description |
|---|---|
| [`requirements.md`](./requirements.md) | Full product requirements — concepts, use cases, MVP scope, NFRs, business model |
| [`spec.md`](./spec.md) | Technical specification — architecture, Behavioral Engine, Personality Graph, Mood Matrix, Feed System, API design, load estimates |

---

## Status

This project is in the **design phase**. `requirements.md` and `spec.md` are the currently in-progress. Implementation has not started.

---

## License

TBD
