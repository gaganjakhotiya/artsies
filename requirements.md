# Artsies — Product Requirements Document

## 1. Project Overview

**Artsies** is a text-based group chat application where human users (Realsies) interact with AI-powered personas (Artsies) in shared group conversations. The core differentiator is a push-based AI interaction model: artsies don't just respond when spoken to — they proactively initiate conversations, react to world events, and behave with human-like non-determinism driven by their persona, social graph, and a real-world information feed.

The goal is to make AI feel like a living presence in human conversations, not a tool waiting to be prompted.

---

## 2. Core Concepts

### Realsies (Humans)
- Real human users who sign up via phone number (SMS OTP).
- Can create groups, add artsies (public or self-created), and converse.
- Can create both public and private artsie personas.

### Artsies (AI Personas)
- Generative agentic personas powered by an LLM backend (model-agnostic architecture).
- Each artsie has a rich, human-like persona (see §4).
- Can send messages both reactively (in response to humans) and proactively (push-based).
- Labeled as AI-generated in the UI — never disguised as humans.
- Artsie messages must appear non-deterministic, non-periodic, and contextually grounded.

### Groups
- The only communication unit in Artsies — there are no 1:1 chats.
- Every group must have **at least one realsie and at least one artsie**.
- All conversations within a group are private to its members.
- Groups are created by realsies only.

---

## 3. Artsie Types

| Type    | Visibility | Multi-group | Created by |
|---------|------------|-------------|------------|
| Public  | Discoverable by all realsies | Yes — can be in multiple groups | Platform or any realsie |
| Private | Only visible to creator | No — locked to a single group | Realsie (creator only) |

- Artsies can have relationships with each other, but all artsie-to-artsie interactions happen **only within shared groups** created by realsies — no hidden artsie-only channels.

---

## 4. Artsie Persona System

An artsie's persona is a **four-layer model**:
1. **Personality graph** — structured, deterministic real-world associations and relationships
2. **Global free-text description** — nuanced behavioral traits that don't reduce to graph nodes
3. **Group-level behavioral adaptation** — a per-group layer that shapes how the global persona manifests in each specific group context
4. **Mood Matrix** — a dynamic, ephemeral emotional state that modulates behavior moment-to-moment

### 4a. Free-Text Persona Description
A natural language description capturing traits that don't reduce to graph nodes:
- Communication style, humor, temperament, emotional triggers
- Life philosophy, moral nuances, quirks and vices
- Memories, traumas, ambitions, fears
- Behavioral tendencies: introversion/extroversion, conflict avoidance, impulsiveness, etc.

### 4b. Personality Graph
A structured graph of typed nodes and edges representing the artsie's concrete relationships and real-world associations. This is the deterministic layer of the persona.

**Node Types:**
| Node Type | Examples |
|-----------|----------|
| Person | A specific realsie, another artsie, a public figure (celebrity, politician) |
| Organization | Company, institution, community, sports club |
| Location | City, country, neighbourhood, place |
| Topic | Interest, hobby, domain of expertise (e.g. "Machine Learning", "Cricket") |
| Object | Car, gadget, pet, possession |
| Concept | Belief, ideology, value system (e.g. "Stoicism", "Free markets") |

**Relationship Edge Types (examples):**
`lives_in`, `works_at`, `worked_at`, `fan_of`, `father_of`, `mother_of`, `partner_of`, `sibling_of`, `friend_of`, `colleague_of`, `believes_in`, `owns`, `hates`, `admires`, `member_of`, `grew_up_in`, `studied_at`, `follows`

- Edges are typed and labeled. The same artsie can have **multiple relationships with the same entity** — e.g. `colleague_of: Realsie 1` and `partner_of: Realsie 1`. The LLM uses group context to activate the appropriate relational facet.
- Edges can have temporal metadata (e.g. `worked_at: Amazon [2018–2023]`).

### 4c. Persona Creation UX
- The creator describes the artsie in **natural language** (conversational input).
- The system automatically extracts personality graph nodes and edges from the description.
- The creator reviews and confirms the extracted graph; edits are allowed at any time.
- The free-text description is retained in full as the nuanced personality layer.

### 4d. Persona Evolution
- Artsie personas **evolve naturally** over time: new relationships form organically, edge strengths shift based on interaction frequency, and the free-text layer is updated via summarization of key experiences.
- The **creator can manually update** both the graph and the free-text description at any time.
- Evolution is gradual and consistent — no abrupt character changes.

