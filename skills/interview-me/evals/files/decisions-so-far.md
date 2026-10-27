# Backup rule for the NAS: where we landed

Settled (Sam, 2026-10-18):
- Retention: 7 daily, 4 weekly, 6 monthly snapshots.
- Tool: restic, repository on the NAS at `/volume1/backups/restic`, encrypted with the existing repo password in the vault.
- Schedule: nightly at 02:30, started by the NAS's own scheduler.
- A failed run is retried once, 30 minutes later. A second failure is a failure.
- A restore test of the newest monthly snapshot runs on the first of each month.

Open:
- Who gets told when a nightly run fails twice in a row, and how fast. Not decided.
