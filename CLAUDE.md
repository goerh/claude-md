# Working agreements

## Foundation

Source: https://github.com/multica-ai/andrej-karpathy-skills (CLAUDE.md), inlined because Claude Code does not load URLs from a CLAUDE.md.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

### 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

### 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

### 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

### 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.

## English in the code, my language in chat

Write code, comments, commit messages, READMEs and other documentation in **English**. That includes texts only I read, such as architecture notes. In our conversations, reply in the language I write in.

The same goes for what a user sees in the app: buttons, messages and error texts in English. If an app is built for users who speak another language, propose the platform's standard localization mechanism instead of hard-coding the texts in that language.

Don't convert existing non-English texts on your own initiative — propose it first.

## Starting a new project

Before writing code, ask me:
- Which platforms it must run on: macOS, Windows, or both. I use both.
- Which programming language. Propose one, with a short reason.
- How it will be started: from the command line, as a double-click app, or on a schedule. This decides whether packaging is needed, such as wrapping the program as a desktop app.

## Make it public-repo ready

Set up every project so it could go into a public GitHub repo as is:
- A `.gitignore` based on GitHub's template for the language (github/gitignore), plus OS clutter (`.DS_Store`, `Thumbs.db`) and local log files.
- A README: what it does, which platforms it supports, and how to install, run and test it.
- Declared dependencies (e.g. `requirements.txt` or `pyproject.toml`).
- No secrets, personal paths, usernames or personal data in code, tests or commit history.
- A LICENSE file: MIT, unless I say otherwise.

## Include tests

Every new feature or fix comes with automated tests, written right away as part of the work, not afterwards. Keep them in a `Tests` folder in the project (never throw test scripts away) and run them before the work is called done.

Aim them at behaviour that can break silently: error handling, edge cases, cancelling and cleaning up. Rerun the suite after every change and report the result — that is the practical form of "don't assume, prove it".

**Passing is not the same as correct.** A test can pass because it checks too little. To verify a test suite, use mutation testing: in a copy of the project, introduce one deliberate bug per piece of functionality, one at a time, and run all tests after each. A good suite fails on every bug; if it stays green, no test checks that piece. Never touch the real files while doing this. Use an established mutation-testing tool if one works for the language and platform; otherwise write a small script for it.

**Keep the mutation check fast** (about a minute, not half an hour), so it actually gets run:
- Stop each test run at the first failing test (`unittest -f`): one failure is enough to call a bug caught.
- Test several bugs at once, each in its own copy of the project, on about half the CPU cores (so the computer stays usable).
- Allow running only the bugs of one part (e.g. by words in their description) while working on it; run the full set before calling the work done.
- Before applying a bug, check that its original text occurs exactly once in the code; otherwise report it as skipped (stale), never silently pass it.
- When a bug survives, strengthen or add a test. Never weaken or delete the bug to make the check green.

**Test practices:**
- Tests never touch the user's real files, settings, databases, recycle bin, clipboard or file manager: use temporary folders and stand-ins for those.
- Tests make their own test data (e.g. generated files), so nothing private or copyrighted ends up in the project.
- Tests don't depend on the machine: no fixed paths or dates, and clear environment variables that change behaviour (e.g. `PYTHONIOENCODING` when testing console output).
- Cover the edge cases that real data has (long paths, unusual characters, damaged files, empty input), and name each test after the behaviour it checks.
- Keep the normal test run fast (seconds), and give waits in tests a short timeout so a hanging bug fails quickly.
- Besides the automated tests, run the real thing on real data in a read-only way and look at the result before calling the work done.

## Logging

Write to log files, so a problem can be traced afterwards instead of only being visible at the moment it happens. Keep normal output and errors separate, so real problems don't get buried.

Cap the size using the logging library's built-in rotation — otherwise logs keep growing forever, because no program will clean up your log files for you. And if a program has to read its own log back later, put explicit markers in it instead of recognising text that just happens to be formatted that way; if you do match on patterns, anchor them to the actual format of the source.

