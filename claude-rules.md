### Claude Global Rules, Imports, Commands, and Hooks

- **Report scope**: Global Claude config, referenced imports, custom commands, and hooks
- **Sources reviewed**:
  - Global config: `/Users/risenowrise/.claude/CLAUDE.md`
  - Imports referenced there: `/Users/risenowrise/syn/web/era/base-truth.md`, `@styles/globals.css` → `/Users/risenowrise/syn/web/era/src/styles/globals.css`, `$ZSH/.env` → `/Users/risenowrise/syn/zsh/.env` (not found)
  - Custom commands: `/Users/risenowrise/.claude/commands/*`
  - Hooks: `/Users/risenowrise/.claude/hooks/**`
  - Shell config: `/Users/risenowrise/.claude/cc.zsh`

---

### Key directives from global `CLAUDE.md`
- **Env var awareness**: Prefer `$VARIABLES` and `echo $VAR` over searching.
- **Web dev workflow**:
  - Prefer existing dev server on `:4747`; otherwise run `bun dev` on `:4848` in the background and clean up afterwards.
  - Tailwind v4: there is no `tailwind.config.js`.
  - Read `@styles/globals.css` for project-wide CSS.
- **Identity**: Use Claude Code, not Claude Desktop.
- **MCP config**: Ensure all MCP servers are top-level entries in `$MCPL` (no nesting); validate JSON and lint.
- **Python**: Use `uv` exclusively.
- **Fixing**: Run `fix` to zero; don’t claim success without matching output.
- **File writes**: Inspect existing structure first; maintain consistency.
- **Safety**: Never deploy/modify production; avoid Rosetta installs; use tmux for long-running/sudo flows.
- **Keys**: API keys live under `$AI`/`$API`.
- **Timestamps**: Convert Unix epoch to human dates before analysis.
- **Repo scope**: Work inside the instructed repo only.

---

### Imports referenced by `CLAUDE.md`
- `/Users/risenowrise/syn/web/era/base-truth.md`
  - Trigger.dev requires your own webhook endpoints; cloud offering doesn’t expose inbound webhooks.
  - OAuth flows: only via self-hosting; implement app-side OAuth (e.g., NextAuth) when needed.
  - Bottom line: Trigger.dev handles background jobs; you must host the web layer for webhooks/OAuth.
- `/Users/risenowrise/syn/zsh/.env` (referenced as `$ZSH/.env`)
  - Not found during scan; confirm presence if needed for path variables and aliases.
- `@styles/globals.css` → `/Users/risenowrise/syn/web/era/src/styles/globals.css`
  - Tailwind v4 `@import` and `@theme` usage; no tailwind config file.
  - Defines brand palette, dark mode vars, custom breakpoint `--breakpoint-xs: 420px`.
  - Provides numerous `@utility` classes (page bg, spacing, radius, hero, section paddings) used across ERA.

---

### Custom commands (from `~/.claude/commands/`)
- **/add-server**: Add/sync MCP servers from `$MCPL` and enable tools in `$MCPS`; includes known server→tool mappings; Python helper at `add-server.py`.
- **/!! (Verification Checklist)**: Rigorous preflight for any verifiable output (syntax, assumptions, docs, testing).
- **/consistent**: Frontend consistency checker for Tailwind v4 patterns, spacing, typography, and component semantics.
- **/crawl**: Web crawling and YouTube transcription via tmux sessions; use `--sequential` when crawling multiple URLs.
- **/fix**: Run linters/formatters iteratively until zero errors; then commit with detailed commentary.
- **/free**: Manual process cleanup with time-based sampling before kills; safety criteria defined.
- **/freemux**: Safe tmux session/window cleanup via `uv run ~/.claude/commands/freemux.py` with strict idle/content checks.
- **/id**: Background agent copies the current `cc --resume <session-id>` to clipboard.
- **/image**: Image processing toolkit overview (webp/pic/icons/focus) with AVIF-first workflow and examples.
- **/pb**: Copy exact, immediately executable commands to clipboard (uses temp file + pbcopy for multiline).
- **/prime**: Prime repository knowledge (tree, core docs, configs) and report architecture/stack.
- **/q**: Read-only mode; analysis only, no writes.
- **/test**: Comprehensive testing standards by project type; integrates `/!!`.
- **/todo**: Create todos via TodoWrite with priority parsing.
- **/tree**: Live codebase index with depth rules and navigation workflow.
- **/undo**: Selective rollback of files changed in last N commits (skips symlinks).
- **/cmd-fix** (update-cmd.md): Framework to modify existing slash commands while preserving ecosystem patterns.
- **/fixpaths** (update-paths.md): Systematically update `$ENV` and aliases after path changes; verify with `x` and alias tests.
- **/uv.md**: UV inline script compliance checker; can scaffold verification script and templates.
- **/zclean**: ZSH history de-spam + dedupe with backup and metrics.

Notes:
- Command aliases are wired in `/Users/risenowrise/.claude/cc.zsh` as `alias '/name'='cc "/name"'`.

---

### Hooks
- **dev/autofix.md**: Retired. Explains why auto-fix hooks don’t work (no comms channel/race conditions). Suggests manual `fix`, IDE on-save, or notification-only hooks.
- **voice hooks** (ElevenLabs preferred; OpenAI TTS disabled; pyttsx3 fallback):
  - `voice-compact.py`: Speaks “Compacting in {repo}”.
  - `voice-done.py`: Speaks completion (“Finished and ready in {repo}”).
  - `voice-input.py`: Optional TTS when user input is needed (skips generic waiting message).
  - `voice-subtask.py`: Speaks subtask completion.
- **tts utils**:
  - `elevenlabs_tts.py`: ElevenLabs Turbo v2.5 playback with nonsense-filter and volume logic.
  - `openai_tts.py`: Present but not preferred (disabled by selection logic).
  - `pyttsx3_tts.py`: Offline fallback.
  - `get_system_volume.py`: macOS volume detection and TTS volume mapping.

---

### Shell config (`~/.claude/cc.zsh`)
- **Entry function**: `cc` wraps `claude` CLI with model selection (default sonnet), MCP config (`$MCPL`), added dirs (`$HOME/.claude`, `$HOME/syn`), and a strict disallowed-tools list (prevents prod deploys, disk ops, tmux kill, destructive git, etc.).
- **Env**: `MCP_TOOL_TIMEOUT=600000`, `MAX_MCP_OUTPUT_TOKENS=50000`, `CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL=true`.
- **Aliases**: All slash commands mapped to `cc` invocations; theme switchers `light()`/`dark()`; `cclog` to summarize latest Claude Code changelog; `cci` to install latest.

---

### Actionable reminders
- Confirm `$ZSH/.env` exists and is sourced (not found during scan).
- For Tailwind v4 projects, keep global styles in `src/styles/globals.css`; do not add a Tailwind config file.
- Use tmux for long processes and any sudo prompts; clean up sessions afterward.
- Never add providers/features not explicitly requested.

---

Last updated: 2025-08-08
