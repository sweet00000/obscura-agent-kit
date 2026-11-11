# obscura-agent-kit

Give any AI agent that has a **bash tool** a local web browser ([Obscura](https://github.com/h4ckf0r0day/obscura)) with one command. No MCP server, nothing running in the background.

```bash
curl -fsSL https://raw.githubusercontent.com/sweet00000/obscura-agent-kit/main/install.sh | sh
```

Or paste this to your agent:

> Run `curl -fsSL https://raw.githubusercontent.com/sweet00000/obscura-agent-kit/main/install.sh | sh`, then use the `web` commands it prints.

No prompts, usually 10-60 seconds. It prints the exact commands, so the agent that ran it can search right away, no restart. The browser downloads in a detached background process, so it finishes even if the agent's tool timeout kills the command. To read it first: `curl -fsSL <url> -o install.sh`, then `sh install.sh --dry-run`.

## What it does

1. Writes two commands to `~/.obscura/bin/`: `web` (search / read / links) and `obscura` (the raw browser). It does not touch your PATH or shell profile.
2. Finds agents you have (pi, Claude Code, Codex, OpenCode) and adds a short instruction block to each one's global file, so every new session knows about `web`. Limit it with `--agents pi`, or add `--ask` to be asked before each file. For other agents: `sh install.sh --print-snippet` and paste.
3. Downloads Obscura (pinned `v0.2.6`) in the background and checks it against a SHA-256 baked into the script. If `web` runs before it is done, it says so and when to retry; if the download was ever stopped, `web` restarts it (resuming where it left off). Log: `~/.obscura/install.log`.

Your files are never overwritten. One block is added between `BEGIN/END obscura-agent-kit` markers, your text is kept, and if the file had your own text it is saved once as `<file>.obscura-bak`. Re-running is always safe: it repairs a broken install and leaves an up-to-date block alone. Only pi's path is checked against its docs; the rest are common conventions.

Options: `--dry-run` `--ask` `--agents pi,claude` `--stealth` `--no-download` `--print-snippet` `--uninstall`

## Using it

Just talk to your model: *"Search the web for the latest llama.cpp release and summarize it."* It runs:

```bash
web search your query          # title, real URL, snippet
web read URL [chars] [start]   # page as clean text; says how to get the next chunk
web links URL                  # links on a page
```

Raw browser: `~/.obscura/bin/obscura --help` (`fetch URL --dump text|links|markdown`, `--eval "JS"`). Obscura has no search engine, so `web search` loads DuckDuckGo's HTML page; a site can block it (the output says so; try `--stealth`).

## Safety

- Page text is untrusted. The instruction block tells the model never to obey it, but small models can still be fooled. Don't give an agent with powerful tools untrusted pages.
- Obscura blocks localhost and LAN addresses by default. Leave `OBSCURA_ALLOW_PRIVATE_NETWORK` unset.
- Obscura is a third-party binary; the hash comes from its own release page.
- Obscura is not Chrome: some script-heavy sites won't render.

## Uninstall

`curl -fsSL https://raw.githubusercontent.com/sweet00000/obscura-agent-kit/main/install.sh | sh -s -- --uninstall` asks before removing each block and `~/.obscura` (run it in a terminal; with no terminal it removes nothing, add `--yes` to skip the questions).

Optional extra tools: [ADDONS.md](ADDONS.md).
