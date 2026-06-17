# ChatGPT Account Generator - Developer Guide

[English](README_DEV.md) | [한국어](README_DEV_KO.md)

This document explains the internal structure and runtime behavior of `Chatgpt_Account_Generator` for developers.

## Project Goal

The project is a Python-based automation tool that generates email accounts, waits for OTP mail in a local Mailcow IMAP mailbox, and completes registration.
It supports both a web UI and a CLI, and stores results in local JSON files.

## Scope

- Account generation
- OTP mail lookup
- Account registration
- Account verification
- Result persistence and backups
- Web UI progress tracking
- CLI-based execution

## Directory Layout

- `main.py`: entry point
- `backend/`
  - `cli.py`: CLI menu and input handling
  - `service.py`: account generation orchestration
  - `config.py`: environment variables and paths
  - `storage.py`: JSON persistence, deduplication, backup, delete
  - `jobs.py`: web job state
  - `account_verification.py`: verification flow
  - `http_client.py`: HTTP request helpers
  - `quota.py`: quota-related logic
  - `debug_log.py`: debug logging helpers
  - `account_creation/`: email generation, OTP waiting, registration
- `frontend/`
  - `app.py`: Flask app, routes, progress computation
  - `i18n.py`: localization helpers
  - `webui/`: templates and static UI assets
- `data/`: results, email DB, job state, backups
- `logs/`: runtime logs and debug output
- `.env`: runtime configuration

## Runtime Flow

### 1. Startup

`main.py` tees `stdout` and `stderr` into `logs/latest.log`.
The default mode is the web UI; passing `cli` starts the CLI.

### 2. Email Generation

`backend.service.create_accounts()` creates account payloads via `create_account_generator()`.
If a suffix is provided, it is appended to the local part of the email address.

### 3. Deduplication

`backend.storage.is_email_used()` checks both `data/email-db.json` and `data/accounts.json`.
Already used emails are skipped before registration.

### 4. OTP Waiting

`create_otp_waiter()` polls the local Mailcow IMAP mailbox for OTP mail.
Timeout and polling frequency are controlled by `OTP_TIMEOUT` and `OTP_POLL`.

### 5. Registration

`register_account()` performs the registration request.
Progress logs include account numbers and email addresses so the UI can show meaningful progress.

### 6. Persistence

Successful accounts are saved through `save_account()` and `save_email_to_db()`.
Backups are created before file writes and are pruned according to `BACKUP_KEEP_LIMIT`.

## Data Flow

### Inputs

- `.env`
- CLI input
- web UI requests
- Mailcow IMAP mailbox

### Processing

- email generation
- deduplication
- OTP waiting
- registration
- verification

### Outputs

- `data/accounts.json`
- `data/email-db.json`
- `data/result.txt`
- `data/jobs.json`
- `logs/latest.log`
- `logs/latest-imap.eml`
- `logs/resend-post.log`

## Core Logic

### `backend/service.py`

- This is the orchestration layer for account creation.
- Success is determined by the presence of `accessToken`, `userId`, and `email`.
- It accepts an optional logger callback for progress output.

### `backend/storage.py`

- Creates backups before writes.
- Prevents duplicate email storage.
- Handles account field updates and deletion.

### `frontend/app.py`

- Converts job logs into human-readable progress text.
- Progress calculation differs by job type.
- Separates account creation and verification status display.

### `backend/cli.py`

- Provides a menu-driven interface.
- Collects account count and suffix input.
- Prints export results.

## Configuration

Values loaded from `backend/config.py`:

- `BATCH_SIZE`
- `OTP_TIMEOUT`
- `OTP_POLL`
- `EMAIL_DOMAINS`
- `MAILCOW_IMAP_HOST`
- `MAILCOW_IMAP_PORT`
- `MAILCOW_IMAP_SSL`
- `MAILCOW_IMAP_USERNAME`
- `MAILCOW_IMAP_PASSWORD`
- `MAILCOW_IMAP_MAILBOX`
- `MAILCOW_IMAP_SCAN_LIMIT`
- `MAILCOW_IMAP_LOG_LIMIT`
- `BACKUP_KEEP_LIMIT`
- `CHATGPT_QUOTA_URL`

## Implementation Notes

- The `account_creation/` implementation should be documented according to the actual files that exist in the repository.
- Backup files are created frequently during writes, so code and docs must stay aligned with that policy.
- Web UI status messages rely heavily on log message strings, so log wording changes may require UI updates.
- `main.py` opens a fresh log file on every run.
- When troubleshooting failures, check both the logs and job state to identify the last successful step.

## Important Facts

- This is a Python implementation.
- It supports both web UI and CLI workflows.
- OTP comes from a local Mailcow IMAP mailbox, not from an external mail API.
- Results are stored in JSON and duplicate emails are rejected before persistence.
- Parts of the previous README may not match the current code structure.

## Future Work

- Keep the README and developer guide aligned with the codebase.
- Inspect the actual files under `backend/account_creation/` if more detail is needed.
- Provide a separate English document set where appropriate.
- Update this documentation whenever routes or CLI behavior changes.

