# Privacy and security

The agent reads a person's email, so what it reads, stores and sends is designed as carefully as the ranking.

## What it reads

- **Read-only Gmail access** through the Gmail API, and nothing broader. It can't send, change or delete email.
- **Only emails from LinkedIn's newsletter sender,** matched exactly, so a forwarded email that merely mentions the address is ignored.
- **Each email is cut at its footer before anything is read.** The footer holds the unsubscribe link and personal account tokens. The parser refuses any email whose footer it can't find, instead of guessing.

## What it never stores

- **Unsubscribe links, personal tokens and tracking codes.** These are never stored, logged or printed. A newsletter's public page number is not personal and may be kept, but the parser still refuses any field that carries unsubscribe parameters.
- **Email text and summaries never go into the code repository.** Data lives in the user's own Google Sheet, and local data folders are excluded from git.
- **Secrets stay out of code:** the AI key and the link-signing secret live in Apps Script's Script Properties.

## Where data goes

- Each user's copy runs in their own Google account. There is no central server.
- The only text that leaves the account is the newsletter text sent to the AI model, under the user's own key.
- The script may contact only the AI provider's address, enforced by an allowlist in the project manifest.

## Test data

The parser's regression test uses ten real emails, cleaned **inside Google** before anything is exported:
- the footer is replaced with a made-up one;
- every link's tracking values are replaced with made-up values;
- images are replaced with placeholders.

An end-to-end test runs made-up emails carrying fake "real" tokens through the export and import, and fails if any token survives. With the cleaning deliberately broken, the export refused to write.

## Issues found by automated security review, and fixed

1. **A tracking code in a diagnostic log.** A layout-inspection log printed a tracking code from an unsubscribe link. The log now prints links as host and path only.
2. **Incomplete redaction in exported sample emails.** Some tracking values survived the cleaning. The cleaning now covers every link above the footer, and the export checks its own output.
3. **A prompt-injection path.** Email text is wrapped in frame tags before being sent to the model. A newsletter containing nested tags could have rebuilt a closing tag and placed instructions outside the frame. The tag removal now repeats until nothing changes, and a test with nested tags fails on the old code and passes on the new.

The damage from the third issue would have been limited, because every model reply is checked against the allowed topics, formats and lengths. It mattered more once other people would run the product on their own inboxes.
