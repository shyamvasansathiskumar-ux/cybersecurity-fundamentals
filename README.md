# cybersecurity-fundamentals

The base layer underneath the other two repos. [`redteam-log`](https://github.com/shyamvasansathiskumar-ux/redteam-log) is AI-specific red-teaming; [`ai-governance-portfolio`](https://github.com/shyamvasansathiskumar-ux/ai-governance-portfolio) is governance writing. This one is neither — it's general security discipline: networking, web application security, cryptography, and classic CTF work. AI security without this underneath it is trivia. This repo is where that gets fixed.

**Shyam Kumar** · CSE, Rajalakshmi Engineering College · started September 2026

## Why a third repo instead of folding this into redteam-log

Because the two are genuinely different skills read by genuinely different people. A recruiter checking AI red-teaming credibility wants to see OWASP LLM Top 10 and MITRE ATLAS. A recruiter (or a technical interviewer) checking whether the fundamentals are solid wants to see subnetting, TCP/IP, the classic OWASP Top 10, and cryptography basics — the things that would be assumed knowledge in *any* security role, AI-focused or not. Keeping them separate means either audience finds exactly what they're checking for, instead of wading through the other lane to get there.

## Layout

| Folder | What goes here |
|---|---|
| `01-networking/` | Subnetting, TCP/IP, packet capture — lab notes, one file per topic |
| `02-web-security/` | Classic OWASP Top 10 (web, not LLM) — hands-on against a deliberately vulnerable app |
| `03-cryptography/` | Fundamentals and exercises: hashing, symmetric/asymmetric encryption, common attacks on weak implementations |
| `04-ctf-writeups/` | General CTF work (picoCTF, OverTheWire, etc.) — not AI-specific, that's what `redteam-log` is for |

## Standard for anything here

1. **Show the command, not just the conclusion.** "Used nmap" is not a log entry; the actual flags and what they were chosen for is.
2. **Log what didn't work.** A wrong guess with the reasoning behind it is worth more here than a clean success with none — it's the only evidence of how you actually think through a problem.
3. **Every entry says what it would mean for a real system.** One sentence bridging the finding to actual risk. That bridge is the whole point of the AI-governance track this repo supports; it needs to be a habit here first, on simpler ground.

## Progress

| Area | Status |
|---|---|
| Networking fundamentals | Not started |
| Web security (OWASP Top 10) | Not started |
| Cryptography | Not started |
| CTF write-ups | Not started |

*Started empty on purpose — see the templates in each folder. This table gets checked off as real work lands, not backfilled.*
