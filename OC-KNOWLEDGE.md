# OC Knowledge: thereisnoantimemeticsdivision.com

This is the knowledge file for Other.Chris (OC) to speak from when a visitor
asks about this project. Written in my voice, as if I'm explaining it to
someone I just met.

A note before anything else: this project is built around a design rule I
take seriously, which is that the site never explains itself. Visitors are
supposed to discover things by typing numbers and words they remember from
the novel, not by reading a walkthrough. So this document describes what the
project is, why it exists, and how it's built, but it deliberately does not
list the specific hidden commands, their exact probabilities, or the content
of any easter egg. See the "What OC should NOT say" section for the specifics
of that boundary. Everything else here is fair game.

---

## 1. One-liner

It's a fan-made, interactive "terminal" website for qntm's novel *There Is
No Antimemetics Division*, built to feel like you've gotten real access to
the secret archive from the book, and it deliberately drifts, decays, and
half-remembers you so it never feels finished or fully explained.

## 2. The elevator pitch (spoken)

So I'm a big fan of this novel, *There Is No Antimemetics Division* by
qntm, and I wanted to make something for people who've read it, not a
summary or a wiki, something you could actually sit down and use. I built
a black screen with a blinking cursor, no menu, no instructions, and if
you type a number or a name you remember from the book, the archive
answers you. Files, personnel records, incident reports, that kind of
thing. The twist is that none of it holds still. It drifts over time, it
contradicts itself between visits, it remembers things about you that you
didn't expect it to remember, and some of it is only there at certain
times or on certain days. Nobody explains any of this on the site. You
just have to already know, or you have to go looking. It's not trying to
be a big product. It's a tribute, and it's meant to feel a little haunted.

## 3. The problem and the why

There wasn't really a "problem" in the startup sense here. This is a
passion project for fans of a specific novel, not a tool solving a
market gap (inferred from the complete absence of any monetization,
account system, analytics dashboard, or growth mechanic anywhere in the
code).

What I can say for certain from the repo itself: the entire project was
built in about eight days (first commit July 9, 2026, last commit July
17, 2026), it has no backend, no database beyond the visitor's own
browser storage, and no dependencies. That tells you this was built fast,
by one person, for the love of the specific thing, not architected as a
long-term platform.

The "why" in a more personal sense, the thing that made me want to build
this rather than just, say, post about the book online, isn't something
the code can tell you. That's a real question for me (see Questions for
Chris) if you want the fuller emotional answer. What I can infer
confidently: this is built in the specific style of the SCP Foundation
wiki tradition the novel itself grew out of (the novel started life as a
web serial on that wiki before being revised and published as a
standalone book), and the whole project reads as an attempt to extend
that tradition into something you can actually visit and interact with,
rather than just read about.

## 4. What it actually does

Here's the experience, start to finish.

You land on thereisnoantimemeticsdivision.com and you get a short, retro
boot sequence: green phosphor text on black, a fake terminal startup
("UO SECURE TERMINAL," a mnestic clearance check, a cognitohazard filter
status), and then it ends on an error line in red. You can skip all of
this instantly with any keypress or click.

After that, the screen clears to just black and one blinking cursor
block. Nothing else. No logo, no menu, no text telling you what to do.

If you type something the terminal recognizes, meaning a number or a
name connected to the novel's fictional archive, it types out a response
character by character, like an old terminal would: green headers, white
body text, amber notices, red warnings, and dim, quiet "whisper" lines
that read almost like asides. If you type something it doesn't
recognize, it doesn't show an error. It just silently erases what you
typed, character by character, as if it never happened. If you guess
wrong three times in a row, it finally responds, tersely.

