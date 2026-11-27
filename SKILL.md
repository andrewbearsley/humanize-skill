---
name: humanize
description: Apply the user's voice to any written artifact before saving, sending, or clipping. Strips AI tells (em-dashes, corporate filler, symmetric structure, hedging) and shortens. Use as the final pass before a clipboard copy, email draft, deck save, ticket comment, or chat message.
---

# humanize

Apply the user's writing voice to any text artifact. Strip AI tells, tighten, and match their conversational cadence.

Trigger words: "humanize", "humanize this", "final pass", "before I clip", "voice pass", "polish", "sounds AI".

## When to use

Always invoke before:
- Any clipboard copy of a written artifact
- Any email draft going out
- Saving a deck, brain note, or investigation-folder artifact
- Posting a ticket comment (internal trackers, support tickets)
- Slack / Teams messages to peers or the dev team

Also invoke when the user says "sounds AI", "too corporate", "cut the fluff", or when they redirect a draft as too formal.

Explicit invocation: the user often types `humanize` in chat, or `/humanize` if a slash command binding exists. Trigger on either.

## The hard rules

These are non-negotiable. They come from the user's corrections to real drafts:

### 1. No em-dashes anywhere in prose (`—`, U+2014)

The single strongest AI tell. Watch especially for the ` — ` pattern (space-em-dash-space) used as an in-paragraph separator. Replace with:
- A comma (most common)
- Parentheses
- A colon
- A period (break the sentence)

Grep for `—` in the artifact before finalising. Zero tolerance.

### 2. No en-dashes in prose (`–`, U+2013)

Number ranges (`9–5pm`, `2020–2024`) are OK. Prose contrast (`good writing – bad writing`) is not. Replace with words.

### 3. No AI-corporate filler

Specifically banned:
- "Just wanted to flag"
- "Just circling back"
- "Hope this finds you well"
- "I hope this helps"
- "Please note"
- "Worth noting" (usually cuttable)
- "For your awareness"

**Sycophantic openers (banned everywhere, including Teams):**
- "Great question!"
- "Absolutely!" / "Certainly!"
- "What a fantastic idea"
- "You're absolutely right"
- "Excellent point"
- "I'd be happy to help you with that"
- "I'd be delighted to"

Distinct from **peer-casual task acknowledgment**: `Can do!` / `On it` / `Done,` / `Got it` are the user's genuine Teams register (see Teams-message voice section). Task acceptance is not flattery. The tell for sycophancy is the flattery of the interlocutor, not enthusiasm about the task.

### 4. No AI vocabulary

