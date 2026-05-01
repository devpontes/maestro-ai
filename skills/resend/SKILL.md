---
name: resend
description: >
  Send transactional and marketing emails via the Resend API.
  Supports HTML and plain text, custom from addresses, reply-to,
  attachments, and email templates.
type: mcp
version: "1.0.0"
mcp:
  server_name: resend
  command: npx
  args: ["-y", "resend-mcp@latest"]
  transport: stdio
env:
  - RESEND_API_KEY
categories: [email, communication, automation]
---

# Resend Email

## When to use

Use Resend when you need to send emails programmatically — transactional emails, newsletters, notification emails, or any automated email workflow. Resend provides a simple API with high deliverability rates.

## Instructions

### Key capabilities

- Send emails with HTML or plain text body
- Use custom `from` addresses (must be a verified domain in Resend)
- Support for `reply-to`, `cc`, `bcc`, and attachments
- Use pre-built templates from the Resend dashboard

### Best practices

- Always use a verified sender domain for better deliverability
- Test with a single recipient before sending bulk campaigns
- Include both HTML and plain text versions for compatibility
- Keep subject lines under 60 characters
- Avoid spam trigger words in subject and body

### Error handling

- If `from` domain is not verified: add domain to Resend dashboard first
- If email bounces: check the recipient address is valid
- If rate-limited: reduce sending frequency or contact Resend support

## Available operations

- **Send Email** — Send a single email to one or multiple recipients
- **List Emails** — Retrieve sent email history
- **Get Email** — Check the status of a specific sent email
- **Create Template** — Create reusable email templates