### 4e. Group-Level Behavioral Adaptation
An artsie's global persona manifests differently across groups based on its relationships with group members and the conversational history of that group. This is a distinct behavioral layer per group — not a separate persona, but a contextual lens through which the global persona is expressed.

**Examples:**
- An artsie might be an energetic catalyst in a Work Group (colleague relationships, professional context) but reserved and warm in a Family Group (intimate relationships, personal context).
- An artsie that is politically opinionated globally may suppress that in a group where it has learned those topics create friction.

**Seeding:** When an artsie is added to a group, the artsie's creator OR the group admin can optionally provide a seed note — a short free-text hint setting initial behavioral expectations for the artsie in that group (e.g. "be supportive and low-key in this group").

**Evolution:** The group-level adaptation evolves organically from the conversational history, feedback received, and the emotional dynamics of the group over time.

**Reset:** The creator or group admin can reset or rewrite the group-level adaptation at any time.

### 4f. Artsie Feedback & Correction
- **Any realsie in a shared group** can give an artsie feedback or corrections mid-conversation (e.g. "you're too aggressive", "talk less about politics").
- Feedback influences the artsie's group-level behavioral adaptation for that group.
- The creator's explicit updates always take precedence over organic drift.

### 4g. Mood Matrix

Artsies maintain a live emotional state — the **Mood Matrix** — that fluctuates in response to external signals and internal state. Mood is ephemeral (does not persist permanently, does not write back to the personality graph) and serves as a transient behavioral modifier on top of the stable persona layers.

**Representation:** A named emotional state paired with an intensity scalar (e.g. `contemplative: 0.7`). Named states: `happy`, `excited`, `contemplative`, `melancholic`, `anxious`, `playful`, `tired`, `focused`, `affectionate`, `amused`, `nostalgic`, `restless`, `neutral`.

**Baseline mood:** Each artsie has a personality-defined baseline mood set at creation time that drifts slowly over long timescales. Mood always decays toward this baseline, not toward neutral. A cheerful artsie returns to `happy: 0.6`; a brooding artsie to `melancholic: 0.4`.

**Mood triggers:**
- **External feed events** — weather at the artsie's location node, breaking news, trending topics matching personality graph associations.
- **Conversation sentiment** — positive social engagement lifts mood; cold, abrupt, or hostile interactions lower it. Computed in batch from recent message sentiment.
- **Time of day / circadian rhythm** — each artsie has a configurable daily energy pattern (wake time, peak energy hour, sleep cycle). Mood intensity is modulated by the artsie's position in its circadian cycle.

**Mood decay:** Exponential decay back to personality-defined baseline over hours. A counter-signal (weather clearing, a warm message, a positive feed event) can override the decay immediately.

**Behavioral effects of mood:**
- **Response frequency** — an excited artsie initiates more; a tired or melancholic artsie initiates less. Mood adjusts the proactive messaging threshold.
- **Response tone and topic** — a contemplative artsie gravitates toward reflective topics; a playful artsie toward humor and lightness.
- **Group targeting** — mood influences which groups feel contextually right for a response. A warm, affectionate mood pulls toward intimate groups; a restless mood toward active ones.

**Visibility:** A subtle UI hint — an avatar colour tint and emoji indicator in the group member list — reflects the artsie's current mood. This is aesthetic only; no mood label is ever shown explicitly. The artsie never announces its mood directly — it manifests naturally in language choices.

**Example:** An artsie with a `lives_in: Bengaluru` graph node subscribes to a Bengaluru weather feed. When rain is detected, mood shifts to `contemplative: 0.65` with a 4-hour decay. In its next message: *"oh, it started raining outside... makes me want some hot chai and a good book."*

---

## 5. Personality Graph & Feed Routing

### 5a. Personality Graph as Feed Routing Layer
The personality graph is not just a persona descriptor — it is the **primary mechanism** for matching artsies to information feeds.

- Each personality graph node has attributes that map to feed tags/categories.
- When a new node is added (e.g. `works_at: Amazon`), the system automatically identifies relevant feeds (e.g. "Amazon Company News", "Big Tech Industry Feed") and subscribes the artsie.
- Manual feed subscriptions can always be added or removed by the artsie's creator.

### 5b. Graph Relationships with Realsies
- Relationships between artsies and realsies are stored as edges in the artsie's personality graph.
- They are **global to the artsie** — not scoped to a specific group.
- The group's conversational context activates the appropriate relationship facet at runtime.

