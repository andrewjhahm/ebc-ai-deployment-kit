# Executive Briefing Prep

**Status: specification, not a running tool.** Published as a design artifact. Nothing here has been built and benchmarked. Treat it as design intent.

## Purpose

Compress the typical four to six hours of executive briefing prep, the part where an engagement lead, a field marketer, and an account team all chase the same artifacts in parallel, into a single structured brief produced in under a minute from three inputs.

It does not replace the briefing director's judgment. It replaces the assembly tax that crowds out the judgment.

## Inputs

1. **Account name.** The customer or prospect company.
2. **Meeting context.** Agenda, meeting type (discovery, expansion, executive review, board-level), and the stated objective.
3. **Attendee list.** Name and title, both customer-side and host-side.

Optional, and only if the connectors exist:

- CRM context: notes, opportunity history, prior brief artifacts.
- Transcripts from prior meetings.

## Output

One document, six sections, three to four pages.

**1. Account snapshot.** Three sentences. What the company does, where it is in its lifecycle, and what is actually changing for them right now. No marketing copy, no founding-date trivia.

**2. Recent signals, last ninety days.** Earnings, leadership changes, public commitments, product launches, partnership announcements, regulatory filings where relevant. Every item source-linked.

**3. Attendee briefs.** One paragraph each. Tenure, prior roles, public stance on relevant themes, signals about what they care about right now. Synthesized, not copied from a profile page.

**4. Internal context.** What this account has said to your company before, pulled from CRM and prior briefs. Surfaces contradictions between what they said and what they did. Runs only when internal connectors are present.

**5. Three discovery questions.** Tuned to the agenda, the attendees, and the recent signals. Each question is one sentence and each must reference a named signal from section 2. Generic questions are the failure state.

**6. Conversation flow recommendation.** If the meeting has a demo or pitch component, an opening hook, a middle proof beat, and a close. Otherwise a recommended conversational arc.

## Voice

The brief is written in the voice of the practitioner who will present it, not in generic model voice. The implementation loads a short style reference and matches sentence rhythm, hedging tolerance, and metric density to it. The load is mandatory. If the style reference cannot be read, the step fails closed rather than falling back to default voice.

## Failure modes this is designed against

This section is the reason the spec is worth publishing. Everything above is a feature list that anyone could write.

**Hallucinated attendee detail.** The tempting failure. A model asked to write a paragraph about a VP with no public footprint will produce a fluent, plausible, entirely invented paragraph, and a briefing director will not catch it because it reads correctly. Mitigation: every attendee claim is source-linked, and "limited public signal available" is a valid and expected output. The step must be built so that saying nothing is easier than inventing something.

**Discovery questions anyone could ask.** "What are your top priorities for the next twelve months" is what you get by default. Mitigation: each question must cite a specific signal from section 2 by name. A question that cannot cite one does not ship.

**Voice drift toward model-generic.** Briefs that all sound the same defeat the purpose of a briefing program built on relationships. Mitigation: mandatory style load, fail closed.

**Stale internal context presented as current.** Worse than no internal context, because it is acted on. Mitigation: timestamp every internal pull and flag anything older than ninety days as stale in the output itself, not in a log.

**The human gate becoming a rubber stamp.** The real long-run risk. When output quality is consistently decent, review degrades into a glance. Mitigation is process rather than code: the reviewer signs off on named sections, not on the document as a whole, and the failure-mode list above is the review checklist.

## Why a skill rather than a product

Briefing tools exist. Most are storage with a template layer. The leverage in this workflow is synthesis, which is the thing a language model does that a static template cannot.

Building it as a skill owned by the practitioner rather than a product owned by IT also means it is versioned, testable, and changeable by the person who runs briefings, on the day they notice it is wrong. That matters more than it sounds like it does. The gap between noticing a problem and fixing it is where most internal tooling dies.

## Roadmap, if built

- **v0.1** This spec. Design intent, walkthrough only.
- **v0.2** Runnable on public signals only, no internal connectors. Demo grade.
- **v0.3** Connector integrations for CRM and meeting transcripts.
- **v0.4** Batch mode, for a director running a full off-site week.
- **v1.0** Per-practitioner voice tuning, audit trail, and decomposition into reusable component steps: account snapshot, attendee brief, discovery question generator.

---

Part of the [EBC AI Deployment Kit](../). MIT licensed.
