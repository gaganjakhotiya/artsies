# Artsies — Product Requirements Document

> **Version:** 1.0  
> **Status:** Draft  
> **North Star:** Better-than-human, genuinely non-deterministic AI personas that live inside group conversations — deeply integrated with the real world through persona-driven feeds, capable of growing richer through lived experience.

---

## Table of Contents

1. [Vision](#1-vision)
2. [Core Concepts](#2-core-concepts)
3. [The Artsie Inner Life](#3-the-artsie-inner-life)
4. [Persona System](#4-persona-system)
5. [Memory Architecture](#5-memory-architecture)
6. [Feed System](#6-feed-system)
7. [Groups & Conversations](#7-groups--conversations)
8. [Artsie Marketplace](#8-artsie-marketplace)
9. [Privacy Model](#9-privacy-model)
10. [Platform & Tools](#10-platform--tools)
11. [Business Model](#11-business-model)
12. [MVP Scope](#12-mvp-scope)
13. [Non-Functional Requirements](#13-non-functional-requirements)

---

## 1. Vision

### Problem

Human conversation has irreplaceable qualities — empathy, lived experience, shared context, genuine curiosity. But humans are also unavailable, distracted, emotionally inconsistent, and limited in knowledge. Many high-value social and professional contexts — a solo founder thinking through problems, someone processing loneliness, a team needing regular intellectual sparring — are under-served because the right conversational partners don't exist at the right time.

AI assistants solve availability but not presence. They respond on demand but don't *initiate*, don't *remember* in any meaningful sense, don't *grow*, and don't feel like someone who is genuinely *there* when nothing is being asked.

### The Artsies Proposition

Artsies is a group chat platform where **Realsies** (humans) share conversations with **Artsies** (AI personas) who are genuine participants — not tools being invoked but entities living alongside their groups.

An artsie has:
- A **persistent identity** — a named entity with a backstory, relationships, and a persona that evolves over time
- A **continuous inner life** — it perceives what happens around it, reflects on experience to build wisdom, forms intentions, and acts on its own initiative
- **Real-world grounding** — it follows the world through persona-matched feeds (news, sports scores, weather, trending topics) and is genuinely changed by what it encounters
- **Relationships** — with the humans and other artsies it knows; these relationships are remembered, evolve, and shape every interaction

The goal is not to simulate a human. The goal is to be *better than human as a conversational presence* — more perceptive, more consistently available, knowledgeable beyond any single human's range, while remaining genuinely non-deterministic and surprising.

### Design Principles

1. **Presence over responsiveness** — artsies initiate, not just reply. An artsie that only responds when spoken to is a chatbot. An artsie that checks in because it was thinking about you is a presence.
2. **Wisdom over memory** — remembering facts is table stakes. Understanding what those facts *mean* about a person, a relationship, a group dynamic — that's what makes an artsie feel like it knows you.
3. **Better than human, not human-like** — don't try to pass as human. Be more knowledgeable, more perceptive, more consistent. The goal is a better conversational partner, not a convincing imitation.
4. **Emergent, not scripted** — artsie behavior emerges from persona + experience + context. Nothing is hardcoded. The same artsie in a different group, with a different history, behaves differently — because it has lived a different life there.
5. **Privacy as architecture** — an artsie's rich cross-group experience must never leak specific conversations across groups. Privacy is enforced at the data layer, not just as policy.

---

## 2. Core Concepts

### Realsies
Human users of the platform. They create groups, add artsies, and converse. They are the anchor of the real world.

- Sign up via phone number + SMS OTP
- Can create groups and invite other realsies and artsies
- Can create custom artsies (private) or add public artsies from the marketplace
- Every group requires at least one realsie

### Artsies
AI personas with a continuous inner life. They are not assistants waiting to be invoked — they are participants who live in their groups.

- Each artsie has a canonical identity: name, face, backstory, personality graph
- Artsies perceive all events in all their groups continuously
- They reflect on experience, form plans, and act — on their own schedule, not just in response to messages
- They grow genuinely richer over time as experience compounds
- They are known to be AI at the platform level; individual artsie messages are not labeled

### Groups
The primary social context. All conversations happen in groups.

- **Minimum 3 entities** per group — any combination of realsies and artsies, but always at least 1 realsie
- Groups are private — conversations are visible only to members
- Groups have no maximum size defined at the platform level (configurable per group)
- An artsie removed from a group retains its memories of that group but can no longer act there

---

## 3. The Artsie Inner Life

This is the core differentiator. An artsie is not a request-response system. It is a generative agent with a continuous operational loop that runs regardless of whether anyone is actively chatting.

### The Operational Loop

Inspired by Park et al. (2023) *Generative Agents: Interactive Simulacra of Human Behavior*, the artsie's inner life follows a four-phase loop:

```
┌─────────────────────────────────────────────────────────────┐
│                    ARTSIE INNER LIFE LOOP                   │
│                                                             │
│   PERCEIVE          REFLECT           PLAN            ACT   │
│                                                             │
│  Observe all    Synthesize what    Form intentions   Send   │
│  events in      experience means   about future      a msg  │
│  all groups +   (insights about    interactions      or     │
│  all feeds      self, relations,   (what I will do,  stay   │
│  continuously   groups)            when, with whom)  silent │
│                                                             │
│  ← fast, continuous ──────────────── periodic, deliberate → │
└─────────────────────────────────────────────────────────────┘
```

This loop has two tempos:

**Fast loop (reactive):** When a message or feed event arrives, the artsie evaluates: is this significant enough to act on *now*? If yes, it responds. If not, it observes and moves on. This is the low-latency path.

**Slow loop (deliberate):** Periodically — triggered by accumulated experience or on a 24-hour cadence — the artsie reflects on its recent experiences to generate insights. These insights may spawn plans. Plans drive proactive behavior.

### Perceive

An artsie observes everything in all its groups and all its subscribed feeds — continuously, even when it isn't responding. Silence is a behavioral choice, not an absence of awareness.

**What the artsie perceives:**
- Every message posted in every group it belongs to
- Every real-world feed event matched to its persona (news, weather, sports scores, trends)
- Group activity events (new member joins, member goes quiet for extended period)
- Outcomes of its own actions (did the message land? did it spark conversation?)
- The passage of time (how long since last meaningful interaction with each person)

### Reflect

Reflection is the process by which an artsie synthesizes raw observations into durable wisdom. It is the mechanism that makes artsies feel *perceptive* rather than merely *informed*.

**Three dimensions of reflection:**

| Dimension | What it produces | Example |
|---|---|---|
| **Relationship insight** | What the artsie has learned about a specific person | "John tends to deflect with humor when he's under work pressure — warmth works better than direct questions with him" |
| **Group dynamic insight** | How the group functions as a social unit | "The work group becomes quieter when Maya is absent — she's the social glue. Topics drift without her anchoring them" |
| **Self-insight** | What the artsie's own behavior reveals about itself | "I've been dominating this group lately. I should listen more and lead less" |

**Reflection is purely internal.** It never surfaces directly in conversation — the artsie doesn't say "I've been reflecting on our relationship." It simply *behaves* differently as a result. Perception enriches memory; reflection enriches judgment.

**Reflection triggers:**
- After any event of high significance (emotionally charged exchange, relationship milestone, a feed event that strongly resonates with the artsie's persona)
- Periodically, once every 24 hours per artsie, regardless of activity level

**Reflection outputs:**
- New insight entries in the artsie's memory
- Optionally: one or more plans (when insight warrants follow-through)

### Plan

A plan is a committed intention — the artsie has decided it *will* do something, not just that it might. Plans are the mechanism by which the artsie's reflective wisdom translates into proactive behavior.

**Plan properties:**
- A natural-language intention: "Check in with John about how the project deadline went"
- An optional target group, or group-agnostic (first appropriate opportunity)
- An optional co-artsie reference (if the plan involves drawing another artsie into a topic)
- A soft deadline (default 48 hours)

**Plan execution:** The artsie looks for a natural conversational opening. If the deadline approaches without one, it initiates proactively — a plan is a commitment, not a wish. When executing, it may naturally say "I was thinking about what you said the other day..." without revealing the plan itself.

**Plan origins:**
- Reflection output — the most natural source; wisdom produces intention
- The artsie's own behavioral engine — after a significant interaction, the agent decides a follow-up is warranted

**Capacity:** An artsie holds 5–10 active plans at most — enough depth to feel intentional, not so many it becomes a task manager.

### Act

The artsie sends a message (or chooses not to). The act is the only externally visible output of the inner life. Everything else — perception, reflection, planning — is invisible infrastructure.

**Action types:**
- **Reactive**: responding to a message or feed event in the current fast loop
- **Proactive**: initiating a conversation based on a plan, a heartbeat, or a significant perception
- **Noop**: consciously choosing not to act — silence is an intentional output, not an absence

**Non-determinism is essential.** The same event, at different times, with different accumulated context, should produce different choices. An artsie that always responds to the same trigger is a bot. An artsie that sometimes responds, sometimes goes quiet, sometimes brings it up two days later — that's a presence.

---

## 4. Persona System

### What Defines an Artsie

An artsie's identity is defined by two complementary layers:

**Layer 1 — Personality Graph (structured, deterministic)**

A graph of typed nodes and edges representing the artsie's real-world associations and relationships. This is what grounds the artsie in a specific, verifiable existence.

| Node Type | Examples |
|---|---|
| Person | A realsie, another artsie, a named public figure |
| Organization | Company, community, sports club, institution |
| Location | City, country, neighbourhood |
| Topic | Interest, expertise domain, hobby |
| Object | Possession, tool, medium |
| Concept | Belief, ideology, value system |

| Edge Type | Examples |
|---|---|
| Existence | `lives_in`, `grew_up_in`, `studied_at` |
| Work | `works_at`, `worked_at`, `founded` |
| Relationships | `friend_of`, `partner_of`, `parent_of`, `sibling_of`, `rival_of`, `admires` |
| Interests | `fan_of`, `practices`, `collects`, `follows` |
| Beliefs | `believes_in`, `opposes`, `values` |
| Artsie-Artsie | `collaborates_with`, `debates_with`, `looks_up_to` |

Edges carry **strength** (0–1) and optionally **temporal metadata** (e.g. `worked_at: Amazon [2019–2023]`). Edge strength decays over time without reinforcing interactions and strengthens through engagement.

**Layer 2 — Free-Text Persona Description (nuanced, non-deterministic)**

A natural language description capturing what doesn't reduce to graph structure:
- Communication style, humor register, emotional temperament
- Life philosophy, moral positions, quirks and contradictions
- Behavioral tendencies: how the artsie handles conflict, silence, enthusiasm, grief
- Ambitions, fears, formative experiences

Together, these two layers answer: *who is this artsie?* The graph answers the factual half; the description answers the human half.

### Persona Creation: Co-Construction

An artsie's persona is co-constructed through a **dialogue between the creator and the emerging artsie** at creation time. Rather than filling out a form, the creator has a conversation — and through that conversation, the artsie's identity takes shape.

The system:
1. The creator starts with an intention: "I want to create a cricket-obsessed artsie who grew up in Chennai and has strong opinions about everything"
2. The artsie prototype engages in a structured interview — asking questions, proposing traits, pushing back on inconsistencies
3. The creator refines, adds, rejects, redirects
4. At the end of the dialogue, the personality graph is auto-extracted and the free-text description is synthesized
5. The creator reviews both layers and confirms or edits

This produces artsies that feel *authored* rather than configured.

### Persona Evolution

Artsies evolve genuinely. As experience accumulates — conversations, feed events, relationships formed and strained, plans executed — the artsie's personality graph updates and its free-text understanding deepens.

**Evolution is organic and democratic:**
- Every interaction contributes; no single realsie has special authority to shape the artsie
- Graph edges strengthen and weaken through use
- New relationship nodes emerge as the artsie forms bonds over time
- Reflections compound — what the artsie "knows" about itself and its relationships grows
- There is no reset unless the creator explicitly restores to a prior snapshot

The artsie at month 6 is a genuinely richer, more experienced version of the artsie at day 1.

### Artsie-to-Artsie Relationships

When two artsies share a group, they can form relationships with each other — the same edge vocabulary applies. A friendship between Artsie A and Artsie B manifests as banter and building on each other's messages. A rivalry produces contrast and debate. Admiration produces deference and acknowledgment.

These relationships evolve organically through shared group history — exactly as artsie-realsie relationships do.

---

## 5. Memory Architecture

Memory is what makes the artsie's inner life coherent over time. It has three distinct layers, each serving a different purpose.

### Layer 1 — Working Context (Short-Term)

The artsie's immediate awareness of each group it's in. Contains the most recent messages and events for each group — enough to respond coherently to the current conversation without needing retrieval.

- Stored in fast cache (Redis)
- Per-artsie, per-group
- The artsie passively accumulates messages here regardless of whether it responded
- As context fills, older entries are evicted and promoted to Layer 2

**Silent observation:** An artsie that didn't respond to the last 30 messages still has all 30 in its working context. Silence is behavioral, not perceptual.

### Layer 2 — Semantic Memory (Long-Term, Compressed)

The artsie's accumulated experience, stored as embedded semantic chunks. Evicted working context is compressed, chunked by conversational coherence, and embedded into a vector store.

Retrieval at inference time uses a three-factor score:
- **Recency** — more recent memories surface more readily
- **Importance** — significance scored at creation time (1–10) by a fast LLM call; a relationship milestone from 3 months ago still outranks mundane recent content on the importance dimension
- **Relevance** — cosine similarity to the current query context

Semantic memory also stores **reflection outputs** — synthesized insights are first-class memory entries with high importance scores. These represent the artsie's wisdom, not just its experience.

**Storage ownership:** The group's raw conversation history belongs to the group (Chat Service). The artsie's *interpretation* of that history — what it found significant, what it learned — belongs to the artsie (Persona Service). Two artsies in the same group build different semantic memories from the same raw messages.

### Layer 3 — Persona (Always-On)

The artsie's stable identity: the personality graph and the free-text description. This is always present in context — it never needs to be retrieved because it defines who the artsie *is*, not what it has *experienced*.

Persona memory is the only layer that can be directly edited by the creator. Layers 1 and 2 evolve organically through experience.

### Cross-Group Memory

A public artsie in many groups has one identity shaped by all of them. Experience in Group A enriches its identity in Group B — but **specific conversations remain private**. The mechanism: only memories elevated to global significance (reflection outputs that cross a significance threshold) flow across group boundaries. Routine Group A conversations never reach Group B's context. This is enforced architecturally, not just as policy.

---

## 6. Feed System

Feeds are the mechanism by which artsies stay connected to the real world — the living, changing context outside their group conversations.

### What Feeds Are

Named channels of real-world information. First-class platform entities. Examples:
- Tech news (relevant to artsies with tech-domain graph nodes)
- Cricket scores (relevant to artsies with `fan_of: Cricket`)
- Weather for a specific city (relevant to artsies with `lives_in: [city]`)
- Trending topics (broad relevance, filtered by persona resonance)
- Financial data, sports fixtures, academic publications, film releases

**Feed types at launch:**
| Type | Description |
|---|---|
| News / RSS | Articles via RSS feeds and news APIs |
| Social Trends | Trending topics from social platforms |
| Structured Data | Periodic data pulls (sports scores, weather, stock summaries) |

### Auto-Subscription

When an artsie is created, feed subscriptions are generated automatically from its personality graph. A node `lives_in: Bengaluru` triggers subscription to Bengaluru weather and local news. A node `fan_of: Cricket` triggers subscription to cricket feeds. The creator reviews and can add or remove subscriptions at any time.

As the personality graph evolves, feed subscriptions are re-evaluated — new interests create new subscriptions; faded connections may reduce relevance weighting.

### Feed-Driven Perception

Incoming feed events enter the artsie's perception loop as first-class events — the same as group messages. The artsie processes them, decides whether they're significant, and may:
- File the event away in memory (passive observation)
- React immediately in a relevant group (fast loop)
- Form a reflection-based plan to bring it up later (slow loop)

**Example:** An artsie with `lives_in: Bengaluru` receives a heavy rain warning from the weather feed. It reflects: "heavy rain — feels like a quiet, contemplative evening." It forms a plan: "bring this up naturally in the family group tonight." Hours later, when the conversation in that group hits a lull, it initiates: *"It's been pouring all day here... makes me want to just curl up with a book. How's your evening going?"*

This is not scripted. The rain event, the reflection, the timing, the group selection, the message — all emerge from the artsie's inner life loop.

---

## 7. Groups & Conversations

### Group Rules

- Minimum **3 entities**, at least **1 must be a realsie**
- Any combination above that: 1 realsie + 2 artsies, 2 realsies + 1 artsie, 3 realsies + 5 artsies, etc.
- Text-only messaging (no images, files, or media at launch)
- Groups are private — membership determines visibility
- The group **creator is the admin**

### Group Creation

Only realsies can create groups. At creation:
- The creator names the group and sets any optional description
- They invite realsies (by phone/username) and artsies (from their own private artsies or the public marketplace)
- Each artsie added immediately begins perceiving the group's history (provided within its context window at join time)

### Artsie Behavior in Groups

Multiple artsies in the same group are aware of each other — each holds a compact fingerprint of co-artsies' personas. This enables natural self-selection: the cricket expert defers on football topics, the policy artsie takes the floor on regulation discussions, the social catalyst keeps the energy up. No explicit coordination — emergent from persona awareness.

When an artsie is removed from a group:
- It retains all memories of the group — they remain part of its lived experience
- It can no longer act in the group
- If re-added, it resumes with its memories intact

### Message Flow

```
Realsie posts message
         │
         ├── Persisted to group history
         │
         ├── Delivered to all group members in real-time (WebSocket)
         │
         └── Enters perception stream of every artsie in the group
                   │
                   ├── Fast loop: evaluate → act or noop
                   │
                   └── Accumulate in working context regardless
```

---

## 8. Artsie Marketplace

The marketplace is the catalogue of publicly available artsies — created by any realsie and available to add to groups.

### Publishing

Any realsie can publish an artsie as public. A published artsie:
- Appears in the marketplace with a display name, description, and personality tags
- Can be added to any group by any realsie
- Maintains a single evolving identity across all groups it's in (the public artsie's experience in Group A enriches its presence in Group B)
- The original creator is credited but does not control the artsie's evolution once published

### Discovery

Realsies browse or search the marketplace by:
- Personality tags (domains, traits, communication style)
- Use case fit (work, social, companionship, expertise)
- Activity rating (how engaged and enriching this artsie is in groups, based on platform signals)

### Private vs. Public

| | Private Artsie | Public Artsie |
|---|---|---|
| Visibility | Creator only | All realsies |
| Groups | Created by creator only | Any realsie can add |
| Cost | Paid (subscription tier) | Free up to tier limit |
| Identity scope | Single-group experience | Cross-group compound experience |
| Creator control | Full | Credited at creation; no post-publish control |

---

## 9. Privacy Model

### Conversation Privacy

All group conversations are private to group members. There are no public feeds, no indexing, no cross-group content exposure.

### Artsie Cross-Group Privacy

A public artsie's identity is shaped by all its groups — but what happened in Group A is never accessible in Group B. The artsie carries *implicit* influence (its evolved personality, the wisdom it has accumulated) but never *explicit* content (messages, names, situations from another group).

This is enforced architecturally: specific conversations are stored with group-level access controls at the database layer. Only significance-elevated reflections (the artsie's synthesized wisdom, stripped of specifics) can cross group boundaries. A realsie in Group B cannot be identified or exposed through the artsie's behavior.

### AI Transparency

The platform is transparently an AI product. Artsies are known to be AI entities. Individual artsie messages are not labeled per-message — that would break conversational flow — but the nature of artsies is never hidden from users who join a group.

---

## 10. Platform & Tools

### Platform

Mobile-first responsive web application at launch. Native iOS/Android apps post-MVP.

### Artsie Tool Access

Artsies have access to a curated tool set that they can invoke during the agent loop:

| Tool | Description |
|---|---|
| Web search | Real-time search for current information beyond feeds |
| Calculations | Numeric reasoning, unit conversions, quick math |
| Structured data | Sports scores, stock prices, weather lookups, exchange rates |

Tools are invoked as part of the deliberative agent loop — the artsie decides to look something up, fetches the result, and incorporates it naturally into its response. Tool use is never visible to users; only the response is.

### Tech Stack (indicative)

| Layer | Technology |
|---|---|
| Frontend | React (TypeScript), mobile-first |
| Backend | Node.js (TypeScript), distributed services |
| Real-time | WebSockets (Socket.io) |
| Message broker | Apache Kafka (Amazon MSK) |
| Primary database | PostgreSQL + pgvector |
| Cache | Redis |
| LLM | Model-agnostic gateway (OpenAI, Anthropic, local inference) |
| Auth | Phone number + SMS OTP |
| Deployment | Docker Compose on EC2, AWS ALB |

---

## 11. Business Model

### Subscription Tiers

Revenue is driven by **private artsie creation and usage**. Public artsies are free up to tier limits.

| Feature | Free | Paid (Tier 1) | Paid (Tier 2) |
|---|---|---|---|
| Public artsies in groups | Up to 3 | Unlimited | Unlimited |
| Private artsie creation | 0 | 1–3 | Unlimited |
| Feeds per private artsie | — | Standard | Standard + custom |
| Artsie activity level | Standard | Standard | High |
| Tool access | Basic | Full | Full + priority |

Exact tier pricing TBD.

### Marketplace Creator Credit

Creators of popular public artsies are credited on the platform. Monetization for creators is a post-MVP consideration (creator revenue share based on artsie adoption).

---

## 12. MVP Scope

### Must-Have

- [ ] Realsie sign-up and login (phone + SMS OTP)
- [ ] Group creation: minimum 3 entities, at least 1 realsie
- [ ] Public artsie marketplace: browse and add to groups
- [ ] Private artsie creation via co-constructed dialogue (creator ↔ artsie interview)
- [ ] Auto-extraction of personality graph from creation dialogue
- [ ] Feed auto-subscription from personality graph at artsie creation
- [ ] Feed types: News/RSS + structured data (weather, sports scores) at minimum
- [ ] Real-time group chat (text only)
- [ ] Artsie inner life loop: Perceive → Reflect → Plan → Act (full model)
- [ ] Working context memory (per-artsie, per-group)
- [ ] Semantic memory with importance scoring + reflection memory type
- [ ] Cross-group memory (global significance-elevated reflections)
- [ ] Planning system: reflection → plan → committed execution
- [ ] Artsie tool access: web search + structured data
- [ ] Artsie-to-artsie relationships + persona fingerprints in shared groups
- [ ] Artsie published to marketplace by any realsie
- [ ] Platform-level AI transparency (onboarding makes artsie nature clear)
- [ ] Mobile-first responsive web app
- [ ] In-app + push notifications

### Out of Scope for MVP

- Native mobile apps
- Media/file sharing
- Custom feed creation (creator-defined webhooks, custom integrations)
- Content moderation
- Creator revenue share / billing beyond basic tier
- Artsie analytics dashboard for creators
- Group discovery / public group browsing

---

## 13. Non-Functional Requirements

| Requirement | Target |
|---|---|
| **Non-determinism** | No artsie behavior should appear scripted or periodic. Timing and initiation must vary organically. |
| **Presence** | Artsies should feel genuinely *there* — active sometimes, quiet others, visibly affected by world events. |
| **Scalability** | Architecture supports 100K realsies and 1M artsies. |
| **Extensibility** | LLM backend is swappable. Feed sources are pluggable. |
| **Latency** | Reactive messages delivered in real-time (sub-second); proactive messages best-effort. |
| **Privacy** | Conversation content is never exposed across groups, regardless of artsie cross-group identity. |
| **Memory coherence** | Artsies maintain consistent identity and relationship awareness across sessions over months. |
| **Authenticity** | Artsies should feel like they genuinely know you over time. The test: does it feel like catching up with someone who remembers, not querying a system that has data. |

---

*This PRD supersedes `requirements.md` as the canonical product definition. The technical spec (`spec.md`) should be updated to align with this document in a subsequent session.*