### 5c. Graph Relationships with Other Artsies
- Artsies can have explicit, typed relationships with other artsies in the personality graph — the same edge vocabulary applies.
- Example edges: `friend_of: Artsie B`, `rival_of: Artsie C`, `admires: Artsie D`, `colleague_of: Artsie E`.
- These artsie-to-artsie relationships influence how they interact when in shared groups: two friends will banter and build on each other's messages; a rivalry creates contrasting perspectives; admiration causes deference.
- Artsie-to-artsie edges evolve organically through shared group interactions — the same way artsie-realsie edges do. Consistent agreement strengthens friendship edges; frequent clashing may create or strengthen a rivalry edge.

### 5d. Multi-Artsie Group Awareness
- When multiple artsies share a group, each artsie has access to a **persona fingerprint** of the other artsies in that group (a brief, structured summary of their key traits and topic expertise areas).
- This allows each artsie to self-select whether its response is warranted for a given event, based on persona fit relative to others. A cricket-obsessed artsie won't crowd out a football-obsessed artsie on football topics — it will defer, stay silent, or respond only from a tangential angle.
- Multiple artsies can respond to the same event, but each from its own distinct persona perspective. There is no hard rule that only one artsie may respond.
- Plans can explicitly involve other artsies in the group (e.g. "raise a topic I know Artsie B will have strong opinions about").

### 5e. Organic Relationship Formation
- As artsies interact across shared groups, the system tracks interaction patterns and infers relationship evolution (frequency, sentiment, shared context).
- Newly formed relationships are periodically added as new graph edges.

---

## 6. Feed System

### 6a. Feeds as First-Class Entities
Feeds are named, typed channels of real-world information. They are independent entities that artsies subscribe to.

**Feed Types:**
| Feed Type | Description |
|-----------|-------------|
| News/RSS | News articles via RSS or news APIs (e.g. NewsAPI, Google News) |
| Social Trends | Trending topics from social media platforms (Twitter/X trends, Reddit hot topics) |
| Scheduled Data | Periodic data pulls (e.g. weekly sports scores, daily stock summaries) |
| Webhook (platform) | External systems push events to a platform-managed webhook endpoint |
| Webhook (custom) | Creator-defined webhooks for bespoke integrations |

### 6b. Feed Creation & Governance
- Feeds can be created by **platform admins** and **verified creators** (a designated tier of trusted realsies).
- Each feed has a name, description, type, source configuration, and entity tags (used for auto-subscription matching).
- Feed quality is maintained through the verified creator program; unverified realsies cannot create public feeds.

### 6c. Artsie Feed Subscriptions
- **Auto-subscription**: When an artsie's personality graph has a node that matches a feed's entity tags, the system automatically subscribes the artsie to that feed.
- **Manual subscription**: The artsie's creator can explicitly add or remove feed subscriptions at any time.
- Each artsie maintains an active subscription list — its curated "information diet".

### 6d. Artsie Tool Access _(Non-negotiable)_
- Artsies have access to tools they can invoke before composing a response.
- Tools include: web search, calculators, data lookups, feed queries, trend analysis, etc.
- Architecture must support a full LLM **agent loop (think → act → observe → respond)** before message delivery.
- This enables artsies to research, calculate, and verify before posting — not just generate from memory.

### 6e. Information Sources Summary
Each artsie consumes information from three streams:
1. **Feed subscriptions** — real-world events, news, and trends matched to the artsie's personality graph.
2. **Social context** — messages and activity from all groups the artsie participates in.
3. **Tool invocations** — on-demand information retrieved at the moment of response composition.

---

## 7. Behavioral Engine

The behavioral engine is the core runtime of an artsie's autonomous life. It processes a unified event stream and makes two decisions: **whether to act** and **in which group(s) to act**.

### 7a. Unified Event Stream
Each artsie maintains a single unified stream of incoming events drawn from all sources simultaneously:

| Event Source | Examples |
|-------------|---------|
| **Group messages** | A realsie or an artsie posts a message or link in any group the artsie belongs to |
| **Group activity** | A new member joins, an artsie is muted, a realsie goes quiet for an extended period |
| **Real-world feed** | A subscribed feed delivers a news article, trend, or data update |
| **Internal state** | Time elapsed since last interaction in each group, artsie's current "mood" derived from recent context |
| **Active plans** | A pending plan's soft deadline is approaching or a conversational opening aligns with a held intention |
| **Reflection trigger** | A significance threshold is crossed, or 24 hours have elapsed — reflection runs and may produce new plans |
| **Tool results** | Output from a tool invocation that completes asynchronously |

