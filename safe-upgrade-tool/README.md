🧰 safe-upgrade — Because typing sudo apt update && sudo apt upgrade -y got old

A small Bash utility born out of laziness and refined out of caution.
I wanted a single command that updates my system safely — without the risk of an upgrade removing half my OS.

🧠 Motivation

I wrote this tool because I got tired of typing the same long commands every time I wanted to update my system.
But I also don’t like when “shortcuts” end up breaking things — so instead of an alias, I built a small script that actually checks what APT intends to do before letting it happen.

When I make something, I want it to work, and to work safely.
That’s why this tool:

Simulates risky operations first,

Refuses to run destructive ones, and

Keeps your system clean afterward.

It started as a time-saver, but turned into a small example of how good automation should look: lazy in execution, strict in safety.

⚙️ Features

Safe update flow – Runs apt update and apt upgrade normally, then simulates a full-upgrade before applying changes.

Removal detection – If APT wants to remove any packages, the script stops and warns you instead of blindly proceeding.

Explicit “unsafe” flag – The --full flag allows a real full-upgrade only when the simulation shows no removals.

Fail-fast Bash mode – Uses set -Eeuo pipefail to abort on errors, undefined variables, or broken pipelines.

Clean system afterward – Automatically performs autoremove --purge and clean to remove leftovers and free disk space.

Readable logs – Simulation results are stored in /tmp/full-upgrade.log for quick inspection.

🧩 Design Choices and Reasoning
Decision	Rationale
Bash, not Python or YAML automation	Shell scripts run everywhere without dependencies; ideal for quick system tools.
set -Eeuo pipefail	Prevents silent errors — if something breaks, the script stops immediately.
Simulation-first approach	Treats apt full-upgrade like a “transaction preview” — nothing happens until it’s confirmed safe.
Simple --full flag	Keeps UX explicit. The script will never perform a risky upgrade by default.
Use of tee	Logs all simulated output while still displaying it live — transparency over silence.
Auto-cleanup	Keeps machines tidy after upgrades, especially useful on long-lived VMs or homelabs.
🧰 Usage
safe-upgrade             # Safe mode: simulate and upgrade safely
safe-upgrade --full      # Apply full-upgrade only if simulation shows NO removals
safe-upgrade -h | --help # Display usage info


Example output:

REMOVALS found, packages that would be removed:
Remv linux-headers-6.14.0-29-generic [...]
Run with --full to apply full-upgrade when there are NO removals.

🧼 Cleanup Routine

After a successful run, the script performs:

sudo apt autoremove --purge -y
sudo apt clean


This ensures old kernels, dependencies, and cached packages are cleaned up automatically —
so you don’t have to remember to do it yourself.

🧪 Tested On

Ubuntu 24.04 LTS

Debian 12 (Bookworm)

Pop!_OS 22.04

Executed under non-root users with sudo privileges.


📜 License

MIT © Adrian Dubowski
