# Verification record

Added 2026-10-04 when this fork was created. It documents what was actually
checked, so nobody has to take the description on trust.

## What this tool is

A **client**, not an engine. `whatsapp-osint.py` makes HTTP requests to a
**third-party paid service**, `whatsapp-osint.p.rapidapi.com`, and pretty-prints
the JSON it returns. All information comes from that service. The script performs
no lookups of its own, touches no WhatsApp infrastructure, and requires no
WhatsApp account.

Endpoints it calls: `/wspic/b64` (profile photo), `/about`, `/bizos` (business
verification), `/devices` (linked-device count), `/privacy`, plus a "doublecheck"
endpoint described in the README. Six in total.

## What was verified here

| Check | Result |
|---|---|
| Repository is real and popular | Yes — 1,151 stars, 222 forks, Python, public |
| Commit history | **Single commit.** `created_at` and `pushed_at` are both 2025-10-16, so it has had no maintenance since the day it was published |
| License | **None declared.** Absent a license this is all rights reserved by default — do not redistribute or vendor it into another project without permission |
| Outbound network calls in the source | Only `whatsapp-osint.p.rapidapi.com`. No other host is contacted |
| Telemetry, phone-home or exfiltration | None found |
| Dangerous constructs | No `eval`, `exec`, `subprocess`, `os.system` or raw sockets |
| Dependencies | `requests`, `python-dotenv`, `colorama` — all benign and widely used |
| Hardcoded credentials | None |
| Behaviour without an API key | Refuses cleanly with "RAPIDAPI_KEY not found in .env" |

## Honest limitations

1. **It needs a paid third-party key.** Nothing works without registering with
   RapidAPI and subscribing to the WhatsApp OSINT API. The tool is only as good
   as that service, and that service is outside this repository's control.
2. **The upstream article's own testing was partly unsuccessful.** It reports
   that images are only available for business accounts, that a test account had
   no custom status, and that the "full OSINT" query returned only a message
   about API maintenance. Expect partial results rather than a complete profile.
3. **The service could change or disappear at any time**, and no upstream
   maintenance has occurred since publication, so no fix would be forthcoming.
4. **Results are not independently corroborated by this tool.** It reports what
   one commercial API says. Treat any output as a lead to verify, not as fact.

## Legal and ethical use

- WhatsApp's Terms of Service do not authorise automated querying of account
  metadata. Using a third-party service to infer it may breach those terms.
- Under the GDPR and comparable regimes, a phone number is personal data.
  Running this against someone requires a lawful basis, and in most
  jurisdictions an individual's own consent or a legitimate-interest assessment.
- It must not be used for harassment, stalking, doxxing, intimate-image
  investigations, or on any person who has not consented, unless you are
  operating under a documented lawful authority such as a signed engagement,
  a penetration-test authorisation, or a law-enforcement mandate.
- The upstream author states the tool is for legitimate cybersecurity
  investigations, authorised audits, educational OSINT, and analysis with
  explicit consent. That is the correct scope and this fork does not widen it.

## Running it

```bash
python3 -m venv myvenv && source myvenv/bin/activate
pip3 install -r requirements.txt
cp .env.example .env      # then put your RapidAPI key in .env
python3 whatsapp-osint.py
```

## Useful next step, not done here

The script has no offline mode, so it cannot be exercised or tested without a
live paid key. Adding a `--dry-run` that prints the exact request it would make,
without sending the number, would make the tool reviewable and testable for
free.
