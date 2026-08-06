---
name: email-voice
description: Learn the user's real email writing voice, then draft replies and new emails that sound like them instead of generic AI. Use this skill WHENEVER the user asks to write, draft, reply to, respond to, or follow up on an email, says "email this person", "write back to", "draft a reply", "respond to this email", "follow up with", or pastes an email they need to answer. ALSO trigger when the user says "set up my email voice", "learn my voice", "create my email voice", or "update my email voice". Always show drafts for review first. Never send, reply, modify, or delete any email automatically.
---

# Email Voice

This skill has two jobs:

1. **Voice Setup (first run):** Study the user's real sent emails and build a personal voice profile saved as `EMAIL-VOICE.md`.
2. **Drafting (every run after):** Read that voice profile and draft emails that sound exactly like the user, not like AI.

**Before drafting, also read `AVOID.md`** in this skill folder. It catalogs the corporate, AI, and templated voices a draft must NOT sound like. Every draft must pass the gut check at the bottom of that file.

## HARD RULES (never break these)

1. **Draft only. Never send.** Always show the draft in chat and let the user review it. Do not send, reply, forward, modify, or delete any email on your own. If a Gmail tool could send or change mail, do not call it for this skill. Gmail access is READ ONLY here.
2. **Never invent a voice.** If there is no `EMAIL-VOICE.md` yet, run Voice Setup first. Do not guess what the user sounds like.
3. **Draft clean.** If the user's real emails have typos, capture the style but never copy the typos.

## STEP 0: CHECK FOR A VOICE PROFILE

Every time this skill triggers, first look for `EMAIL-VOICE.md`:

- If the user has a connected folder, look there (and in any `memory` or root location they use).
- If found → skip to DRAFTING MODE.
- If NOT found → tell the user: "I don't know your email voice yet. Give me 2 minutes to learn it from your real emails." Then run VOICE SETUP.

## VOICE SETUP (first run)

### 1. Get their real sent emails

**Preferred: Gmail connector (read only).**
- Search the user's SENT mail. Pull 15-20 recent sent emails that the user actually wrote (skip auto-replies, forwards with no added text, and one-word replies).
- Try to get a mix: emails to people they know well, emails to strangers or formal contacts, asks, follow-ups, and thank-yous.
- If the Gmail connector is not connected, ask the user to connect it (Settings > Connectors), OR offer the fallback below.

**Fallback: paste.**
- Ask the user to paste 10-20 sent emails into chat. More is better. Remind them to grab a mix of casual and formal ones.

### 2. Analyze the emails

Study them for:

- **Greetings:** What do they actually open with? ("Hey Name," / "Hi Name," / no greeting?) Does it change for strangers?
- **Sign-offs:** Their go-to close. Do quick replies skip the sign-off?
- **Tone:** Warm or reserved? Direct or soft? Formal or casual? How does it shift with the audience?
- **Sentence rhythm:** Short and punchy, or longer? One idea per line, or paragraphs?
- **Signature words and phrases:** Words they use over and over. Phrases that are clearly "them."
- **Punctuation habits:** Exclamation points? Em dashes or never? Emoji?
- **How they handle situations:** Asks, follow-ups, saying no, negotiating, thank-yous.
- **Things they NEVER do:** Phrases or formats that appear nowhere in their emails.

### 3. Write EMAIL-VOICE.md

Save a file called `EMAIL-VOICE.md` in the user's connected folder (ask them to connect one if needed, so the profile survives between sessions). Use this structure:

```
# [Name]'s Email Voice
Built from [N] real sent emails on [date].

## TONE
## GREETINGS
## SIGN-OFFS
## SENTENCE STYLE & RHYTHM
## WORDS & PHRASES THEY USE
## HOW THEY HANDLE SITUATIONS
(asks, follow-ups, negotiating, pushback, thank-yous)
## NEVER DO
(personal rules, e.g. "never uses em dashes", "never says Best regards")
## QUICK STYLE EXAMPLES
(3-5 short real snippets from their emails)
```

### 4. Confirm with the user

Show a short summary of what you learned ("Here's your voice in 5 bullets...") and ask if it sounds right. Fix anything they push back on, then update the file. Tell them: "Done. From now on, just ask me to draft any email and it'll sound like you. Say 'update my email voice' anytime to redo this."

## DRAFTING MODE (every run after setup)

1. Read `EMAIL-VOICE.md` and `AVOID.md` before writing anything.
2. Read what the user is replying to (pasted thread, or Gmail read-only for context).
3. Ask for missing key facts: who it's to, how well they know them, the main point or ask.
4. Match greeting and sign-off to the relationship and the thread's energy, per the voice profile.
5. Write the draft in their voice. Lead with the point. Keep it tight.
6. **Show the draft in chat for review.** Offer to tweak tone, length, or details. Never send.

## UPDATING THE VOICE

If the user says "update my email voice" or "that doesn't sound like me", re-run Voice Setup (you can pull fresh sent emails) and rewrite `EMAIL-VOICE.md`. Their voice profile is theirs to change anytime.
