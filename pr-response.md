# PR Response Doc — CineLog Watchlist Feature

## AI Usage

Briefly describe how you used AI during this project.

---

## Comment 1 — Rename

**What I did:**
Renamed `save_to_watchlist()` to `add_to_watchlist()` throughout the project to match the naming convention used by the collection service.

**How I verified:**
Ran the full test suite successfully.

---

## Comment 2 — Deduplication

**What I did:**
Added a duplicate check in `add_to_watchlist()` and created an `AlreadyInWatchlistError` exception. The function now raises the exception instead of creating duplicate watchlist entries.

**How I verified:**
Added a unit test that attempts to add the same film twice and confirmed only one watchlist entry exists. All tests passed.

---

## Comment 3 — Missing test

**What I did:**
Added a unit test to verify that `add_to_watchlist()` raises `FilmNotFoundError` when a nonexistent `film_id` is provided.

**How I verified:**
Ran the full test suite and confirmed the new watchlist test passed along with all existing collection tests (7 tests passed).

---

## Comment 4 — Default visibility

**My position:**
I chose `public=True` as the default because CineLog is designed as a social film tracking application where users can discover movies through other users' watchlists.

**Reasoning:**
Making watchlists public by default encourages sharing and makes it easier for users to discover recommendations without requiring additional setup.

**Tradeoff acknowledged:**
This approach reduces privacy by default. Users who prefer private watchlists would need a setting to change the visibility, so adding a privacy option in the future would be a good improvement.

---

## Comment 5 — Sort order

**My position:**
I would sort the watchlist by date added (newest first).

**Reasoning:**
Most users want to quickly see the movies they recently added, making recent entries easier to find and manage.

**Engagement with reviewer's point:**
Alphabetical order is useful when searching for a specific title, but I agree that ordering by date added provides a better default experience because it reflects recent user activity.

---

## Comment 6 — Rebase

**What conflicted:**
The main branch had been updated to use UUIDs for film IDs instead of integers, so the watchlist code needed to be updated to match the new model.

**How I resolved it:**
I rebased my branch onto the latest main branch, updated the watchlist code to use the new UUID-based implementation, and ensured all references were consistent with the current codebase.

**How I verified no conflict remains:**
I ran the full test suite after the changes and confirmed that all tests passed successfully.

---

## PR Description

This pull request adds a watchlist feature that allows users to save films they want to watch later and retrieve their saved watchlist. The feature includes a new `WatchlistEntry` database model, watchlist service functions, and REST API endpoints for adding and viewing watchlist items.

The default visibility for new watchlist entries is **public (`public=True`)** because CineLog is intended to support sharing and film discovery between users, while recognizing that a future privacy setting would improve flexibility.

The default watchlist order should be **date added (newest first)** because users are more likely to want quick access to the films they recently saved rather than an alphabetical list.

### Manual Testing

1. Start the Flask application with `python app.py`.
2. Add a valid film to a user's watchlist through the watchlist endpoint.
3. Verify that the watchlist entry is successfully created.
4. Attempt to add the same film again and confirm that a duplicate entry is prevented.
5. Attempt to add a film using a nonexistent `film_id` and confirm that the appropriate error is returned.
6. Retrieve the user's watchlist and verify that the saved film is returned correctly.
7. Run `pytest tests/ -v` and confirm that all seven tests pass.

---

## AI Usage

I used AI as a development assistant throughout this project. Specifically, I used it to:

- Explain the reviewer comments and clarify what changes were being requested.
- Identify the files that needed to be modified for each review comment.
- Suggest an approach for implementing duplicate-entry prevention in the watchlist service.
- Help design unit tests by following the same testing patterns already used in `test_collection.py`.
- Review my implementation and point out any missing updates after making changes.

I did not copy AI-generated code directly into the project without verification. After each change, I reviewed the code, made adjustments where necessary, and ran the project's test suite to verify that the implementation worked correctly. All seven tests passed after the requested changes were completed.