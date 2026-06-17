# ChatGPT Account Generator

[English](README.md) | [한국어](README_KO.md)

This project helps you make and save ChatGPT-related account data.
It can run in a web screen or in a simple command window.
If you are not used to technical tools, just follow the short steps below.

## What it does

- Makes account records
- Waits for OTP mail
- Saves the result on your computer
- Lets you export saved data
- Works in a web screen or a command window

## What you need

- Python installed on your computer
- This project folder
- Access to the mail system used for OTP

## Easy start

1. Open the project folder.
2. Install the needed files.

```bash
pip install -r requirements.txt
```

3. Start the app.

```bash
python main.py
```

4. If you want the text-only version, use this instead:

```bash
python main.py cli
```

## Simple usage

1. Enter how many accounts you want.
2. If needed, add a short email suffix.
3. Let the program make the account data.
4. Wait for the OTP mail.
5. Confirm the OTP.
6. Save or export the result.

## Saved files

- `data/accounts.json`
- `data/email-db.json`
- `data/jobs.json`
- `data/result.txt`
- `data/backups/`
- `logs/latest.log`
- `logs/latest-imap.eml`
- `logs/resend-post.log`

## Settings

If you need to change the mail connection or waiting time, copy `.env.example` to `.env`.
Most people can keep the default values.

## Notes

- This project saves data on your computer.
- Results are written in a simple file format.
- Backup copies are made automatically.

