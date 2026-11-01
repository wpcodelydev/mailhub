# MailHub: instructions for GitHub Copilot

MailHub is a Windows desktop email + cold-outreach app. It is a fork of Mailspring (Electron + React + TypeScript).
Mail sync is done by a separate native program, "Mailsync", built from our own repo wpcodelydev/mailhub-sync and unpacked into app/ by scripts/postinstall.js from mailsync-local/mailsync.tar.gz.

READ CLAUDE.md in the repo root first. It explains the architecture: the UI reads the SQLite database read-only, every write goes through a Task queued with Actions.queueTask and executed by Mailsync, and MailsyncBridge (which runs the sync processes) lives in the main window only.

## Rules (always follow)
1. License: Mailspring is GPLv3. Never delete LICENSE.md or the original copyright notices.
2. Put new code in NEW packages (app/internal_packages/mailhub-*) and new files. Keep edits to existing Mailspring files small and list every upstream file you edited, so future upstream merges stay easy.
3. No hardcoded business values. Sequence steps, day intervals, send-time windows, industry list, daily-limit options, ramp-up numbers: all must come from a settings schema with defaults, editable in the UI.
4. UI language is English.
5. Never connect to Mailspring's servers (getmailspring.com, mailspring-builds, id.getmailspring.com). No analytics or telemetry. No Mailspring ID, no plans.
6. Never inject scripts or automate anything inside social-app webviews (LinkedIn, WhatsApp, Telegram...). They are for manual use only.
7. Pure logic (scheduling, time zones, templates, state machines, parsing) goes in plain CommonJS .js files with JSDoc types and unit tests using Node's built-in runner (node --test), so it runs without Electron. All tests must pass before you finish. Show me the test output.
8. Secrets (Google service account JSON, tokens, passwords) must never be committed. Store only file paths in settings, and add the patterns to .gitignore.
9. Do not add dependencies without telling me why. Prefer small, well-known, maintained packages and list them with their license. Never hand-edit app/package-lock.json and never commit its changes unless you added a dependency.
10. Windows 10/11 and PowerShell. Use path.join for paths.
11. Be honest. If something cannot be verified without running the app, say so clearly. Do not claim a feature works if you have not run its tests.
12. Work on the git branch named in the task. Commit with clear messages. Do NOT push, merge, or force-push unless the task says so.
13. End every task with a REPORT: (a) files added/changed, (b) exact manual test steps I can follow, (c) what is NOT done or uncertain.
