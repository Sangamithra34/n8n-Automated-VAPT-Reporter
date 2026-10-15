# Passive VAPT Scanner with AI Report

An n8n workflow that takes a target URL from a form, runs passive security checks, scores the findings by severity, has an AI agent write a pentest-style report, and sends the report to Telegram.

## What it does

1. **Target URL Form**: the user submits a URL (and a Telegram chat ID for delivery).
2. **Validate URL**: normalises the input and extracts the domain. Invalid input goes to **Notify Invalid Input**.
3. **Passive checks (run in parallel)**
   - HTTP status and redirect chain
   - Security headers (HSTS, CSP, X-Frame-Options, X-Content-Type-Options, Referrer-Policy) and information disclosure (Server, X-Powered-By)
   - Cookie flags (Secure, HttpOnly, SameSite)
   - robots.txt and sitemap.xml
   - SSL and DNS resolution
4. **Merge**: combines all check results into one item.
5. **Severity scoring (Code node)**: classifies each finding as Critical, High, Medium, Low or Info, and calculates an overall risk rating.
6. **AI Agent report writer**: acts as a senior penetration tester and produces a report with fixed sections (7 in the live run).
7. **HTML formatter**: builds `VAPT_<domain>_<date>.html`.
8. **Delivery**: sends the file to Telegram.

## Setup

- **Credentials:** a Telegram Bot API credential (used by "Send Report via Telegram" and "Notify Invalid Input") and an AI model credential for the agent.
- **Form URL:** `https://incident-ticket-bot.app.n8n.cloud/form/vapt-scanner`. This becomes live only after the workflow is published. Use the test URL until then.
- **Telegram chat ID:** enter your own chat ID in the form.

## Error handling

- Invalid or unreachable URLs are rejected at validation and reported via Telegram.
- HTTP check nodes continue on failure, so one failed check does not stop the scan.



