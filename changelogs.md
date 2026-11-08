# Changelogs

## 1.1.6
- V1 compatibility, and scoped server stop/restart handling.



## 1.1.5
- Fixed: a failed shell spawn could leave the installer waiting forever; command start errors are now handled.
- Server detection now asks OpenCode's `/global/health` endpoint first, so another service on the port is no longer treated as OpenCode, and it still works when the web UI asks for a password.
- Server start now falls back to `nohup` when `setsid` is unavailable in the Alpine sandbox.
- Command is registered through Acode's `commands` API (with a legacy fallback), and `exec` returns `true`.
- Unloading the plugin now removes its sidebar entry and stops the background server, so reloads don't leave duplicates or orphan processes.
- Command repair now also checks npm's global `bin` and additional package entry points.
- Server start timeout increased to 120s.
- Manifest now declares `changelogs` and ships `changelog.md`.

## 1.1.4
- Fixed: a half-finished npm install (package folder present but no `bin/opencode`) could never repair itself. If the command is still missing after install, the plugin now removes the leftover OpenCode folders and the dangling link, then reinstalls cleanly.

## 1.1.3
- Fixed: npm could report success but leave no `opencode` command (missing link in `/usr/local/bin`). The plugin now re-links it from npm's global folder, and falls back to `npm install -g --force` if needed.
- On launch, a missing command is re-linked first, before any reinstall.

## 1.1.2
- Fixed "export: not found" (exit code 127): Acode runs commands with `exec`, so shell built-ins failed. Every command now runs through `sh -c`.
- This also fixes the false "not installed" detection that caused repeated installs, and the stale-file cleanup that could delete a working `/usr/local/bin/opencode`. The cleanup now only removes a broken link.

## 1.1.1
- Fixed: the plugin re-installed OpenCode on every launch. Commands now run with the full Alpine PATH (`/usr/local/bin` etc.), so an already-installed OpenCode is detected.
- Launch flow is now: server already running → open UI; else installed → skip install and run `opencode serve --port 4096 --hostname 127.0.0.1`; else install first.
- The log now shows which step was taken ("OpenCode 1.x.x is installed — skipping install", "Running: opencode serve …").

## 1.1.0
- One-tap launch: tapping the OpenCode icon now does everything automatically (check, install, repair, start, open). No terminal commands needed.
- Auto-repairs interrupted installs: removes a stale `/usr/local/bin/opencode` link and retries with `--force` if npm fails.
- Auto-repairs the "Database is not empty and has no session table" error by moving the broken database to `~/oc-backup` and restarting.
- Installed check now runs `opencode --version`, so broken leftovers are no longer treated as installed.
- Live "still working… 42s" status during slow silent steps.
- proot "can't sanitize binding" warnings are hidden from logs.
- Error screen shows the last lines of the real output, plus a "Reset data & retry" button.
- Start step stops any old server before launching a fresh one.

## 1.0.0
- Initial release: runs `opencode serve` on 127.0.0.1:4096 inside Acode's Alpine terminal and embeds the web UI.
- Auto-installs `opencode-ai` via npm, restart, update and stop buttons.