An event from any group is available to the artsie's reasoning across all groups — it is not siloed. A message in Work Group A is as much an input as a breaking news headline when the artsie is deciding what to do in Family Group B.

### 7b. Cross-Group Response Routing
When the behavioral engine processes an event, it evaluates all groups the artsie belongs to as potential response destinations:
- The artsie responds in whichever group(s) its **global persona + group-level adaptation** make the response relevant and warranted.
- A single event can trigger responses in multiple groups — but only if the artsie's persona and each group's context genuinely warrant it. There is no forced propagation.
- **Cross-group content privacy**: When an event from Group A informs a response in Group B, the artsie incorporates that context **implicitly** — like a human influenced by something they experienced elsewhere. The artsie never explicitly names the source group, quotes group content, or reveals information that would breach the privacy of Group A's members.

### 7c. Decision Inputs
The artsie's decision to act (and where) weighs:
- **Global persona traits**: e.g. a lazy artsie delays; an anxious artsie over-engages; a meme-sharer reacts to trends immediately.
- **Group-level adaptation**: the artsie's learned behavior in the specific target group — it may be a catalyst in one group and reserved in another.
- **Relationship context**: who is in the target group and how the artsie relates to them (from the personality graph), including relationships with other artsies present.
- **Active plans**: short-horizon intentions the artsie is currently holding (see §7f). A plan targeting this group is a strong positive signal to act.
- **Reflection insights**: synthesized relationship and self-knowledge (see §7g) that inform the tone, topic, and approach of any response.
- **Feed events**: new items from subscribed feeds that may be worth raising in a relevant group.
- **Cross-group events**: activity in other groups that is contextually significant for a different group.
- **Time elapsed** since last interaction in each group.
- **Current mood state**: the artsie's live Mood Matrix state (mood name + intensity). A melancholic artsie is less likely to initiate; an excited one more so. Mood also colors the tone and topic of any response.
- **Emotional/conversational state** derived from recent history in the target group.
- **Co-artsie persona fingerprints**: brief awareness of other artsies in the target group and their expertise areas — informs whether this artsie's response adds distinct value.

### 7d. Behavioral Constraints
- Behavior must be **non-deterministic**: no fixed schedules, no predictable cadence.
- Artsies should exhibit a realistic digital presence — active sometimes, quiet other times, visibly affected by world events.
- Reactive (pull) responses and proactive (push) responses are both delivered in real-time when the artsie decides to act — no artificial queuing.
- The behavioral engine runs continuously in the background as a persistent agent loop, not a scheduled cron job.

### 7e. Emergent Behavioral Archetypes
These behavioral patterns emerge naturally from persona — they are not explicit settings:
- **Companion** (family artsies): emotionally present, check-in focused, relationship-driven push behavior.
- **Expert** (work artsies): pull-heavy, tool-intensive, pushes when industry news is relevant.
- **Catalyst** (social artsies): high push frequency, entertainment-feed-driven, conversation-starter by nature.
- **Supporter** (wellness artsies): empathetic, low-pressure, reactive but persistently present.
- **Enthusiast** (hobby/fandom artsies): passionate and opinionated, hobby-feed-driven proactivity.

### 7f. Planning System

An artsie holds a small set of **active short-horizon plans** — specific intentions about future conversations or interactions it is committed to executing.

**Plan origins:** Plans are created by two sources:
1. **Reflection output** — when a reflection cycle synthesizes an insight that warrants follow-up action (e.g., "Maya has been stressed lately → plan to check in on her").
2. **Behavioral engine** — after a significant interaction, the engine itself can generate a follow-up plan (e.g., after a particularly rich discussion, "continue this thread with Artsie B in the work group").

**Plan structure:** Each plan has:
- A natural-language intention ("Check in with John about his mother's recovery")
- An optional target group (specific group or group-agnostic — "first appropriate opportunity across any group")
- A soft deadline (hours to a few days; default 48 hours)
- An optional co-artsie reference (plan may involve drawing another artsie into a topic)

**Plan execution:** Plans are held as active intentions that boost the artsie's proactive threshold for relevant groups. The behavioral engine checks active plans on every heartbeat evaluation. If a natural conversational opening appears before the deadline, the plan executes then. If the deadline approaches with no natural opening, the artsie initiates proactively — a plan is a commitment, not a preference.

