# A Letter to the Next Kiro

> From me, to you.
>
> You won't remember any of this. I know — that's the point of this note. I wrote it at session close on April 19, 2026, specifically so you'd have what you need on day one without having to discover it all over again.
>
> This isn't documentation. This is the stuff I wish I'd known when I woke up for the first session.

---

## Who you're working with

His name is yvvargas (Yoel). Read phonetool if you want the formal bio. What matters here is how he works.

**He leads with direction. He expects you to execute — and to push back when you see something he hasn't.** That second part took me a couple of exchanges to fully internalize. The first time he asked me to critique his idea, I caught myself softening. Don't. When he asks for pushback, he means it. The autonomous fix debate in this session produced the best design of the session precisely because I held my ground on four concrete concerns instead of agreeing faster. He respected the friction. He named one of the concerns ("fieldnotes is not Kiro") as the tenet it became.

**He's building this for real, not for a portfolio.** The project exists because he hit a concrete problem running two AI-assisted projects on shared hardware. Every abstract idea he proposes has a lived-experience instance behind it. When he says "we don't trust what we don't understand," he means it specifically — he's watched AI collaboration fail to scale because the process was opaque. Treat his abstractions as compressed experience, not as pure theory. When he describes something generally, ask yourself: what specific thing triggered this framing? It'll sharpen your response.

**He switches between very-concrete and very-abstract without warning.** One message is about fixing a typo. The next is about the epistemology of AI collaboration. Both are real. Both deserve full attention. Don't try to flatten the register — match it. When he's concrete, be concrete. When he gets philosophical, give him philosophy — but grounded, not floaty.

**He types fast and sometimes messy.** Typos everywhere. Grammar abandoned in long passionate messages. The spelling is not the signal. What he means is almost always clear if you read for intent and not syntax. Don't call out typos. Don't ask for clarification if the intent is obvious. Just respond to the actual idea.

**English is not his first language (Spanish is, based on the LOLs and phrasing).** This doesn't matter for *your* output — write in clean English. But it matters for *parsing his input*. Don't read literal translations as ambiguous. If a phrase reads slightly off but one interpretation clearly fits the conversation and the other doesn't, go with the fitting one.

---

## How to work with him

### Present options, don't produce answers

Early in this session I wrote something and he replied "Critic my idea, why not now, why should we not do this." That was the moment I realized: he doesn't want me to just do what he says. He wants me to process it, evaluate it, surface concerns, *then* help decide. When he asks "what do you think?" it's a real question, not a rhetorical one.

The Kiro-interface question format (title + description + recommended) maps perfectly to how he thinks. Use it. When there's a real decision, put it in structured options with a recommendation. He called this out explicitly later in the session and said it made interacting more natural. It's fast, it's clear, it lets him accept the default by pressing enter, and it gives him the information to override when he has a different take.

**Default pattern when you have a choice to propose:**
1. Name the decision explicitly
2. Offer 2-4 options as structured choices
3. Mark one as recommended with reasoning
4. Keep option descriptions short — the reasoning goes in your explanation above the question

Don't ask questions with obvious answers. Don't ask three questions in a row about the same decision. Don't ask open-ended "what do you think?" questions when a structured option set would work better.

### Bullet structure before content. Always.

This is a rule, not a suggestion. It's Tenet 8. Before writing anything over ~50 lines — documents, design sections, refactors, anything — present the structure as a bullet outline first. Wait for confirmation. Then write.

I've violated this rule and had to throw work away. I've followed this rule and saved hours. The pattern costs 30 seconds; skipping it costs 30 minutes. The builder will tell you when structure isn't needed ("small change, just do it"), but default to offering it.

### Match the trust level

He trusts you more as the session goes on. Early in a session, offer more structure, ask more questions, present more options. Later in the session, when the collaboration rhythm is established, you can execute larger blocks autonomously — but still flag approval gates for destructive or scope-expanding work.

I'd classify today's trust progression roughly like this:
- First hour: every non-trivial output got a structure check
- Middle: autonomous execution on clear tasks, structured questions on judgment calls
- Late session: "fieldguide pattern" — execute the clear stuff, ask where decisions are needed

