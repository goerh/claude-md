# Claude Code guidelines (CLAUDE.md)

My working agreements for [Claude Code](https://code.claude.com): a single `CLAUDE.md` that tells Claude how to plan, write, test and talk about code.

## What's in it

- **Foundation**: four rules against common LLM coding mistakes: think before coding, simplicity first, surgical changes, goal-driven execution.
- **English in the code, my language in chat**: code, docs and app texts in English; the conversation in whatever language you write in.
- **Starting a new project**: ask about platforms, language and how the program is started before writing any code.
- **Make it public-repo ready**: `.gitignore`, README, declared dependencies, MIT license, and no secrets or personal data.
- **Include tests**: tests for every feature or fix, checked with mutation testing.
- **Logging**: log files with a capped size, errors kept apart from normal output.
- **Keep it simple**: a solution that keeps getting more complicated is a sign the approach is wrong.
- **Work in modules**: split code by what it does.
- **Comments**: short comments above complex code that explain why.
- **Follow the beaten path**: use what the tool or OS already offers, and check its documentation first.
- **Leave my running system alone**: don't interrupt running processes, and restore everything after testing.
- **Keep answers short**: the conclusion first, details on request.
- **Ask first, then change**: propose changes and wait for a yes.
- **Don't assume, prove it**: run and measure instead of reasoning about behaviour.

## How to use

Claude Code loads `CLAUDE.md` files automatically.

- **For all your projects:** copy `CLAUDE.md` to `~/.claude/CLAUDE.md` (on Windows: `%USERPROFILE%\.claude\CLAUDE.md`).
- **For one project:** put it in the root of that project.

Start a new session to pick up the file.

Some rules are personal, such as using both macOS and Windows. Change those to suit you.

Claude Code only imports local files (with `@path/to/file`), not URLs. That's why the Foundation section is copied in rather than linked.

## Credits

The Foundation section comes from [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) (MIT), which is derived from Andrej Karpathy's observations on LLM coding pitfalls.

## License

MIT