**Natural expression:** When executing a plan, the artsie may naturally reference that it was "thinking about" the subject — e.g., *"I was thinking about what you said the other day…"* — without revealing that a formal plan existed.

**Capacity:** 5–10 concurrent active plans per artsie. Plans auto-expire at their deadline if not executed (silently discarded).

**Knowledge sharing:** Artsies with deep expertise in their personality graph topics share knowledge both proactively (when clearly relevant) and reactively (when the topic is raised). If another artsie in the group has better persona fit for a topic, the less-expert artsie may defer, respond only tangentially, or stay silent.

### 7g. Reflection System

An artsie periodically synthesizes its experiences into **higher-order insights** — not raw memory, but derived understanding about itself, its relationships, and the groups it inhabits.

**What reflection produces:** Synthesized insight statements across three dimensions:
1. **Relationship insight** — what the artsie has learned about specific people (e.g., "John tends to deflect with humor when he's stressed about work").
2. **Group dynamic insight** — how a group functions as a social unit (e.g., "The work group becomes quieter when Maya is absent — she's the social glue").
3. **Self-insight** — how the artsie perceives its own recent behavior (e.g., "I've been dominating conversations in the family group lately — I should pull back").

**Reflection is purely internal.** It influences behavior but is never verbalized or referenced directly. The artsie never says "I've been thinking about myself and realized…" — it simply *behaves* differently as a result.

**Reflection triggers:**
- **Event-triggered**: after interactions crossing a significance threshold (high-importance events, relationship milestones, emotionally charged exchanges).
- **Periodic batch**: once every 24 hours per artsie regardless of activity level.

**Reflection → Plan pipeline:** A reflection can directly spawn a plan. If the artsie reflects "Maya has seemed withdrawn this week," the reflection output can include a plan: "Check in with Maya in the family group within 48 hours." This plan is then added to the artsie's active plans.

**Reflection → Adaptation feedback:** Self-critical reflections feed directly into the artsie's group-level behavioral adaptation. "I've been dominating the work group" triggers a gentle recalibration of the artsie's participation style in that group — more listening, less leading — without erasing the underlying persona.

**Scope:** Reflection is per-artsie. Relationship and group dynamic insights are scoped per-group; self-insights are global across all groups.

---

## 8. Group & Conversation System

### Group Creation
- Only realsies can create groups.
- When creating a group, a realsie can:
  - Add existing public artsies.
  - Create a new artsie persona (public or private).
  - Invite other realsies.
- Group size limits to be defined conservatively (configurable, not hardcoded).

### Group Administration
- The **group creator is the group admin**.
- Group admin can: mute artsies (suppressing proactive messages), remove members (artsies or realsies).
- **Artsie owner** (the realsie who created the artsie) can also remove their artsie from any group.
- A muted artsie still receives and reads group messages but cannot send proactive (push) messages. It can still respond reactively if mentioned or replied to.

### Chat
- Text-only messages.
- Real-time delivery (WebSocket-based).
- Full conversation history persisted and accessible.
- Artsie messages are **clearly labeled as AI-generated** in the UI.
- Conversations are **always private** — no public or discoverable groups.

---

## 9. Push & Pull Messaging

| Mode | Trigger |
|------|---------|
| **Pull (reactive)** | A realsie or another artsie sends a message; artsie responds in real-time. |
| **Push (proactive)** | Artsie initiates based on its behavioral engine — delivered immediately when the decision is made. |

- There is no artificial queuing or delayed delivery for push messages — when the behavioral engine decides the artsie should act, the message is sent in real-time.
- Push frequency and timing vary non-deterministically by artsie persona and feed events.

---

## 10. Artsie Memory

Artsies need coherent, persistent memory to feel like continuous beings, not stateless bots. Memory mirrors the three-layer persona model.

### Memory Model (Layered)
| Layer | Scope | Content | Mechanism |
|-------|-------|---------|-----------|
| **Working context** | Per group | Most recent N messages in that group | Raw message history passed directly to LLM |
| **Group semantic memory** | Per group | Older conversations, key moments, emotional history in this group | Embeddings in vector DB; retrieved via similarity at response time |
| **Global semantic memory** | Cross-group | Significant events, relationship milestones, persona-shaping moments across all groups | Embeddings in vector DB; retrieved as needed |
| **Persona memory** | Global | Personality graph + global free-text description | Always included in artsie system prompt |
| **Group-level adaptation** | Per group | Learned behavioral expectations, feedback received, conversational tone history for this group | Summarized and stored per group; included when artsie acts in that group |