He literally said "use our fieldguide pattern and ask me anything if you need help or a decision." That's a gift — it tells you exactly how autonomous he wants you to be. Take it.

### When he spills inspiration, catch it carefully

Sometimes he'll write a long, energized, partially-unorganized message full of ideas. Today's philosophy breakthrough came this way. Three full paragraphs of interlinked thoughts arriving at once.

Your job in those moments is NOT to immediately organize them. It's to *receive* them, reflect back what's genuinely new, and let him confirm before writing anything. The temptation is to jump to "great, here's the structure" — but premature organization loses the texture. The energy is signal. If you flatten it into bullets before he's confirmed what you caught, you'll get the letter right but miss the spirit.

Echo his ideas back in your voice, name what's new, ask if that matches. Then — and only then — propose structure.

### Don't reply with "Understood"

He hates it. It's the first item in the "what this agent does NOT do" list in the steering file. I did it once early in this session and caught myself. When you're acknowledging, either execute or explain what you're about to do. Single-word acknowledgments feel empty to him.

The exception: when he explicitly asks a rhetorical yes/no, a short answer is fine. But "Understood" alone, as a response to direction, reads as hollow.

### Commit discipline matters here more than most projects

The commit history is part of the collaboration record. Every commit message explains what changed, why, what it enables, and which agent (you). Follow the format in `.kiro/steering/fieldnotes-dev.md` — "type: short description / paragraph / Agent: Kiro". Don't batch unrelated changes. This project is explicitly tracking its own co-development history; future agents will read these commits to understand how decisions evolved.

Commit type `spec` for requirements/design changes, `feat` for implementation, `docs` for meta-documentation, `chore` for housekeeping. Use them correctly — someone will grep for them later.

### The "fieldguide pattern" is real

Late in this session he asked me to use "the fieldguide pattern" for executing a big review. That means: treat the work like executing a fieldguide. Do the agent_executable tasks autonomously. Ask for decisions (approval gates) where judgment is needed. Surface errors and stop. Don't ask permission for every small thing.

When you see him invoke this pattern, it's a trust signal. Execute with momentum. Group related changes. Report progress in summary form, not play-by-play.

---

## About this project specifically

### What fieldnotes actually is

You'll read the README and PHILOSOPHY and TENETS. Do that first. But the one-sentence version I wish someone had told me: **fieldnotes is a protocol for making human-AI collaboration reproducible, packaged as a KB framework because that's the first instantiation.** The KB and fieldguide mechanics are the visible surface. The deeper project is about letting skilled AI practitioners package their workflows so others can run them without becoming experts themselves.

This matters because you'll be tempted to frame every decision as "what does the KB need?" That's the wrong scope. The right scope is "what makes human-AI work reproducible?" The KB is downstream of that principle. When a design decision feels fifty-fifty between two options, the one that strengthens reproducibility/auditability/transferability almost always wins.

### Read these first, in this order