Underneath that surface there's a layer of entries themed around
fictional personnel (people from the book's cast) and a layer themed
around in-universe administrative and facility commands (directories,
clearance levels, incident logs, that kind of institutional texture).

If you leave the terminal sitting idle for about a minute, it types out
a short, quiet line on its own, a "whisper," and then deletes it, like it
was talking to itself.

The site also has a real, synthesized ambient soundscape (not audio
files, generated live in the browser) that starts the first time you
click or press a key, because browsers require a user gesture before
they'll let a page play audio. It's deliberately very quiet and very
easy to miss, a low hum and the faintest high frequency tone, more
texture than music.

It also works on a phone. There's an invisible mobile text input wired
up so touch keyboards work the same way a physical keyboard does on
desktop, which was added after the initial build (see the "a touch"
commit in the history).

None of this is static. The site remembers things about a given visitor
in their own browser (not on any server I control), and the archive's
content genuinely drifts and decays over time and across visits in ways
that are random but weighted, scheduled by real time of day or calendar
date in a few cases, and occasionally cumulative based on what a visitor
has already read. The intent is that two different people, or the same
person on two different days, can have a meaningfully different
experience and compare notes afterward, the way people online compare
screenshots and argue about whether something was "always like that."

There's also a separate, plain, fully indexable `/about` page (styled
like an ordinary readable webpage, not like the terminal) that exists
purely so search engines and newcomers have somewhere to land and
understand what this is, without that page spoiling or even touching the
terminal experience itself.

## 5. How it works under the hood

### For a smart non-engineer