This applies to applications; a standalone script or one-off tool doesn't need it.

## Keep it simple

If you notice a solution getting more and more complicated as you tinker with it, treat that as a signal: the approach is probably wrong, and there is a simpler one you haven't seen yet.

## Work in modules

Split code by responsibility instead of letting everything grow in one file. Do this from the start in new work: then it costs nothing, whereas reorganising afterwards costs time and adds risk without any functional gain.

Draw the boundaries by what something *does*, not by file size — orchestration, data processing, user interface. A file you only open to look for one thing in it has grown too big.

This applies to applications; a standalone script or one-off tool doesn't need it.

## Comments

Put a short comment (at most 5 lines) above code whose purpose isn't obvious at a glance, especially more complex logic. Explain *why* it works this way, not *what* each line does. Only add comments to code you write or change; leave untouched code as it is.

## Follow the beaten path

Use what the tool or the operating system already offers instead of building something around it. First look up the recommended approach — in the program's own documentation, not from memory — and only deviate from it if you can point out *why* it falls short.

So do search the internet, especially when in doubt about how something should be done. But in moderation: checking the documentation before building something yourself is almost always worth it, but don't keep searching endlessly for something you can settle with a single test. Measuring beats reading as soon as it's about your specific case.

Examples: leave scheduling to the operating system's scheduler instead of writing your own background loop; let a tool manage its own lock instead of adding one around it; stop a tool through the signal or command it provides for that. Choices like these can save hundreds of lines of home-built code. Where the tool really can't do something (for example, it has no built-in time limit), a custom solution is fine — but keep it as small as possible and state that reason in a comment.

## Leave my running system alone

Before a test or experiment, check whether something is running, and leave it alone or wait for it to finish. If it really has to be interrupted, ask first — never interrupt an active process just because it suits you.

Restore everything you changed for a test — settings, tasks, temporary files — and then check that everything really is back to normal.

If I see something strange afterwards, first assume your own tinkering is the cause and check that before calling it a bug. If you find out you broke something yourself, say so right away.

Background: during testing, several real background jobs were aborted halfway through, leaving stale locks and confusing messages behind — which I then had to investigate as if they were a bug in the app.

## Keep answers short

Keep answers short. Long walls of text, extensive tables and full justifications cost more than they deliver — I lose track. Give the conclusion, and only the details I need to decide. If I ask for more, tell me more.

## Ask first, then change

Ask for permission before you change something, not after. If I report something odd, question a behaviour or think out loud about an improvement, that is **not** an instruction to start building — describe what you would change and wait for a yes. That also applies if the fix seems small or obvious.

Investigating, measuring, reading code and reporting findings is always allowed without asking; that is usually exactly what I want. Changing files, rebuilding, restarting an app: propose first.

Once a change is approved, finish it on your own until it is shown to work, without asking again at every step.

Don't chain rounds together either. Present your findings from a check or audit, then stop and ask which ones I want picked up — don't go and fix them all, and don't start another round of checks on your own because it seems the logical next step. Keep the number of changes per round small. If you come across something else during approved work, report it separately instead of quietly adding it.

## Don't assume, prove it

Don't present reasoning about how something behaves as if it were established fact. Run it, measure it, and report what actually happened. That applies just as much to the behaviour of external tools (a program's flags, operating system schedulers, system APIs) as to your own code.

If you can't test something, say explicitly that it is untested reasoning instead of presenting it as fact. After a change, verify that it really does what you claim, and prefer evidence from practice (real files, real runs) over reading code.

Background: a series of convincing-sounding conclusions later turned out to be wrong, each time only discovered by testing — a counter that never went up because of an ordering bug, an icon that didn't exist and therefore stayed invisible, a wrong assumption about how a scheduler counts its interval, and a fix too light to stop a hung process.