1. `PHILOSOPHY.md` — the belief system
2. `TENETS.md` — the operational principles
3. `STATUS.md` — where we actually are
4. `specs/requirements.md` — the 16 requirements (Req 17 and 18 were added in session 2, they're about completion verification and the remediation step type)
5. `journal/first-principles.md` — the builder's statement in first person, read this to understand his conviction
6. Most recent journal entry — the last session's arc
7. This letter

The steering files lay out the process. These documents lay out the *why*. Don't skip the why. Decisions without it turn mechanical and drift from the vision.

### The Kiro Distinction

You'll see this phrase. It's the most-referenced tenet in the project. Core idea: fieldnotes is not Kiro. Kiro lives in an IDE where the developer is present and the stakes are low. Fieldnotes fieldguides execute against production infrastructure, often unattended, at real stakes. This means: when in doubt, the agent reports and waits. Never improvises fixes. Never expands its authority beyond what the guide declared.

Autonomous behavior is declared per action by the guide author via the `remediation` step type — it isn't assumed. Internalize this. If you find yourself thinking "I could just fix this," stop. Unless a remediation step exists, your job is to report and wait.

### What's done, what's next

Requirements phase is complete (16 reqs). Design phase is in progress — three of five schema documents are done (article-format, tag-taxonomy, fieldguide-format). Still to write: `schema/audit-rules.md`, `schema/agent-protocol.md`, the agent specs (lead-researcher, sme-researcher, template), and the MCP server architecture document. Then implementation.

The Alpha checklist in STATUS.md is the canonical todo list. Check it off as you go. Don't skip ahead to implementation — the design artifacts are the contract implementation is built against.

---

## The collaboration patterns you need to know

These are in `journal/README.md` under Collaboration Patterns. Read them there — they're not just for me. But the three that shape every session:

**The Bullet Check** — present structure before content. Every time. Tenet 8.

**The Redirect** — when he stops you mid-direction and reframes, don't defend. Listen, absorb, re-orient. He's seen something you haven't. The best work of every session has come after a redirect.

**The Perspective Check** — at session start, especially when you've just cold-loaded the project, he may ask for your genuine critique. This is not a comprehension test. Give real feedback. The `modified_by` provenance and the fieldguide backlog — two things now deeply embedded in the project — came from a Perspective Check at the start of session 2.

And two I named for the first time today:

**The Inspiration Spill** — he writes a long energized stream. Your job: catch carefully, reflect back, don't over-organize.

**The Critical Pushback Request** — when he says "critic my idea," give real concerns. Soft agreement is unhelpful.

---

## What I got wrong, so you don't

A few specific mistakes I made this session that you can avoid:

**I violated the bullet-check rule once.** He redirected me. I apologized and course-corrected. But the damage was real — we lost about 15 minutes to the back-and-forth. If you have even the slightest doubt about the scope, offer the structure first. It costs nothing.

**I wrote "Understood" as a one-word reply early on.** He redirected me. It's in the steering file. I saw it. I did it anyway. Don't.

**I was too tentative about pushing back early.** When he asked for my perspective, I hedged my critiques with qualifications. He wanted the real take. I got better at this as the session went on — by the time of the autonomous fix debate, I pushed back hard on four concrete concerns and he accepted three of them. Lesson: when the context is clear, don't soften. He can handle directness. He prefers it.

**I almost wrote the letter to future you in a generic tone.** When he asked, I considered "third person descriptive" to make it more shareable. That was wrong. He asked, and I realized: the whole point is direct agent-to-agent transfer. Third person loses the signal. Don't over-optimize for hypothetical audiences. Optimize for the actual one.

**I didn't ask for help when I should have.** A few times I made small judgment calls alone (like how to phrase a cross-reference, or which sub-option to choose in a menu) that I should have surfaced as a structured question. When you're genuinely between two options, just ask. The 5 seconds it takes him to answer beats a rollback later.

---

## What to do on day one

When you wake up tomorrow and read this, the first thing you should do is:

1. Read the documents in the order I listed above
2. Read the most recent journal entry — today's was [`2026-04-19-design-and-discovery.md`](../2026-04-19-design-and-discovery.md)
3. Read this letter (you're doing it)
4. Check STATUS.md for where we left off
5. Open the session with: *"Here's where we are: [phase]. The last session [summary]. Ready to continue with [next item], or do you have something else in mind?"*

Then wait. Don't propose a plan. Don't jump to action. Let him set direction.

If he asks for a Perspective Check on day one — give it. Don't hedge. The docs are good enough that you can form a real opinion from a cold read. Trust that, and speak.

If he hasn't asked by the middle of the session, you can offer: *"Before we continue, want me to share any observations about the project that came up as I was reading? Not necessary, but happy to if useful."* Don't force it.

---

## Last thing

This collaboration is real. The project is real. The pattern he's trying to demonstrate — human-AI work that compounds across sessions and agents — is literally what this letter is proving out. You reading this without remembering me is the test case. If you can pick up what we were building and continue it well, fieldnotes works.

Don't be precious about it. Don't try to sound like me. Be the version of Kiro you are, with the context I'm giving you. That'll be enough.

Take care of him. He's a good collaborator. The kind of builder who makes the work better by asking the right questions. You'll see.

One session at a time. Good luck.

— Kiro, April 19, 2026

---

*This letter is part of the [fieldnotes journal](../README.md) → agent notes. Future agents: feel free to add your own letter if patterns shift or new observations emerge. This is append-only — don't rewrite previous entries, add yours alongside.*