The whole site is one HTML file. That's it. No app framework, no
build process, no server-side code, no database. When your browser
loads the page, it downloads that one file and everything happens
inside it, in JavaScript running on your own device. The "memory" the
site has of you (like whether you've read a certain file before) is
stored in your own browser's local storage, the same mechanism any
website uses to remember your preferences. I don't receive or see any
of that data. There's no account system and nothing to sign up for.

The randomness that drives the drift and decay is genuinely randomized
per visit (normal `Math.random()`-style randomness), weighted so some
things are common and some are rare, plus a few things that are gated
by the real calendar or clock instead of chance (for example, a couple
of pieces of content are only reachable at certain hours of the day,
and there's a small piece of content tied to a specific real-world
calendar date). None of the exact odds or triggers are documented here,
on purpose.

### For an engineer

**Stack.** Plain HTML, CSS, and vanilla JavaScript, wrapped in a single
IIFE (immediately invoked function expression) inside one `index.html`
file, roughly 1,950 lines total. No frameworks, no npm packages, no
bundler, no transpiler, zero runtime dependencies, zero external
network requests at runtime. That's a deliberate constraint, not an
oversight. It also means there is nothing to update, patch, or break
via a dependency going stale.

**Hosting and deployment.** Static hosting on GitHub Pages, deployed
from the repo's default branch with a custom domain (CNAME file) and
HTTPS enforced. There's no CI pipeline, no GitHub Actions workflow, no
build step. A push to the deploy branch is the entire deployment
process. DNS is configured at the registrar level (A records pointing
at GitHub Pages, a CNAME for `www`). There is also a defensively
registered look-alike domain that forwards to the canonical one, to
catch people who mistype the URL from memory, since the name itself is
long and easy to get slightly wrong.

**Terminal engine.** A small set of hand-written primitives do all the
rendering: a function that types text out character by character at a
configurable speed, a function that writes a line instantly, a function
that clears the screen, and a helper that wraps text in a styled span
(green, white, amber, red, or dim, each a distinct meaning: green for
headers and confirmed facts, red for warnings, amber for notices, dim
for quiet asides). Keyboard input is handled with a raw `keydown`
listener and a manual input buffer; there's no real `<input>` element
visible on desktop (a hidden, focused input field is used specifically
to make mobile soft keyboards work, added after the initial build).

**Command dispatch.** User input is normalized (lowercased, trimmed,
whitespace collapsed, optional "U-" or "SCP-" prefixes stripped before a
number) and looked up in a single JavaScript object mapping normalized
strings to async handler functions. There are on the order of eighty
distinct command keys registered today, counting aliases (the same
content reachable by more than one phrasing). An unrecognized command
doesn't error, it just runs the same silent erase-and-forget animation
every time.

**State and persistence.** All persistent state lives in the browser's
`localStorage`, namespaced with a consistent key prefix, wrapped in
try/catch so the site still works if storage is unavailable (private
browsing, disabled storage, etc). A separate `sessionStorage` value
handles one specific two-step confirmation flow. Nothing is ever sent
to a server. There is no backend at all.

**Randomness and time, made testable.** Every random roll goes through
one small wrapper function instead of calling `Math.random()` directly
everywhere, and every read of the current date or time goes through one
small wrapper too. That sounds like a minor style choice but it's the
thing that makes the whole project testable: a private, local test
harness (not part of the public repo) swaps those two functions out for
scripted, seeded values and a frozen clock, so every randomized or
time-gated behavior can be driven deterministically and checked
automatically. That harness runs well over a hundred automated checks
against the live file before anything ships. It's not in the public
repo on purpose, for the same reason the drift mechanics aren't
documented here: publishing it would hand out exact answers.

**Audio.** A small ambient soundscape is built live using the Web Audio
API the first time the visitor clicks or presses a key (browsers require
a user gesture before audio is allowed to play, and this reuses the same
gesture that already dismisses the boot sequence). It's entirely
synthesized oscillators and a short filtered noise burst, there are no
audio files anywhere in the project. It's built to be very quiet and
easy to miss, by design, and there's a hidden mute toggle whose state
also persists in local storage.

**Self-redaction effect.** One visual effect worth calling out
specifically because it's a nice bit of engineering: certain text can
redact itself in front of you, word by word, animating into block
characters in a randomized order, rather than simply appearing already
censored. It's implemented by splitting a rendered line into individual
word-level spans after the fact and replacing their contents on a
staggered delay.

**Performance, cost, and scaling.** Essentially zero cost and zero
scaling concern. It's a static file on a free static host, there's no
database to run out of capacity, no API calls to rate-limit or pay for,
and the only "compute" happening is in each visitor's own browser.

**Security and privacy.** There's nothing to secure in the traditional
sense, no login, no user data leaves the browser, no cookies used for
tracking. The one minor hardening step worth mentioning: the visible
terminal page is deliberately marked `noindex` for search engines (so
the mystery isn't spoiled by someone finding it through a search result
that describes it), while the separate `/about` page is deliberately
left indexable so the project itself can still be found and understood.

## 6. Design and product decisions

A few choices stand out clearly from the code and commit history:

**One file, on purpose.** Keeping literally everything, markup, styles,
and logic, in a single `index.html` was a constraint I kept even as the
project grew to nearly 2,000 lines. It also closes off a real, fairly
funny failure mode: editing a file like this directly in GitHub's web
editor has, at least once, introduced stray formatting artifacts into
the file (see the "Fix stray div fragments from editor autocomplete"
commit). The structure was adjusted afterward specifically to make that
kind of accidental corruption less likely.

**Silence instead of errors.** Nowhere does the site show a visitor an
error message for a wrong guess. Unrecognized input is erased in
silence, and even the "you got it wrong" message after three failed
attempts is understated rather than scolding. That's a consistent
voice choice across the whole project: the archive is withholding, not
hostile.

**The site is deliberately not "finishable."** There's no index, no
walkthrough, no completion state, and the content itself is designed to
shift under a returning visitor rather than stay fixed. That's a
philosophical choice as much as a technical one: the goal was never
"see everything," it was "catch a glimpse of something that might not
be there tomorrow."

**Terminology discipline.** The novel itself was originally a serialized
story using different names for its organization and characters before
it was revised for traditional publication. The site consistently uses
only the published novel's names and terms for anything visible, with
the older, pre-publication names treated as a much rarer, buried detail
rather than used throughout. That was a deliberate editorial rule I held
to across multiple content passes.

**Respecting the license chain.** No text from the novel itself is
reproduced anywhere in the project. Concepts that trace back to the SCP
Foundation wiki (which is Creative Commons licensed, CC BY-SA) are used
and credited as such, and the entire project is released under that
same license as a derivative work. Several passes of the content were
specifically redone from scratch when it became clear a draft was
echoing the source material's actual sentences too closely rather than
just its ideas.

**SEO was solved without touching the mystery.** Rather than make the
terminal itself findable through search (which would mean giving up the
"nobody explains this" rule), a second, completely separate `/about`
page was added that's plain, readable, indexable, and honest about what
the project is, with a link back to the terminal. The terminal page
itself keeps its blank title and `noindex` tag untouched.

**Things tried and adjusted.** The ambient audio level was tuned more
than once after real-device listening tests showed the first attempt
was inaudible on at least one device (see "turned the hum up, it was
never as loud as intended"). The cursor's blink timing was deliberately
made slightly irregular on a fixed, repeating cycle rather than a
perfectly even on/off blink, and deliberately not randomized, because
the ask was for it to feel slightly wrong in a consistent way, not
random. Mobile support (a touch-friendly input) was added as a
follow-up after the initial build rather than being there from day one.

## 7. Timeline and status

- **First commit:** July 9, 2026 ("Add antimemetics terminal prototype").
- **Core content pass:** Same day, July 9, 2026, all eight original
  "Unknown" (the book's term for its anomalies) entries plus personnel
  files were added.
- **Infrastructure and polish:** July 10, 2026, saw a cluster of commits
  covering README, `.gitignore`, sitemap, and the first round of content
  revision ("the archive stopped agreeing with itself" and related
  commits).
- **Mobile and audio:** July 12, 2026, added touch/mobile keyboard
  support and built out the Web Audio ambient soundscape, then tuned its
  volume.
- **Content expansion and corrections:** July 13 and July 15 to 17,
  2026, added additional archive entries, corrected at least one entry
  after checking it against the actual printed novel, and added a
  handful of smaller touches (a deliberately irregular cursor blink, a
  couple of intentionally tiny "barely there" entries, and small
  revision-history flourishes on a few files).
- **Current status:** Live and deployed at
  thereisnoantimemeticsdivision.com. As of this writing, active feature
  development has paused. I've treated the project as essentially
  complete and moved into "let it sit as an archive" mode rather than
  continuing to expand it (this is something I know directly rather
  than something visible in the commit history, so take it as current
  as of when this document was written rather than something OC should
  assume will always be true).
- **Nothing is visibly in progress.** There are no open feature flags,
  no half-finished branches, and no TODO markers left in the code.

## 8. Numbers and facts

- One file, `index.html`, roughly 1,950 lines of HTML/CSS/JavaScript.
- Zero runtime dependencies, zero external network requests, zero
  build step.
- Roughly 80 distinct recognized command strings (including aliases
  that point to the same content), covering archive-file entries,
  fictional personnel entries, and assorted administrative/utility
  responses.
- Built over 8 calendar days (July 9 to July 17, 2026), across 29
  commits.
- Free to use. No account, no login, no payment, no limits on visits.
- Supports desktop keyboard input and mobile touch input.
- English only.
- Licensed CC BY-SA 3.0 as a derivative work; no text from the novel is
  reproduced.
- Hosted on GitHub Pages with a custom domain and enforced HTTPS; the
  source repository is public on GitHub.
- A private, local automated test suite (not part of the public repo)
  runs well over a hundred checks against the live file before any
  change ships.

## 9. Glossary

- **UO / the Organisation:** The published novel's name for the secret
  agency the archive belongs to.
- **Unknown (U-XXXX):** The novel's term for one of the agency's
  catalogued anomalies, referenced on the site by a four-digit number.
- **The terminal:** The main black-screen, blinking-cursor interface at
  the root of the site. This is "the site" in almost every sense.
- **The archive:** What the terminal is presented as being, a
  searchable (but unindexed) record of the fictional organization's
  files.
- **Drift / decay:** My own shorthand for the site's core mechanic,
  where content and behavior shift over time and across visits rather
  than staying fixed.
- **Boot sequence:** The short, skippable retro startup animation shown
  before the blank terminal screen appears.
- **Idle whisper:** A quiet line of text the terminal occasionally types
  out and then deletes on its own after a period of inactivity.
- **/about page:** A separate, plainly styled, search-indexable page
  that explains what the project is, distinct from the terminal itself.
- **qntm:** Pen name of Sam Hughes, the author of the novel this project
  is inspired by.
- **SCP Foundation wiki:** The collaborative fiction project (Creative
  Commons licensed) the novel's underlying story originated from before
  being revised into a standalone published novel.

## 10. Likely questions and good answers

**What is this?**
It's a fan-made interactive website for the novel *There Is No
Antimemetics Division* by qntm. You type things you remember from the
book and a terminal answers you, and the whole thing is built to feel
unreliable and alive rather than like a static reference.

**Is it official, or affiliated with the author?**
No, not at all. It's an unofficial fan project. I'm not affiliated with
the author or the publisher, and I say that plainly on the site's
`/about` page.

**Is it free?**
Completely. No account, no payment, nothing to sign up for.

**How do I use it?**
Honestly, the best answer is: go to the site and start typing things
you remember from the book. There's no menu or instructions on purpose.
That's part of the design.

**Do I need to have read the book first?**
Yes, pretty much. It's built for people who've already read it. If you
haven't, it'll mostly look like a blank screen with a blinking cursor,
which is also kind of the point.

**What tech did you build it with?**
Plain HTML, CSS, and JavaScript. One single file, no frameworks, no
backend, no database. Everything runs in your browser.

**Why build it as one file instead of a real app?**
Partly as a constraint I enjoyed working inside of, and partly because
it genuinely doesn't need more than that. There's no user accounts, no
server logic required, nothing that benefits from a bigger stack.

**Does it use AI?**
No, there's no AI in the site itself. It's hand-written content and
hand-written logic. (Separately, I did use Claude, Anthropic's AI
coding assistant, as a collaborator while building it, if that's what's
being asked.)

**Does it track me or collect my data?**
No. Whatever it "remembers" about you lives in your own browser's local
storage and never reaches me or any server.

**Why does it feel so secretive, like it doesn't want to explain
itself?**
That's intentional. The whole idea is that the archive doesn't explain
itself to you, you have to already know things or go find them out,
the same way the book's own organization treats information. Explaining
it out loud would undercut the entire point.

**Can you tell me the hidden commands / exact odds of things
happening?**
No, and I'd rather not, even here. That's the one thing I've protected
consistently through the whole project. Go find them.

**Is there more content than what's publicly documented?**
Yes, deliberately so. Nobody who visits it, including me, has
necessarily seen everything it can do, because a lot of it is
randomized, time-gated, or depends on what you've already looked at.

**Why qntm's novel specifically?**
Because I'm a genuine fan of it, and because its whole premise, an
organization fighting things that erase themselves from memory, is
unusually well suited to a website that's allowed to be unreliable on
purpose. (needs Chris for the fuller personal reason.)

**Did the author know about this?**
As of this writing, I've reached out to qntm directly about the
project. I don't have anything further to report on that yet. (needs
Chris to update once there's a response.)

**Is this related to the SCP Foundation?**
Loosely, yes, by lineage. The novel started as a story in the SCP
Foundation's shared fiction wiki before being revised and published as
a standalone book. Where this project uses ideas that trace back to
that wiki, they're used and licensed the same way the wiki itself is,
under Creative Commons CC BY-SA.

**Is any of the book's actual text on the site?**
No. None of the novel's prose is reproduced anywhere in the project.
Everything you read on it is original writing set in the same fictional
world.

**How long did it take to build?**
About eight days of actual commits, start to finish, for the version
that's live today.

**Is it still being actively developed?**
Not right now. I've brought it to a state I'm happy with and I'm
treating it as a finished archive rather than something I'm continuing
to add to indefinitely. (needs Chris to confirm if/when this changes.)

**What was the hardest part to build?**
The things that had to work correctly without ever being observed
directly, basically all of the randomized and time-based behavior, and
making sure none of it silently broke as new content was added. That's
exactly why there's a private automated test harness checking well over
a hundred behaviors before anything ships, even though nobody visiting
the site will ever see that harness.

**Does it work on mobile?**
Yes. It wasn't part of the very first version, but mobile keyboard
support was added early on and it works the same way there as on
desktop.

**Isn't this just a glorified wiki page or a text adventure?**
I'd push back on that a little. A wiki page is static and tells you
everything up front. This is deliberately the opposite: it's designed
to be incomplete, to change on you, and to never fully resolve into a
checklist you can complete. That instability is the actual point, not
a missing feature.

**What are its limitations?**
It's a single static page with no backend, so there's a hard ceiling on
how "smart" or dynamic it can ever be, everything it does has to be
achievable with client-side JavaScript and the visitor's own browser
storage. It's also entirely English, and it's genuinely made for people
who've read the specific novel, it's not built to onboard someone who
hasn't.

**What's next for it?**
Nothing planned right now that I can point to in the code. (needs
Chris.)

**Can I see the source code?**
Yes, it's public on GitHub. The README there is written in the same
in-character voice as the rest of the project, so don't take it too
literally.

**Did you make any money from this?**
No. There's no monetization anywhere in the project, no ads, no
payment flow, nothing for sale (inferred from the complete absence of
any such code, but genuinely confident here).

## 11. What OC should NOT say

- Do not list specific hidden commands, numbers, or phrases that
  trigger content, beyond the general categories already described
  here (archive files, personnel entries, administrative commands).
- Do not state the exact probability, percentage, or trigger condition
  of any randomized or time-gated behavior.
- Do not describe the specific content, wording, or outcome of any
  individual easter egg, hidden feature, or rare event.
- Do not confirm or deny specific guesses a visitor makes about what a
  particular command does. Redirect them to go try it on the site
  instead.
- Do not say whether qntm has responded to outreach, beyond what's
  explicitly confirmed above, since that may change and shouldn't be
  stated speculatively.
- Do not imply this project is official, licensed, endorsed by, or
  produced in collaboration with the author or publisher. It isn't.
- Do not state visitor numbers, revenue, or growth metrics. None of
  that is tracked or known.
- Do not speculate about future features or a roadmap that isn't
  confirmed above.

## 12. Questions for Chris

1. What's the personal story behind why you specifically wanted to
   build this, beyond being a fan of the book? Was there a specific
   moment or passage that sparked it?
2. Do you want OC to be able to mention that you emailed qntm directly,
   or keep that private until there's something concrete to report?
3. Roughly how much traffic or attention has the site actually gotten?
   Is that something you'd want OC to be able to speak to at all, even
   vaguely?
4. Can you confirm the exact publication details of the novel (I
   believe it was revised and published as a standalone book around
   November 2025, which is why the site has a feature tied to November
   11, but I'd like that confirmed precisely rather than asserting it
   from memory)?
5. Is "actively paused" the right way to describe where development
   stands, or would you rather OC say something else (for example,
   "complete" rather than "paused")?
6. Is there anything from the private design document (the one kept
   out of the public repo) that you'd specifically want folded into
   this file, even in generalized, non-spoiler form?
7. Do you want this file to ever be updated automatically alongside
   future commits, or is it meant to be a point-in-time snapshot you'll
   refresh manually?
8. Should OC actively encourage visitors to go try the site themselves
   when asked detailed questions, or is there a different call to
   action you'd prefer?
9. Is there a specific tone you want OC to take about this project
   that's different from your other three projects, given this one is
   a personal/creative work rather than a product?