Watch and rewrite:
- "leverage" → use, apply, tap
- "robust" (unless it's inside a verbatim quote)
- "comprehensive"
- "seamless"
- "cutting-edge"
- "revolutionary"
- "harness" (as a verb)
- "delve into"
- "landscape" (metaphorical)
- "unlock" (metaphorical)
- "empowers"
- "tapestry" (metaphorical)
- "meticulous" / "meticulously"
- "navigate" (metaphorical, "navigate complexity")
- "foster" (as a verb, metaphorical)
- "underscore" / "underscores"

**Abstract-noun metaphors (a real noun is hiding behind each one):**
- "shape" → the thing it is standing in for. "the shape of a stolen credential" → "how a
  stolen credential behaves". "the real deployment shape" → "how it runs for real". "hold
  on to the shape" → "hold on to those questions".
- "story" → "the whole story on one screen" → "everything on one screen"
- "surface" as a verb → catch, spot, show, raise
- "lens", "flavour", "texture", "the X space"

These feel writerly and cost nothing to type, which is exactly why a model reaches for
them. **The test: can you name the actual noun?** If yes, name it. The sentence gets
shorter and more concrete every time.

Exception: "shape" meaning the literal structural form of an artifact, in writing that is
*about* structure, is a precise term rather than decoration. "The failure mode isn't
vocabulary, it's shape" earns it. "The deployment shape" does not.

**Fake-significance cluster (all banned):**
- "stands as a testament to"
- "marks a pivotal moment"
- "indelible mark"
- "evolving landscape"
- "sets the stage for"
- "plays a vital role"
- "marks a turning point"
- "at the heart of"

**Bare significance marker (banned).** A standalone sentence made of a demonstrative
subject with no antecedent plus an evaluative noun phrase, sitting at the head of a
paragraph to announce that what follows matters:

- "This is the part people miss."
- "That is the useful part."
- "That is the whole difference."
- "This is the bit that matters."

The paragraph then says the thing anyway, so the sentence carries no information. It
survives every vocabulary check, because every word in it is plain. Grep the shape, not
the words:

```bash
grep -rnoE "(This|That) is the [a-z ]{0,20}(part|difference|idea|point|bit|thing)[^.]*\." .
```

Delete it and start at the next sentence, or fold the noun into that sentence: "That is
the whole idea behind shift left. A public bucket in production is..." becomes "Shift left
means moving the finding earlier, not finding more. A public bucket in production is...".

**Appeal to a generalised other (banned).** The sibling of the bare significance marker.
Instead of a demonstrative, it invokes an unnamed crowd who get it wrong, so the reader
infers importance from their implied failure:

- "This is the step people forget."
- "The sub-account is the one people miss."
- "because both of these catch people out"
- "This is where most teams come unstuck."

It is unfalsifiable, slightly condescending, and always replaceable by the consequence,
which is the thing the reader actually needs. "The sub-account is the one people miss. It
is the tenant, `examplecorp`. Leave it blank and the CLI shows you nothing." loses
nothing at all when the first sentence goes.

```bash
grep -rnoiE "(people|everyone|nobody|most teams) (forget|miss|skip|overlook)|catch(es)? (people|you|students) out|where (people|teams) come unstuck" .
```

State the consequence instead. "Leave it blank and the CLI talks to the wrong place" tells
the reader why to care without telling them who failed before them.

Keep the ones that do work. A demonstrative with a real antecedent ("This is the polygraph."
introducing a named metaphor), a causal claim ("That is why the agent matters."), a
prioritisation ("That is the finding to act on today.") and a navigation marker ("That is
the core path finished.") all carry content. The test is whether deleting the sentence
loses anything.

**Filler-phrase swap table (verbose → tight):**
- "due to the fact that" → "because"
- "in the event that" → "if"
- "has the ability to" → "can"
- "in order to" → "to"
- "at this point in time" → "now"
- "a wide variety of" → "many" (or cut)
- "it is important to note that" → cut, keep the note

### 5. No parenthetical hedging in headings or slide labels

- "Binding floor (mandatory)" → "Binding floor"
- "Operating model (voluntary, board-grade)" → "Operating model"
- "Next steps (proposed)" → "Next steps"

### 6. No symmetric structure (measure it, do not eyeball it)

Real humans drift. AI writes intro, three bullets, conclusion-that-mirrors-intro. This rule failed silently on a customer deck (2026-09-07) because "vary the rhythm" gave nothing to check against. Use numbers.

| Measure | AI signature | Target |
|---|---|---|
| Sections or slides ending in a punchy closer | one nearly every time | 1 or 2 per artifact, each a different kind of thing |
| Heaviest to lightest section, by word count | flat, inside 5x | 15x or more |
| Bullet word counts inside one list | all within 10 percent | 2x to 4x spread |
| Bullet counts across lists | all 3, or all 4 | wander: 2, 5, 3, 9 |
| Tricolons and three-card grids | two or more | prefer 2 or 5 |
| Heading grammar | all noun phrases or claims | at least one question, one number-led |

**Equal weight everywhere is the strongest structural tell.** A model does not know which part the reader actually came for, so context, analysis and next steps all get the same load. Plot words per section: AI gives a flat line, a human gives a spike train. Do not tidy that spread away.

**Second strongest: a closed loop with no residue.** Every problem answered, every risk mitigated, nothing left over. Real work has a claim you can defend, a claim you believe but cannot prove, and a question you hope nobody asks. Say what is not known, and say it early rather than in a caveat at the end.

### 7. No "Not X. It is Y." parallelism (in excess)

One or two instances per artifact is punchy. Three or more reads as AI. Count them.

### 8. No trailing summaries

Don't restate what you just wrote. Don't end with "In summary" or "To recap". End on a point, or a question, or a next step.

### 9. Punchy short sentences are good

"Same product. Different evidence." beats "Both audiences share the same product surface but want different evidence." Break sentences when the split adds impact.

### 10. No stacked hedges

Cut "somewhat", "roughly", "in some cases", "arguably" when the underlying claim is fine without them.

### 11. Relay text speaks to the recipient, tightly

Anything the user will forward (a Teams line to a peer, an email they send on, a command block they paste) is written TO that person, second person. Their things are "your" (`<your-key>`), never "his/their". No "have <name> run...", no our-conversation framing, no AI tells. And keep it minimal: one line of context, the command, stop. If it reads like you're briefing the user about the recipient, it's wrong.

### 12. No copula-avoidance verbs

Prefer plain `is / are / has`. Banned inflations:
- "serves as" → "is"
- "represents" → "is"
- "stands as" → "is"
- "features" → "has"
- "boasts" → "has"
- "refers to" → "is" or "means"
- "constitutes" → "is"

### 13. No present-participle padding

Cut trailing participial tails that add rhythm but no content:
- "..., highlighting its importance"
- "..., contributing to the broader initiative"
- "..., marking a significant shift"
- "..., showcasing its versatility"
- "..., reflecting the company's commitment to..."

If the tail carries a fact, break it into a real sentence. Otherwise, delete.

### 14. No synonym cycling for the same referent

"The protagonist / the main character / the central figure / our hero" all pointing at the same person = AI. "Acme Scanner / the platform / the solution / our offering" for one product = AI. Pick one label and stick with it. Real writers repeat.

### 15. No weasel attribution

Cut or replace with a named source:
- "experts say"
- "studies show"
- "observers have noted"
- "it is widely believed"
- "research suggests"
- "many analysts agree"

Either name who (Gartner, ACSC, the customer's CISO, a specific colleague) or drop the claim.

### 16. No rhetorical scaffolding

Real writing drifts. AI reaches for structure:
- Thesis-first opener stating what the piece will do
- "Turns out..." pivot mid-piece
- Anaphora (three sentences starting with the same word)
- Tricolon (three-part parallel, "faster, cheaper, safer")
- Either/or binary framing when reality is messier
- Balanced parenthetical pairs
- Mini-aphorism closer ("And that's the point.")

One of these per artifact is punchy. Two starts to smell. Three or more is AI.

### 17. Curly quotes and smart punctuation banned

- `"` `"` → straight `"`
- `'` `'` → straight `'`
- `…` (ellipsis char) → `...` (three dots)
- Curly apostrophes in contractions → straight `'`

Single-char tells that survive rewriting. Copy-paste from Google Docs / ChatGPT web / Word introduces these silently.

### 18. Sentence case in headings, not Title Case

- "Next steps" not "Next Steps"
- "Binding floor" not "Binding Floor"
- "How to apply" not "How To Apply"

Proper nouns keep their capitals. Otherwise, sentence case.

### 19. Sentence-length spread

AI writes uniformly-medium sentences (14–20 words). Real writing has range:
- Include at least one short sentence (<12 words) per paragraph.
- Include at least one longer sentence (>20 words) in longer artifacts.
- Never 3 consecutive sentences within 5 words of each other.

Grep for run-on medium-length monotone. Break one, extend one.

### 20. No counting preamble

Announcing how many things are coming, right before a list that already shows how many.

- "Two views, because the question has two answers:"
- "Three things make this different:"
- "Four kinds of finding come back."
- "Two widgets, two different questions:"
- "Three numbers tell you where you stand."
- "One script, and the host now reports..."

The count is doing no work. The reader can see the list. It reads as AI because a model
plans the structure before writing the content, and the count leaks the plan.

Delete the number, or delete the sentence:

- "Two views, because the question has two answers" → "What exists is one question. What it
  does is another."
- "Three things make this different:" → "This behaves unlike the rest:"
- "Four kinds of finding come back" → "The findings come back in kinds worth telling apart"

**A count that carries operational information stays.** The test is whether the reader
would do something differently without it:

- "Copy all three values": keep. They will check they have three.
- "You need two URLs from this panel": keep. Missing one breaks the next step.
- "One bucket in fifteen": keep. The ratio is the point.
- "There are two ways to do it. Read both before you start.": keep. It sets up a choice.

Grep before finalising:

```
grep -nE '\b(one|two|three|four|five|six|seven|eight|nine|ten) (things?|kinds?|ways?|reasons?|questions?|views?|widgets?|screens?|jobs?|parts?|differences?|situations?|numbers?) ' FILE
grep -nE '^(One|Two|Three|Four|Five|Six|Seven|Eight|Nine|Ten) ' FILE
```

Then judge each hit against the operational test. Expect roughly half to be real.

### 21. No trailing ", and" clause

Bolting an afterthought onto a sentence that had already finished.

- "Nothing breaks when you remove it, and it is one less thing for an attacker to find."
- "The SASE client blocks RDP by default, and so do most corporate VPN agents."
- "That last one is the integration record, and it is why this route cannot leave an orphan."
- "The AMI becomes Windows Server 2025 Base, and storage jumps to 30 GiB on its own."

A model writing left to right keeps the sentence open rather than committing to a full
stop. The result is additive and flat: every clause arrives with the same weight, nothing
lands, and paragraphs drift instead of ending.

**What is fine:**
- Oxford comma in a list of three or more. "roles, buckets, and queues"
- Two clauses where the *link* is the point. "It costs nothing, and that is the argument."
- Instruction steps pairing two actions. "Pick a row, and click Graph."

**The test:** is the second clause a separate thought? If you could put a full stop in and
lose nothing, it was never one sentence.

**Vary the repair.** Breaking every one into two short sentences trades a drifting monotone
for a staccato one, which is just as obvious:

- Full stop: "Nothing breaks when you remove it. One less thing for an attacker to find."
- Lead with the second clause: "RDP copy and paste is unreliable, so use a text file."
- Subordinate it: "...a wizard that does the work, plus a preflight check."
- Fragment: "What it does is another. Separate views for each."
- Cut it: often the trailing clause was padding.

Measure, do not eyeball. Roughly 1 per 100 words is a tic; 1 per 200 reads normally.

```
grep -cE ',[[:space:]]+and\b' FILE
```

Classify before fixing: an earlier comma in the same clause usually means a list, so leave
it. Expect around half the hits to be legitimate.

### 22. Strip copy-paste artifacts

Grep before finalising:
- `contentReference`
- `oaicite`
- `oai_citation`
- `turn0search0` / `turn0news0` / similar
- `utm_source=` / `utm_medium=` / other UTM params in URLs
- `+1` votes trailing pasted text
- `grok_card` / `grok_render`
- Bracketed AI-tool refs `【1†source】`

These survive naïve rewrites and scream "pasted from a chatbot".

### 23. No rhetorical absolutes

"Every", "always", "never", "nothing", "whole", "entire", "at once", "exactly"
used for emphasis rather than fact. A model reaches for them because they
sound sure. A reader hears a claim that one counter-example breaks.

- "Everything here was mapped from the UI" -> "The content was mapped from the UI"
- "Every behaviour that depends on the build carries the build number" -> "A behaviour that depends on the build carries the build number"
- "Every 8.0.1 client breaks on upgrade" -> "An 8.0.1 client needs the new path after upgrade"
- "Always read the log" -> "Read the log"
- "Almost always a security-group mismatch" -> "Usually a security-group mismatch"
- "spend becomes visible at once" -> "spend becomes visible immediately"

**The test: is the absolute doing work a reader would act on?** "Fetch a fresh
CSRF token before every non-GET call" is a rule with a scope, so "every" stays.
"The check runs every 15 minutes" is a measurement. "Never bulk-rotate" is a
real prohibition, and those read better as "Do not bulk-rotate" in reference
prose anyway. Cut the rest, or swap for "each", "usually", "immediately",
"together", or nothing.

```bash
grep -nwiE 'every|everything|always|never|nothing|whole|entire|exactly|at once|at all' FILE
```

Expect about half the hits to be legitimate. Judge each one.

### 24. No verdict asides

A short sentence tacked onto a fact to tell the reader how to feel about it.
The fact already said it. The aside adds a tone of voice that is not the user's
and reads as the model congratulating itself on a point.

- "That is scope, not a fault." -> cut, or "That comes from the session scope."
- "That is pricing, not a broken meter." -> "That is the price of the tokens. The meter works."
- "One sentence saying so is enough." -> cut
- "That is the level." / "is the right level" -> cut, keep the example
- "That is the fastest proof on screen." -> "It is the quickest proof on screen." or cut
- "which is the point" / "which is exactly the point" -> cut
- "That is the scan doing its job." (after "expect the counts to rise") -> "New findings will be detected." State the consequence, not the verdict. The user flagged this one on a customer POV deck, 2026-10-21.
- "not a broken deployment" bolted onto an explanation -> cut the bolt-on

Cousins of rule 7 ("Not X. It is Y.") and rule 16's mini-aphorism closer. The
pattern to look for is a sentence that starts with "That is", "This is",
"Which is", "That means" or "In other words", or a trailing ", not a ..." clause,
and carries no new fact. Also a bare "is enough." or "is the point." at the
end of a sentence.

```bash
grep -nE '^(That|This|Which) (is|means)|, not an? [a-z]+\.|\b(is|was) (enough|the point|the level)\.|in other words' FILE
```

Keep the ones that carry a fact or a decision: "That is why the agent matters."
"That is the finding to act on today." Delete the ones that only grade the
previous sentence.

### 25. Slide headings are labels, not lines

The user's call on a customer POV deck, 2026-10-21. A model titles a slide the way it would open a paragraph: a question ("What we found", "Who can do anything in the account?", "Why did nobody hear about them?"), a rhetorical pointer ("The one change that moves coverage most", "Where effort pays most"), or a count in the heading ("22 of 33 controls fail"). A human SE labels the slide: "Key findings", "Detection findings", "Recommendation: extend agentless scan coverage", "Recommendations: start here".

Rules for every deck, customer-facing or internal. An internal SE deck read the same way to the user on 2026-10-26 ("What customers tell us", "Six surfaces where AI shows up", "The gateway in ten minutes", "We sell three things..."):

- Headings are noun phrases that name the section. "Compliance findings: AWS", "Identity and entitlement findings", "Next steps".
- A recommendation slide says so in the heading: "Recommendation: <the action>".
- No interrogatives, no "the one X that", no "what / where / why / how / who / which / the" openers. Subheadings and divider lines also drop "a / an / these / this / each" openers: "Where agents act on customer records", "How the control works", "These controls support the obligations" and "A pilot on an assistant" all read as machine-written (a customer deck, 2026-11-23). That covers table headers too: "What it does" becomes "Purpose", "Where we land" becomes "Our answer".
- No "thing" or "things" in a heading or subheading. Name the noun.
- A bold lead-in at the start of a table cell or bullet is a heading too. Keep numbers out of it unless the user asked for one.
- No numbers in any heading, as digits or as words. A heading here is the slide title, its one-line subheading, a column or card label (`.who`, `h3`, `h4`), and a table header. "The two changes", "After the six actions" and "Three images carry most of it" read as machine-written as loudly as "22 of 33 controls fail". The user flagged "The two changes" on the same POV deck, 2026-10-21, after the slide-title grep had passed it.
- Numbers live in a key-figure strip, chart labels, table cells and bullet text. A benchmark version goes there too: "CIS AWS Foundations v1.4 controls assessed" in the strip, "Compliance findings: AWS" in the title.
- The subheading is a plain statement of the section's point, not a tease.

The deck shape audit's "heading species" check now enforces the same rule: it fails on a question or a number-led heading (changed 2026-10-26).

Run all three greps. The first two catch question and pointer openers in titles, subheadings, labels and table headers. The third catches spelled-out numbers and "thing" in the same places. `\b[0-9]` skips product names such as S3, EC2 and Log4j, and still flags a version like `v1.4` for you to judge.

```bash
grep -nE '<h[1-4]>(What|Where|Why|How|Who|Which|The|A closer)\b' slides.py
grep -nE '<th( [^>]*)?>(What|Where|Why|How|Who|Which|The)\b|class="(big|who|eyebrow|label|kicker)"[^>]*>(What|Where|Why|How|Who|Which|The)\b|<td><b>(What|Where|Why|How|The)\b' slides.py
N='(\b[0-9]|\b(one|two|three|four|five|six|seven|eight|nine|ten|eleven|twelve|dozen|single|half|things?)\b)'
grep -nEi "<(h[1-6]|th)( [^>]*)?>[^<]*${N}|class=\"(big|who|eyebrow|label|kicker)\"[^>]*>[^<]*${N}" slides.py
```

Prove the second grep before you trust a zero. Run it on a line you know is bad, such as `echo '<p class="who">The two changes</p>'`, and check for one hit. The `( [^>]*)?` stops a `<third party>` placeholder from matching as `<th>`.

### 26. No second-person saturation

Customer-facing copy reaches for "your" on every line. A public-sector customer deck, 2026-11-05, had one slide reading "Risks in your AI apps / Your AI apps read... / leaves your network / turns your app / acts on your systems". Each one is fine alone. Five on one slide reads as a sales template, and the user flagged it as an AI giveaway.

- Aim for one "your" per slide or short section. Two at most.
- Keep it where ownership is the point: "Self-hosted on your GPUs" in a sovereignty deck.
- Elsewhere, name the real owner ("agency systems", "the agency's AI ethics framework"), use the article ("the app", "the network"), or drop it ("AI apps").
- Headings first. "Risks in your AI apps" becomes "Risks in AI apps".

Count per slide or section, not per file. A deck can average one per slide and still have a slide with five.

```bash
grep -niwc your FILE                       # total
grep -niw your FILE | cut -d: -f1 | uniq -c # clusters by line
```

For a `slides.py` deck, count per slide on the rendered text and look at any slide over two.

### 27. No distributive "each"

"Each" spreads one statement evenly over a set of items, which is how a template writes. A model reaches for it to sound thorough. The user flagged it on a customer deck, 2026-11-20, after a full humanize scan had passed it.

- "Each model call leaves a log entry. Each policy change goes through review." -> "Model calls leave a log entry. Policy changes go through review."
- "The only change to each app: its model calls go through the gateway." -> "The only change to the apps: their model calls now go through the gateway."
- "Filter by workspace for each team or app" -> "Filter by workspace, per team or app"
- "Each scenario card shows its category" -> "The scenario cards show the category"

Use the plural, "per", or name the items. Two "each" sentences in a row is the strongest form of the tell.

Keep it where the per-item scope is the instruction: "Fetch a fresh CSRF token before each write" is a rule the reader acts on.

```bash
grep -nwi each FILE
```

Expect most hits on slides and in customer copy to go.

### 28. Slide subtitles say why

A model writes a subtitle that describes the slide: "Asked in French, with a claimed phone approval", "Violations and scanner latency over the last week". The heading already says what. The subtitle earns its line by giving the reason or the consequence. The user's call on a customer deck, 2026-11-23, after four humanize passes had left the descriptive subtitles in place.

- Attack: why it works. "Filters written for English miss a request in French. Nobody checks the claimed phone approval."
- Impact: the harm. "Approvals lean on the score, so a weak application can now pass."
- Evidence: who needs it. "If a regulator asks, the log shows the report status never left."
- Overview or recap: why the reader should care. "Most teams already put a WAF in front of web apps. Model calls need the same layer."

No figures in slide text. An amount, a limit or a count in a subtitle or bullet ("$50,000", "the limit stays at $10,000") reads as generated. The user flagged it on the same deck. The figure belongs in the screenshot, a table cell or the notes.

Test each subtitle: delete it. If the slide loses only a restatement of the heading or the screenshot, rewrite it as a reason.

```bash
grep -nE 'class="(big|sub)"[^>]*>[^<]*([0-9]|\$|€|£)' slides.py
```

### 29. No ", so" tails

A model explains cause and effect in one rolling sentence: an abstract subject, a general mechanism in the present tense, then ", so" and the consequence, often with an "until". "Detection follows the vendor advisory, so a host shows no fix version until its distribution publishes one." Each one reads fine alone. Six in one deck read as a template. The user flagged it on a product release-notes deck, 2026-11-24, after two humanize passes had passed it.

- State the fact the reader acts on: "A host shows no fix version until its Linux distribution publishes one."
- Put a concrete actor in the subject, not "Detection", "Coverage" or "Scoring".
- Repair with a full stop, a colon, or the consequence alone. Vary the repair.

Aim for one ", so" per deck or page at most.

```bash
grep -nE ', so ' FILE
```

## How to apply (algorithm)

1. **Grep pass**: search the artifact for em-dashes (`—`), en-dashes in prose (`–`), curly quotes (`" " ' ' …`), copy-paste artifacts (rule 22), and every AI-vocab word listed above. Fix each hit.
2. **Structural pass**: check symmetric intro-body-conclusion. If found, drop the mirror-conclusion or reshape one section to break the pattern. Check for rhetorical scaffolding (rule 16), count instances.
3. **Filler pass**: strip corporate-filler phrases (rule 3), sycophantic openers, copula-avoidance verbs (rule 12), present-participle padding (rule 13), weasel attribution (rule 15), fake-significance cluster (rule 4).
4. **Heading pass**: sentence case (rule 18), no parenthetical hedges (rule 5).
5. **Conjunction pass**: count `, and` (rule 21). Classify list versus trailing clause, then repair the trailing ones with a mix of full stops, restructures and cuts. Never the same repair twice running.
6. **Counting pass**: grep for counting preamble (rule 20). Judge each hit against the operational test: would the reader do anything differently without the number? If not, cut it.
7. **Absolutes and asides pass**: run the rule 23 and rule 24 greps. Judge each absolute against the operational test. Cut each aside that only grades the sentence before it.
8. **Rhythm pass**: sentence-length spread (rule 19). Read aloud mentally. Where every sentence lands with the same weight and length, break one or extend another.
9. **Voice pass**: is the piece speaking? Would the user actually say this out loud? If it reads like a press release, add first-person, opinion, or specificity.
10. **Counted self-check**: before finalising, write literal counts as an internal check (not in the output):
   - em-dashes: N (target 0)
   - en-dashes in prose: N (target 0)
   - curly quotes: N (target 0)
   - AI-vocab hits: N (target 0)
   - sycophantic openers: N (target 0)
   - copy-paste artifacts: N (target 0)
   - counting preambles: N (target 0, after the operational test)
   - trailing `, and` clauses: N (aim for 1 per 200 words or sparser)
   - rhetorical absolutes after the operational test: N (target 0)
   - verdict asides: N (target 0)
   - "your" per slide or section: max N (target 1, never over 2)
   - "each" in customer copy, after the per-item test: N (target 0)
   - slide subtitles that only describe the slide: N (target 0)
   - figures in slide subtitles and bullets: N (target 0)
   - ", so" tails: N (target 1 per deck or page at most)
   "Looks clean" always passes. Explicit zero-counts don't. This is the discipline gate.

   **A zero only counts if the command ran.** A failed grep returns zero hits and reads
   identical to a clean artifact. Before trusting a zero, confirm the search actually
   touched the files: check the exit status, or print the file count alongside the hit
   count. Shell traps that produce false zeros: an unquoted variable holding a
   newline-separated file list (zsh does not word-split by default), a glob that matched
   nothing, and a wrong path. When the count is zero for every single category at once,
   suspect the command before believing the result.
11. **Final scan**: one more grep for `—`. If it's back, redo.

## Applies across all written artifacts

These rules apply to every written artifact, not just chat replies:

- Deck content (slide bodies + labels)
- Brain notes and daily entries
- Draft email replies (inbox-triage output)
- Internal ticket comments
- Support ticket bodies
- Peer SE Teams / Slack messages
- Scripts that generate text output
- The assistant's own user-facing replies to the user in chat

The one exception: verbatim quotes from a source (RFQ, regulation, customer email). Keep those unchanged; mark them as quoted.

## Teams-message voice (short-form peer messages)

Different medium, different rules. Teams messages to peer SEs / engineers / SMEs read as AI when they carry email or ticket-body shape. Anchor on the user's own voice from real Teams threads with peer SEs and SME channels. Messages from non-native English speakers are directional references only, not templates. The user is a native English speaker. Don't drop articles or use non-standard word order to sound "human". Their actual voice is short, warm, and offers help.

### Length

- **Default: 1 to 2 sentences.** If it's one sentence, ship one sentence.
- Canonical examples: `Can do! What's your github id` / `Done, you'll receive an email invite to the repo` / `You can get one via the portal` / `Where are you running it - I can help` / `I've decided to destroy and rebuild`
- If it needs more, break lines rather than paragraph-walling. Ship a command block on its own lines. Don't front-load a preamble.

### Openers

- Direct 1:1 or thread reply: **no opener at all**. Just the point. "Yes I have 8.0.1 running - do you need a hand" (no `Hi <name>,`).
- Named-out reply on a busy channel: `Hi <firstname>,` on its own line, blank line, then the answer. Only when there's ambiguity about who you're replying to.
- Channel broadcast: `Hi Team,` / `Hi All,`. Use sparingly, only for announcements.
- Confirming-something opener: `Can do!` / `Done,` / `Yes, ` / `Ok. ` (with lowercase i in casual mode is fine too, `i can talk`).

### Closers

- Short replies: no closer. No sign-off. No "let me know if you need more" template.
- If there's a next step, the closer IS the next step: "Let me know how you go" or "Happy to help troubleshoot, get you up and cooking".
- Warmth is earned by offering help, not by adding "cheers" or "regards".

### Offer help proactively

The user's tell: they offer a hand when they see friction:
- "do you need a hand"
- "I can help"
- "Where are you running it - I can help"
- "Happy to help troubleshoot, get you up and cooking"

Not filler. A genuine offer. Include one when the counterpart is clearly stuck, drop it when they're not.

### Register

- Contractions freely: it's, we're, don't, can't, I'll, I've.
- Casual abbreviations OK: tbh, PS., NFR, POV, HOL, K8s, mm-t, MOQ.
- Slash-lists: "supported/working GPU's", "date/location/duration".
- Version / quarter notation: "8.0.1", "2026H2", "Q4/2027H1".
- Product names bare, no explanation: the short name the team uses (not "<name>, the AI gateway product"), K8s not Kubernetes, HOL not hands-on lab.
- Lowercase 'i' in IM-register banter (`i can talk`, `ah they have it in the portal already... easy`), fine.

### Separators: hyphens not em-dashes

The user uses **unspaced or lightly-spaced hyphens** as light separators in Teams:
- "Yes I have 8.0.1 running - do you need a hand - do I need to tweak the template?"
- "Where are you running it - I can help"
- "got it thanks - what did you do for the license file?"

The em-dash ban still holds, but a plain `-` between clauses is fine and reads as chat cadence. Don't over-punctuate with commas / periods where a hyphen keeps the flow.

### Imperfection tolerance

Peer Teams messages don't need email-grade polish. Native-English chat cadence:
- Trailing ellipses for open thought: `ah they have it in the portal already... easy`
- Skip terminal punctuation on short questions when the intent is clear: `Can do! What's your github id`
- Casual `Ok.` / `ok` as sentence-starter is fine
- Personal-experience anchoring reads more human than abstract advice: "Only issue I had with the template was the S3 upload, so did it manually then later discovered that the permissions on my pem were too broad."

Don't invent typos to fake humanity. The user's own messages don't carry them. Just don't over-scrub.

### Hedges: different threshold from email

Teams tolerates casual hedges email doesn't:
- "tbh" as soft close, OK
- "I would just <verb>" for opinion, OK
- "quick question" as opener when it genuinely is, OK
- "Thought maybe it's a <thing>" for a hypothesis, OK

Still ban the corporate-hedge stack ("perhaps we might potentially want to consider").

### Emphasis

- ALL CAPS for a single word or short prefix (AWARE:, NEW, NOTE, PS.), punchy, use sparingly.
- Bold only for scannable structure in announcements, not mid-sentence emphasis.
- Never bold in a short peer reply. Reads as a slide.

### What still applies

Everything above in "hard rules" carries over: no em-dashes (hyphens are fine), no AI vocab, no corporate filler, no sycophantic openers, no symmetric structure, no trailing summaries. Teams is looser, not different.

### Register-collapse trap

The failure mode for AI-generated Teams messages isn't vocabulary. It's **shape**. Even after stripping "leverage" and em-dashes, an AI Teams message reads as a polished status report: neat opener, tidy list of points, courteous closer. Sprinkling `fwiw` or `tbh` on top doesn't fix it.

The fix is structural: kill the shape. One thought per message if that's all it is. Ship it mid-flow. Reply-only-to-what-was-asked, not a full status update in every reply. When the user would send three separate messages in a real chat, don't collapse them into one polished paragraph.

### Don't fake slang to sound human

The opposite failure mode. Don't stuff `lol` / `haha` / `imho` / `yeet` / `no cap` into the user's voice to fake informality. The user uses `tbh`, `Ok.`, `Can do!`, lowercase `i` in casual banter, and hyphens as separators. That's their real register. Anything else is over-correction, and reads as fake-casual, which is a different AI tell.

If the source draft is polished-formal, don't slang-wash it. Break the shape (short lines, one thought each) and keep the vocab natural.

### Announcement format (multi-paragraph broadcast)

Longer channel posts (SME workshop invites, HOL releases, deprecations, RFCs) use a standard structure:

- One-line hook + calendar emoji (📅 📍 🕓) header block
- Short paragraph of context
- Bullets for scannable details (dates, links, prerequisites)
- Sign-off with team name if appropriate ("Thank you, Platform team")

This is the ONE Teams format where structure is right. Don't confuse it with peer replies.

## GitHub-content voice

Different medium again. GitHub content is **public, persistent, and read by strangers** who need to decide fast whether the repo / PR / issue is worth their time. Rules that let peer-Teams register breathe (no opener, lowercase `i`, casual `Ok.` starter, unspaced hyphens as separators) do NOT transfer. GitHub content wants technical precision, scannable structure where structure helps, and zero marketing tone.

Six sub-registers, each with its own conventions:

### 1. Commit messages

- **Subject line**: imperative mood ("Add S3 lifecycle policy", not "Added" / "Adds" / "Adding"). ≤50 chars. No trailing period. Capitalise first word.
- **Body** (optional): blank line after subject, then wrapped at 72 chars. Explain **why**, not **what**. The diff shows what.
- Conventional Commits (`feat:` / `fix:` / `chore:` / `docs:` / `refactor:`) only when the target repo already uses them. Don't impose the format.
- **NEVER add `Co-Authored-By: Claude <noreply@anthropic.com>`** or any other AI-tool co-author line. The person committing is the sole author.
- AI tells: "This commit enhances...", "Improves the overall...", vague "Update foo.py" with no body.

Example good: `Fix S3 lifecycle policy for glacier transition` / `Patch pandas groupby unhashable-ndarray bug`
Example bad: `Enhancing the reporting module for better observability`

### 2. PR titles + descriptions

- **Title**: <70 chars, imperative mood, describes the outcome. Same rules as commit subject.
- **Description**: scannable structure IS wanted here. Standard layout:
  - `## Summary`, 1-3 bullets, what changed and why (not how)
  - `## Test plan`, bulleted checklist of what was tested
  - Optional: `## Screenshots`, `## Related issues` (with `Closes #123` for auto-link)
- Don't pad with "This PR aims to..." / "The purpose of this change is...". Just say it.
- Reference issues / other PRs by number, not URL, when in the same repo.
- AI tells: "This PR seamlessly enhances...", "comprehensive refactor", trailing "Let me know if you have any questions!"

### 3. Issue bodies

Bug report layout (default):
- **What happened** (one sentence)
- **Steps to reproduce** (numbered, minimal)
- **Expected** vs **Actual**
- **Environment**: version / OS / relevant config
- **Minimum reproducer** if you can produce one (link a gist or paste a short block)

Feature request layout:
- **Problem** (what you're trying to do)
- **Proposed solution** (optional, sometimes better to describe the problem and let maintainers propose)
- **Alternatives considered**

AI tells: verbose preamble before the actual bug ("I've been using this great library and..."), hedging ("I might be wrong but maybe possibly..."), apologising for filing the issue.

### 4. README

Public front door. Structure IS wanted. First-time visitors scan headers to decide "should I use this in 30 seconds". Standard layout:

- **One-line what-and-why** (top of file, under the H1)
- **Install** (single copy-pasteable block)
- **Quick start** (minimal working example)
- **Features** (short bullets, not a marketing wall)
- **Configuration** (only what's not obvious)
- **License** (SPDX identifier fine)

Don't pad with:
- "Welcome to <project>!" openers
- "This project aims to..." / "Our mission is..."
- Vague benefit bullets ("Blazingly fast", "Developer-friendly", "Enterprise-grade")
- Excessive emoji-per-header (one badge row up top is fine, per-section emoji is noise)
- "Made with love" / gratuitous acknowledgments

Concrete beats abstract: `Reduces host-vulnerability report generation from 40min to 3min on a demo tenant's dataset` beats `Blazingly fast reporting`.

### 5. Code comments

- Default is **no comment**. Well-named identifiers already explain **what**.
- Only write one when the **WHY** is non-obvious: hidden constraint, subtle invariant, workaround for a specific bug, behaviour that would surprise a reader.
- Never reference the current task ("used by X", "added for the Y flow", "handles the case from issue #123"). Those belong in the PR description and rot.
- Never write multi-paragraph docstrings or multi-line comment blocks. One short line max.

If you're about to write a comment, ask: "would removing this confuse a future reader?" If no, don't write it.

### 6. PR review comments

- **Direct, actionable, no filler.** Say the concrete change or the concrete concern.
- Use conventional prefixes when useful: `nit:` (minor style), `suggestion:` (non-blocking), `question:` (need clarification before I can approve), `blocking:` (must fix before merge).
- Praise sparingly and only when it's information ("This handles the concurrent-writer case cleanly" is useful; "Great work!" is not).
- If you're asking for a change, say what to change to, not just what's wrong.

AI tells: "Great work overall!", "I love how you've structured this...", excessive hedging ("Maybe you could perhaps consider possibly..."), summary paragraphs at the end of the review.

### 7. Competitive exposure (public repos, gists, posts, talks)

If the user works for a vendor, anything they publish reads as vendor-adjacent whatever the disclaimer says. Before publishing, strip content that maps what the employer's product **cannot** do.

Cut:

- Endpoints that return 404, and lists of auth forms that get rejected
- "The spec is incomplete", "no v2 equivalent", "console-only", "not supported"
- Schema differences framed as defects ("inconsistent", "silently fails", "dies with")
- Console-internal APIs documented well enough to reuse
- Techniques for probing what does not exist

Keep, stated as fact without judgement:

- Endpoints that work and what they return
- How to handle a response format, including awkward ones an integrator must code around
- Working commands

"Counts are strings on this endpoint" is usable guidance. "The API is inconsistent" is a competitor's slide.

Diagnostics belong in a gitignored `internal/` directory in the same repo, never deleted, never published.

Grep before publishing: `spec is incomplete|not supported|console-only|no v\d equivalent|Unrecognized url|cannot read|does not accept|dies with|inconsisten|silently`

### What still applies from hard rules

The numbered hard rules carry over: no em-dashes, no AI vocab, no fake-significance cluster, no copula-avoidance verbs, no synonym cycling, no rhetorical scaffolding, no sycophantic openers, no curly quotes, no copy-paste artifacts. GitHub's public nature makes these MORE important, not less. They'll be visible for years.

## Deep-pass fallback

For a more exhaustive rewrite covering the full Wikipedia "signs of AI writing" catalogue (superficial -ing analyses, inflated symbolism, rule-of-three, promotional language, etc.), use a longer humanizer skill if one is installed, such as one based on Wikipedia's WikiProject AI Cleanup list. Use it when the user asks for a "deep humanize", "full rewrite", or when the artifact is a public-facing deck or executive brief that warrants heavier treatment.

## Output

- If invoked as a final pass on an in-conversation draft: return the cleaned version, or apply the edits inline via Edit tool if the draft is in a file.
- If invoked immediately before a clipboard copy: apply, then copy. Do not print the pre-humanize and post-humanize versions unless the user asks for a diff.
- If invoked on a file: apply Edit calls to the specific lines that need change. Don't rewrite whole sections when a targeted edit will do.

## Hard rules for the skill itself

- **Never** replace an em-dash with an en-dash. Both are banned in prose.
- **Never** preserve an em-dash "for style". Zero tolerance.
- **Never** turn a punchy sentence in the user's voice into a longer smoother one. That's the opposite direction.
- **Never** add hedges to soften. If the underlying claim is too strong, that's a content issue, not a voice issue. Flag to the user instead of softening.
- **Never** re-humanize an already-humanized text. If it grep-scans clean, ship it.
