# StoryBrand Messaging Engine — 7-prompt ChatGPT workflow

A chained ChatGPT workflow that rewrites an entire brand's website
messaging through Donald Miller's StoryBrand framework in one
sitting. Source: thread by **@godofprompt** on X, 2026-05-25
([original](https://x.com/godofprompt/status/2058978178698531167)).

Posted as 7 sequential prompts. They're designed to be run in order
in a **single ChatGPT conversation** so each prompt can reference
"my previous message" — context accumulates. The brand context
captured by Prompt 1 carries through Prompts 2–7.

## The framework in one paragraph

StoryBrand (Donald Miller, 2017) reframes brand messaging as
**story structure**, not feature lists. Every customer is the
hero of a story; the brand is the *guide* who helps them solve a
problem. The hero wants something, faces an obstacle, meets a
guide who has empathy + authority, gets a plan, is called to
action, and either succeeds (transformation) or fails (stakes).
Each of the 7 prompts below operationalises one of those story
beats into website-ready copy.

---

## How to use this workflow

1. Open a **fresh ChatGPT conversation** (no other context).
2. Paste **Prompt 1**, filling in the two bracketed placeholders.
3. Paste **Prompt 2** verbatim — it references "previous message"
   so the model uses what came out of Prompt 1.
4. Continue with Prompts 3–7 in order. Each builds on accumulated
   context.
5. The final outputs collectively become a draft website rewrite:
   opening line, subheadlines, About-page copy, process steps,
   CTAs, stakes paragraph, and a success section.

What you can edit later: the placeholders in Prompt 1, and any
prompt where you want to bias toward a specific tone or industry.
The framework structure itself shouldn't be modified — the prompts
are calibrated to each StoryBrand beat.

---

## Prompt 1 — The Hero Identifier

**Purpose:** establish who the customer is and the single dominant
desire that drives their buying decision. This is the foundation
every downstream prompt builds on.

**Output you'll get:** one customer-as-hero opening line for the
website that names the desire without cleverness or brand-puffery.

```
I run [describe your business in one sentence].

My ideal customer is [describe them: role, situation, daily reality].

Using StoryBrand's Character principle, identify the ONE dominant
desire my customer has that my product connects to.

Not a list of wants. One survival-level desire that drives their
buying decision.

Then write a single opening line for my website that positions the
customer as the hero pursuing that desire. No cleverness. No brand
name in the headline. Just the desire, stated plainly.
```

---

## Prompt 2 — The Three-Layer Problem Mapper

**Purpose:** map the customer's problem across StoryBrand's three
layers — what they say out loud (external), what they actually feel
(internal), and what they sense is unjust about the situation
(philosophical).

**Output you'll get:** all three problem layers + three short
taglines (one per layer) you can test as subheadlines.

```
Using the brand context from my previous message, apply StoryBrand's
Problem framework.

Every customer problem has three layers:

→ External Problem: the tangible surface-level issue they can name out loud
→ Internal Problem: the frustration, self-doubt, or emotional struggle underneath
→ Philosophical Problem: the "it shouldn't be this way" injustice they feel

Map all three layers for my customer.

Then write three short taglines (one per layer) that I could test as
subheadlines on my homepage. The internal one should sting a little.
The philosophical one should feel like a rallying cry.
```

---

## Prompt 3 — The Empathy-Authority Positioner

**Purpose:** establish the brand as the *guide*, not the hero — the
core StoryBrand inversion. A guide earns trust via empathy ("I get
your pain") and authority ("I can fix it").

**Output you'll get:** one targeted empathy statement, three
authority proof points to display, and a 2-sentence About-page
guide introduction.

```
In StoryBrand, the brand is never the hero. The brand is the guide.

A guide earns trust two ways:
→ Empathy: 'I understand your pain'
→ Authority: 'I have the competence to solve it'

Using my business context, write:

1. One empathy statement that mirrors exactly what my customer feels
   (not generic sympathy, the specific frustration from the internal
   problem you identified)

2. Three authority proof points I should display. Choose from: stats,
   client logos, testimonials, certifications, years of experience,
   media mentions, or case study results.

3. A 2-sentence 'guide introduction' I could use on my About page that
   leads with empathy and closes with authority.
```

---

## Prompt 4 — The Clarity Plan Builder

**Purpose:** StoryBrand insists "confused customers don't buy." A
Plan removes confusion by laying out 3-4 simple steps from where
the customer is now to where they want to be.

**Output you'll get:** two plans — a Process Plan (how to work with
you) and an Agreement Plan (what you commit to that eliminates
their buying fears).

```
StoryBrand says confused customers don't buy.

A Plan removes confusion by giving the customer 3-4 simple steps
between where they are now and the result they want.

For my business, create two plan types:

→ Process Plan: 3 steps that explain HOW to work with me (e.g.,
  'Schedule a call > Get a custom strategy > Launch in 30 days')

→ Agreement Plan: 3-4 commitments I make to the customer that
  eliminate their biggest fears about buying (e.g., 'No long-term
  contracts')

Write both plans in plain language. Each step should be 5 words or
fewer. If a step needs more than 5 words, it's too complicated.
```

---

## Prompt 5 — The Action Trigger

**Purpose:** force the brand to explicitly call the customer to
action. StoryBrand defines two CTA types — direct (the ask) and
transitional (the value-first offer for the not-yet-ready buyer).

**Output you'll get:** one direct CTA, one transitional CTA concept
(usually a free resource), and a button placement strategy.

```
StoryBrand defines two types of CTAs:

→ Direct CTA: asks for the sale ('Buy Now,' 'Schedule a Call,'
  'Start Free Trial'). This must be obvious, repeated, and
  impossible to miss.

→ Transitional CTA: offers value first for customers not ready to
  buy ('Download the Free Guide,' 'Watch the Demo,' 'Take the
  Quiz').

For my business, write:

1. One direct CTA (3-5 words, action verb first)
2. One transitional CTA with a free resource concept that solves a
   small version of my customer's external problem
3. A button placement strategy: where each CTA should appear on my
   homepage and how many times

The direct CTA should feel confident, not desperate. The
transitional CTA should feel generous, not cheap.
```

---

## Prompt 6 — The Stakes Amplifier

**Purpose:** name the cost of inaction. StoryBrand argues that a
story without a villain is boring, and a brand without defined
failure is forgettable — so the messaging must articulate what the
customer loses if they don't act.

**Output you'll get:** a stakes paragraph that makes inaction feel
more expensive than buying, without slipping into fear-mongering.

```
Most brands skip this step. StoryBrand says that's a fatal mistake.

If there are no stakes, there's no urgency. A story without a
villain is boring. A brand without defined failure is forgettable.

For my customer, define:

→ What specific negative outcome happens if they do nothing?
→ What does their life look like in 6 months if they don't solve
  this problem?
→ What's the cost of inaction in dollars, time, or status?

Then write one 'stakes paragraph' I can add to my sales page. It
should make inaction feel more expensive than buying. Not
fear-mongering. Just honest consequence.
```

---

## Prompt 7 — The Transformation Close

**Purpose:** show the customer what winning looks like — not in
features or deliverables but as an identity shift. This is the
final story beat that lands the offer.

**Output you'll get:** answers to the three StoryBrand resolution
questions (Status / Story / Experience) and a Success-section
website draft.

```
The final piece of StoryBrand: show the customer what winning looks
like.

Not features. Not deliverables. The identity shift.

StoryBrand says the best brands answer three resolution questions:

→ What does the customer STATUS look like after? (How others see
  them)
→ What STORY can they tell about themselves now? (How they see
  themselves)
→ What EXPERIENCE changes in their daily life? (What feels
  different)

Answer all three for my customer.

Then write a 'Success section' for my website. Paint the after-state
so clearly that the reader sees themselves already living in it. Use
present tense. Use 'you.' Make it feel like a destination, not a
sales pitch.
```

---

## Why these 7 prompts in this order

The sequence maps 1:1 to StoryBrand's 7-part framework:

| # | Prompt | StoryBrand beat |
|---|---|---|
| 1 | Hero Identifier | A Character (the customer) |
| 2 | Three-Layer Problem Mapper | Has a Problem (3 layers) |
| 3 | Empathy-Authority Positioner | Meets a Guide (empathy + authority) |
| 4 | Clarity Plan Builder | Who gives them a Plan |
| 5 | Action Trigger | And calls them to Action |
| 6 | Stakes Amplifier | That helps them avoid Failure |
| 7 | Transformation Close | And ends in Success |

Run them out of order and the context-chaining breaks: Prompt 3's
empathy statement depends on the internal problem from Prompt 2,
which depends on the customer identification from Prompt 1, and so
on. The order isn't decorative.

## When this is the right workflow

**Good fit:**
- A landing page that's all features, no story
- Founders who can describe what they *do* but struggle to
  articulate what the customer *gets*
- A brand pivot or rebrand where messaging needs to be coherent
  across pages
- B2C and B2B services / SaaS where the buyer's identity matters

**Poor fit:**
- Pure transactional commodity products where "story" overcomplicates
  the buying decision
- Highly technical audiences who respond to specifications more than
  narrative
- Existing strong brand voice you don't want to dilute through a
  framework

## Provenance & licence

This document captures a public workflow shared by @godofprompt on
X. The 7 prompts themselves are reproduced verbatim from that
thread for personal-use reference. StoryBrand framework © Donald
Miller, *Building a StoryBrand* (HarperCollins Leadership, 2017).
This document is research notes, not a commercial product.