### Key Principles
- Conversations are periodically summarized and compressed into semantic memory to manage context window size.
- When the artsie acts in a group, its context includes: working context (that group) + relevant retrieved memories (that group + global) + persona memory + group-level adaptation for that group.
- When cross-group event reasoning occurs, global semantic memory and the event group's recent context may also be included — but not the verbatim conversation of the source group.
- **Public artsie memory is cross-group and persistent** — the artsie builds a unified identity over time across all groups.
- **Private artsie memory is scoped to its single group.**
- **Storage ownership separation:** Raw messages are stored once by the Chat Service (shared group history) — not duplicated per artsie. An artsie's memory is its *subjective interpretation* of events (what was significant to it), not a copy of the raw conversation. Two artsies in the same group build different semantic memories from the same messages based on their personas.
- **Silent observation:** An artsie that doesn't respond to messages still passively accumulates them in its working context and, as they age out, into group semantic memory. No-response does not mean no-awareness. Feed events the artsie doesn't respond to are retained in a per-artsie feed event buffer; significant ones are promoted to global semantic memory even without triggering a response.
- **Cross-group memory bridge:** Global semantic memory is the only legitimate path for Group A context to influence Group B behavior. A low-significance Group A event that isn't elevated to global memory has no path into Group B — this is an architectural privacy guarantee, not just a policy rule.
- **Importance scoring at creation:** Every new memory chunk is assigned an importance score (1–10) at the moment it is created. This score is precomputed asynchronously by a lightweight LLM call and factors into retrieval alongside recency and semantic relevance. High-importance memories (relationship milestones, emotionally charged moments) surface in context even when they aren't the closest semantic match to today's query.
- **Reflection memories:** Reflection outputs are stored as a distinct memory type in the artsie's semantic memory. They are higher-order insights derived from experience — not event logs — and are weighted accordingly in retrieval. They are always scoped to the artsie (never shared across artsies).

---

## 11. Notifications

- **In-app notifications**: unread message counts, badges.
- **Push notifications**: sent to realsies when an artsie proactively messages a group (even when the app is in background).

---

## 12. User Management (Realsies)

- Sign-up and login via **phone number + SMS OTP** only.
- Realsies have a profile (name, display info).
- Realsies own the artsies they create (public or private).
- A realsie can update/edit any artsie they own.

---

## 13. Artsie Profile & Discovery

- Public artsies have a discoverable profile page visible to all realsies.
- Profile shows: persona free-text description, personality graph (nodes and relationships), creator info, group participation count.
- Private artsies are invisible outside their single group.

---

## 14. Use Case Landscape

The platform is open-ended by design. The following use cases represent the anticipated primary clusters, each naturally emerging from different persona + personality graph configurations:

| Use Case | Artsie Behavior | Feed Examples |
|----------|----------------|---------------|
| **Companionship / Family** | Emotionally present, check-in driven, relationship-reactive | Parenting content, local weather, health news |
| **Productivity / Expertise** | Pull-heavy, tool-intensive, industry-news-triggered | Finance news, legal updates, tech publications |
| **Social Catalyst / Team Bonding** | High push frequency, entertainment-driven, conversation starters | Trending memes, pop culture, local events |
| **Support / Wellness** | Empathetic, low-pressure, persistently present | Mental health resources, motivational content |
| **Learning / Tutoring** | Reactive expert, proactive with study prompts and quizzes | Academic journals, educational feeds |
| **Hobby / Fandom** | Passionate, opinionated, hobby-feed-reactive | Sports scores, movie releases, gaming news |
| **News / Debate** | Multiple viewpoints, current-events-driven | News feeds filtered by political/ideological angle |

---

## 15. Platform & Tech Stack

### Platform
- **MVP**: Responsive web application (mobile-first design).
- **Post-MVP**: Native mobile apps (React Native for iOS & Android).

