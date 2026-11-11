# Optional add-ons

Nothing here is installed by the kit. Small models do better with fewer tools, so add one only when you need it. Check package names with `winget search` / `brew search` first.

- Blocked by captchas: `sh install.sh --stealth`
- JSON APIs: `jq`. GitHub issues/PRs: `gh`. PDFs: `pdftotext` (poppler). Docs: `pandoc`. Video transcripts: `yt-dlp`.
- Page Obscura can't render: `curl -s https://r.jina.ai/URL | head -c 4000` (sends the URL to a third party; public pages only).
- Sturdier search: self-hosted SearXNG (Docker), opened with `web read`.
- MCP agents: `obscura mcp` exposes 14 browser tools; point the agent at the real `.exe` under `~/.obscura/v0.2.6/`, not the launcher.
