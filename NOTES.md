# Patch exercise notes

## Summary of changes

Fixed the native SQL search query so archived, title/description, and status filters apply together (parentheses around the OR). Removed an artificial `Thread.sleep` in the task API that slowed empty and short searches. Corrected the React data hook so failed requests clear loading and successful retries clear errors. Reset pagination to page 1 when search or status filters change. Aligned `db/queries/search_tasks.sql` and the Oracle reference package with the same WHERE logic.

## What I chose not to change

I left in-memory pagination after a full DB fetch, invalid-status 500 responses, fetch race handling without AbortController, and search debouncing. Those need broader design or are lower impact for this read-only demo. I did not bump dependencies or restructure the app.

## Biggest remaining risk

Every search still loads all matching rows into the JVM before slicing pages. That is fine for seeded data but will not scale; pagination and filtering should move into SQL with limits/offsets and proper indexes.

## AI / tools

Used Cursor (Claude) to explore the repo, reproduce API bugs with curl, implement a small focused diff, run smoke tests, and draft this file. I verified behavior against the running backend rather than accepting suggestions blindly.
