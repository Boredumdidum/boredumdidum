# TaskFlow Sync Plan (Google Drive appData + E2EE)

## TL;DR
Add end-to-end encryption with a passphrase, store encrypted tasks in Google Drive appData using Google OAuth, and sync automatically across devices with dataset-level last-write-wins.

## Estimate
- 6-10 hours total for me to do all coding plus a bug-fix/testing pass.
- You handle external setup (Google OAuth client + consent screen).

## User Setup (You)
- Create a Google OAuth client (Web).
- Enable Google Drive API.
- Add scopes for Calendar (existing) + Drive appData.

## Implementation Steps (Me)
1. Add sync state + dataset metadata (updatedAt, deviceId, lastSyncAt).
2. Implement encryption helpers (PBKDF2 + AES-GCM) and a versioned payload envelope.
3. Extend OAuth scope to include drive.appdata and add Drive appData read/write helpers.
4. Add sync flow (pull on load, push on change, debounce, last-write-wins).
5. Add UI controls (sync enable toggle, passphrase input, sync now, status).
6. Optional: store only encrypted tasks at rest in localStorage while sync is enabled.

## Verification
1. Device A: set passphrase, add tasks, confirm Drive file exists and is ciphertext.
2. Device B: enter passphrase, confirm tasks load and edits sync back.
3. Conflicting edits: confirm last-write-wins and toast warning.
4. Token expiry/revoke: confirm errors are surfaced and no data loss.

## Decisions / Assumptions
- Sync backend: Google Drive appData (personal use).
- Security: end-to-end encryption with a passphrase not stored.
- Conflicts: last-write-wins at dataset level.
- Always-online usage (no offline merge needed).

## Out of Scope (Unless Requested)
- Per-task merge, offline conflict resolution, or alternative backends (e.g., GitHub Gist, WebDAV).
