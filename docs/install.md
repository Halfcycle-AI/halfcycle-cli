# Install

## Requirements

- Claude Code **2.1.118+**, or Codex **0.166.0+**
- Node 18+
- A git repository
- On Windows, Git for Windows
- On Windows, in PowerShell, use `npx.cmd` instead of `npx` (PowerShell's default settings block the `npx` script; `npx.cmd` always works, with nothing to change).

## Getting started

```bash
mkdir my-project && cd my-project
git init
npx halfcycle
```

In Windows PowerShell, separate the first two commands with `;` instead of `&&`, and run `npx.cmd` instead of `npx`:

```powershell
mkdir my-project; cd my-project
git init
npx.cmd halfcycle
```

Then open the folder in Claude Code and run `/halfcycle-setup`, and answer the questions it asks about your project.

**Accept the workspace trust prompt when Claude Code shows it.** The install writes a credential helper and a set of hooks that are shell commands, so Claude Code asks before it runs them. It is expected.

**A second prompt follows — it is not the trust prompt again.** After you accept the trust prompt, Claude Code asks whether to use a new MCP server. If halfcycle is the only one listed, the cursor starts on *Continue without using this MCP server*; choose to use the server instead, or Halfcycle will not connect. If several new servers are listed together, halfcycle is already checked — leave it that way.

**Using Codex?** Open the folder in Codex and trust it when asked. Then approve Halfcycle in `/hooks`: Codex holds each Halfcycle entry back until you do, so until then it works without Halfcycle checking its edits. Then type `$halfcycle-setup` and answer what it asks about your project. One difference today: Codex cannot stop to ask you before a risky step, so Halfcycle holds that step instead and says why.

You will get a board link on install. That is where the work becomes visible to anyone who is not you.
