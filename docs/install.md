# Install

## Requirements

- Claude Code **2.1.118+**
- Node 20+
- A git repository
- On Windows, if you use PowerShell: let it run the scripts Node installs, once per user. Otherwise PowerShell refuses `npx` with *running scripts is disabled on this system*:

  ```powershell
  Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
  ```

## Getting started

```bash
mkdir my-project && cd my-project
git init
npx halfcycle
```

In Windows PowerShell, separate the first two commands with `;` instead of `&&`:

```powershell
mkdir my-project; cd my-project
git init
npx halfcycle
```

Then open the folder in Claude Code and run `/halfcycle-setup`, and answer the questions it asks about your project.

**Accept the workspace trust prompt when Claude Code shows it.** The install writes a credential helper and a set of hooks that are shell commands, so Claude Code asks before it runs them. It is expected.

**A second prompt follows — it is not the trust prompt again.** After you accept the trust prompt, Claude Code asks whether to use a new MCP server. The cursor starts on *Continue without using this MCP server*; choose to use the server instead, or Halfcycle will not connect.

You will get a board link on install. That is where the work becomes visible to anyone who is not you.