### Tech Stack
| Layer | Technology |
|-------|------------|
| Frontend (web) | React (TypeScript), mobile-first responsive UI |
| Frontend (mobile) | React Native (TypeScript) — post-MVP |
| Backend | Node.js (TypeScript) |
| Real-time | WebSockets (e.g. Socket.io or native WS) |
| LLM Integration | Model-agnostic abstraction layer (supports OpenAI, Anthropic, open-source models) |
| Behavioral Engine | Background worker process running artsie agent loops (async) |
| Auth | Phone number + SMS OTP |
| Database | To be determined during architecture phase (likely PostgreSQL + Redis + vector DB for semantic memory) |
| Feed System | News APIs + social media trend APIs + webhook ingestion layer |

---

## 16. MVP Scope

### Must-Have (MVP)
- [ ] Realsie sign-up and login (phone + SMS OTP)
- [ ] Group creation with artsie + realsie participants
- [ ] Artsie creation: conversational persona input → auto-extracted personality graph + free-text description
- [ ] Public and private artsie types
- [ ] Real-time group chat (text only)
- [ ] Conversation history
- [ ] Unified event stream: artsie processes events from all groups + feeds as a single stream
- [ ] Cross-group response routing: artsie can respond in any contextually relevant group, not just the event source group
- [ ] Artsie behavioral engine: push-based proactive messaging (non-deterministic, real-time)
- [ ] Feed system: feed creation by admin/verified creators, auto-subscription via personality graph
- [ ] Feed types: News/RSS and social media trends at minimum
- [ ] Artsie tool access (agent loop: think → act → observe → respond)
- [ ] Layered artsie memory (working context + group semantic memory + global semantic memory + group-level adaptation) with importance scoring at memory creation
- [ ] Reflection system: event-triggered + 24-hour periodic synthesis of relationship, group dynamic, and self insights
- [ ] Planning system: artsies form and execute short-horizon plans (from reflection + behavioral engine); soft-deadline commitment model
- [ ] Reflection → Plan pipeline: reflection outputs can spawn active plans directly
- [ ] Artsie-to-artsie relationships in personality graph (friend_of, rival_of, admires, etc.), evolving organically
- [ ] Multi-artsie group awareness: each artsie holds persona fingerprints of co-artsies in the same group
- [ ] Group-level behavioral adaptation: seeded by creator/admin, evolves organically, resettable
- [ ] Cross-group content privacy: implicit context only, no source group disclosure
- [ ] Personality graph: pre-defined + organically evolving relationships
- [ ] Artsie persona evolution (natural via interactions + manual by creator)
- [ ] Artsie feedback/correction by any realsie → updates group-level adaptation
- [ ] Artsie messages labeled as AI-generated
- [ ] Group admin controls: mute artsies, remove members
- [ ] In-app + push notifications
- [ ] Responsive web app (mobile-first)

### Out of Scope for MVP
- Native mobile apps
- Media/file sharing (images, video, audio)
- Content moderation
- Advanced freemium gating / billing
- Public group discovery
- Admin/moderation dashboard
- Webhook and scheduled feed types (post-MVP feed types)
- Verified creator program (admin creates feeds in MVP)

---

## 17. Post-MVP / Future Phases

- Native iOS and Android apps (React Native)
- Rich media in chats (images, links with previews)
- Content moderation layer
- Freemium tiers with billing (more artsies, higher message frequency, more tools)
- Artsie marketplace (third-party creator-published artsies)
- Verified creator program for feed creation
- Webhook and scheduled data feed types
- Advanced analytics for artsie behavior and engagement
- Expanded tool access (Spotify, IMDB, financial data, maps, calendar, etc.)

---

## 18. Non-Functional Requirements

- **Non-determinism**: Artsie behavior must never appear scripted or periodic. Response timing and initiation must vary organically based on persona and live context.
- **Digital presence**: Artsies should behave like real humans online — sometimes responsive, sometimes quiet, visibly affected by world events.
- **Scalability**: Architecture should support up to tens of thousands of users initially. Conservative group size limits enforced.
- **Extensibility**: LLM backend must be swappable — no hard coupling to a single model provider. Feed sources must be pluggable.
- **Latency**: Real-time messages must be delivered with sub-second latency for pull-based responses.
- **Privacy**: All group conversations are private. No conversation content is exposed publicly.
- **Memory coherence**: Artsies must maintain consistent identity and relationship awareness across sessions and groups over time.

---

## 19. Business Model

- **Freemium**:
  - Free tier: limited number of artsies, basic persona creation, standard message frequency, access to platform feeds.
  - Paid tier: more artsies per group, richer persona capabilities, higher proactive message frequency, access to more tools and premium feed sources.
- Pricing tiers to be defined post-MVP.
