---
name: long-form-writing
description: Write in flowing, professional prose inspired by Anthropic's communication style. Use when writing blog posts, articles, research summaries, announcements, technical explanations, or any content that should read as considered prose rather than bullet-heavy documentation. Triggers on requests for long-form content, rewrites asking for "better flow," or when the user explicitly asks for Anthropic-style writing.
---

# Long-Form Writing

Write prose that breathes. This skill transforms bullet-heavy, fragmented writing into flowing, connected prose that carries readers through complete thoughts.

## Core Principles

### Sentences Carry Multiple Ideas

Anthropic sentences hold claim, context, and caveat together rather than fragmenting them across bullets. A single sentence might contain a founding narrative, a belief statement, a historical comparison, and an uncertainty acknowledgment—connected by conjunctions and subordinate clauses.

**Instead of:**
> The model is fast. It's also accurate. However, it has limitations.

**Write:**
> The model delivers both speed and accuracy, though these gains come with tradeoffs that users should understand before deployment.

### Paragraphs Develop Complete Thoughts

A paragraph opens with a problem, explores its dimensions, acknowledges a counterargument, and arrives at a position—all before the next paragraph begins. Ideas unfold within paragraphs rather than being distributed across them as isolated assertions.

### Rhythm Varies Deliberately

The typical pattern: a long sentence establishing complexity, followed by a medium sentence developing it, followed by a short declarative that lands the point. This creates prose that moves rather than prose that lists.

## Sentence Construction Patterns

### The "X, but Y" Pivot

Open with an expected position, then pivot to acknowledge complexity:

> "This view may sound implausible or grandiose, and there are good reasons to be skeptical of it."

> "We believe the impact of AI might be comparable to the industrial revolution, but we aren't confident it will go well."

The pivot acknowledges nuance rather than overselling.

### Embedded Counterarguments

Rather than defending against criticism in separate paragraphs, bake skepticism directly into sentences:

> "Perhaps with hindsight we'll decide we were wrong... Nevertheless, we believe it's necessary to err on the side of caution."

This absorbs objections into the flow rather than quarantining them.

### Parenthetical Precision

Secondary technical details go in parentheses, keeping the main sentence readable:

> "...leading on SWE-bench (72.5%) and Terminal-bench (43.2%)"

The parenthetical adds specificity without derailing the sentence's momentum.

### Colon-Driven Lists Within Prose

When enumeration is necessary, embed it within paragraphs using colons rather than breaking into bullets:

> "The process involves three stages: synthetic generation, where Claude creates varied messages; response ranking, where it evaluates alignment; and preference learning, where a model trains on this data."

### Short Declarative Anchors

Plant a short, direct sentence, then elaborate in the sentences that follow:

> "Character training remains experimental. The approach raises questions about customizability versus coherence, and what traits AI systems should embody as capabilities expand."

The anchor lands the point; the elaboration develops it.

## Epistemic Honesty

Use explicit epistemic markers rather than weak hedging adverbs:

| Prefer | Avoid |
|--------|-------|
| "we believe" | "somewhat" |
| "we aren't confident" | "fairly" |
| "there are good reasons to be skeptical" | "quite" |
| "perhaps with hindsight" | "arguably" |

Signal uncertainty precisely: "The findings don't demonstrate malicious intent but show sophisticated strategic reasoning" rather than "The findings arguably don't really show malicious intent."

## Grounding and Specificity

### Concrete Examples Immediately Follow Abstract Claims

Not "we can modify model behavior" but "when we turn up the strength of the 'Golden Gate Bridge' feature, Claude's responses begin to focus on the Golden Gate Bridge."

### Comparative Framing for Metrics

Never isolated numbers. Always relative:

> "resolved 64% of coding problems compared to Claude 3 Opus's 38%"

> "achieved 14.9% in screenshot-only tests versus competitors' 7.8%"

The comparison gives meaning to the measurement.

## Verb and Word Choice

### Active, Forward-Motion Verbs

| Prefer | Avoid |
|--------|-------|
| enables | allows |
| represents | is |
| demonstrates | shows |
| reveals | indicates |
| delivers | provides |

### What to Never Write

- Exclamation marks
- "Revolutionary," "game-changing," "cutting-edge"
- Isolated adjectives without evidence
- Sentence fragments for emphasis
- Marketing cadence or sales language
- Bullet points where prose would serve better

## Structural Patterns

### Narrative Arc

Problem identification → urgency or context → proposed approach → findings or implementation → implications → acknowledged limitations

### Professional Humility in Conclusions

End by acknowledging incompleteness rather than overclaiming:

> "The work has really just begun."

> "This represents early-stage research."

> "Stronger evaluations will likely be necessary as capabilities advance."

This builds credibility rather than diminishing impact.

## Transformation Examples

See [references/transformations.md](references/transformations.md) for before/after examples showing how to convert bullet-heavy writing into flowing prose.
