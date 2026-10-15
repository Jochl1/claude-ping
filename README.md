# claude-ping

A GitHub Actions workflow that sends one tiny Haiku message on a fixed schedule, so your Claude
**5-hour usage window** starts on a grid you know.

## Why

The 5-hour window starts with your first message. If you usually burn through it in a few hours
and then sit locked out with weekly credit left, starting the window *before* you sit down means
it resets partway through your session, and you get two windows in one sitting. If you start right
when the ping fires, nothing is lost. A ping inside a window that is already running does nothing.

Default grid: **08 · 13 · 18 · 23, Europe/Berlin**.

## Cost

Nothing, as long as the repo is **public** (Actions minutes on standard runners are free there).
In a private repo every run is billed as at least one minute, and the grid uses 8 runs a day.

## Setup

1. Fork or copy this repo (keep it public).
2. On your own machine, with Claude Code installed and logged in to your subscription:
   ```
   claude setup-token
   ```
   This prints a long-lived token (about a year). It uses your Pro/Max subscription, not the API.
3. In the repo: **Settings → Secrets and variables → Actions → New repository secret**,
   name `CLAUDE_CODE_OAUTH_TOKEN`, value the token. Never commit it.
4. **Actions** tab → enable workflows if asked → **ping → Run workflow** once to test.
5. Check `/usage` in Claude Code (or claude.ai → Settings → Usage): a reset time should now show.

## Your own grid

Edit `TIMEZONE` and `GRID` in [`.github/workflows/ping.yml`](.github/workflows/ping.yml), then
make the cron list both UTC hours for every grid hour (UTC offset in winter and in summer). The job
only pings when the local hour is on the grid, so daylight saving needs no changes.

## Good to know

- GitHub's scheduler often runs 5–30 minutes late. Fine for this.
- GitHub disables scheduled workflows after 60 days without activity in the repo. It emails you
  first; any commit, or **Enable workflow** in the Actions tab, turns it back on.
- When the token expires the run fails and GitHub emails you: make a new one with step 2.
- The workflow only runs on its schedule or by hand, never on pull requests, so nobody else can
  use your secret.

## Licence

MIT
