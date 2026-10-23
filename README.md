<!--
  Showcase README for the public repo arman-mahabad/kunji.
  The code lives in a private repo. This repo holds only this page.
  No pictures on purpose (claims row 17): the demo screens show things this page must not show.
  Add them only after a clean re-shoot, and every caption with a score must say "offline mock".
-->

<sub><a href="https://byarman.site">Made by Arman ↗</a></sub>

# Kunji

**A working prototype of an AI copilot that helps people write and check their replies. It never sends anything by itself, and it comes with a workspace for measuring how well it works.**

**Working prototype** &nbsp;·&nbsp; Runs on invented customer data &nbsp;·&nbsp; The code is private. This page shows how it works and how I check it.

It rests on two ideas. Agents will never sit down and write test sets, but an agent who edits a draft before sending has already shown a better reply. Kunji keeps that pair, and once a person approves it, it becomes a test case. And rules that must hold live in code, because an instruction in a prompt can be talked out of firing.

## The hard parts

### A judge that graded itself

Kunji has three AI jobs, each set up on its own: drafting replies, reading what agents sent, and judging test answers. Early on, the judge got the same model it was meant to test. On the offline mock that did no harm. With a real model, every benchmark would have been the model marking its own work, and it would still have looked like a fair comparison.

I moved the choice of judge into one place. It records why a judge was or was not used, and Kunji now refuses to score when the judge is the model under test. A test file holds that rule in place.

### A block that holds when the agent writes alone

Only three rules block a reply. Each one stops something a follow-up message can't take back, such as showing a customer's personal data. Everything else warns, because agents stop reading a guard that blocks too often.

Kunji never sends by itself. At send, the server runs the rules again on the exact text, so the block holds even when the agent typed the reply without any AI help.

### The same rules in two languages

A safety rule that works in Arabic and misses in English is a hole. Tests take each dangerous Arabic example in English and expect the same rules to fire at the same level. Kurdish has no expert review yet, so a Kurdish reply carries a warning saying so instead of coming back clean.

## How the AI is checked

- **Personal data stays home.** Names and numbers become placeholders before any text leaves, and come back on the agent's screen.
- **Every score names its source:** code, an AI judge or a person. An AI judge never scores alone. The scores reuse the guard's rules, so what Kunji measures and what it enforces can't drift apart.
- **Prompts are compared blind.** New prompt versions run beside the old ones in shadow mode, with the answers labelled A, B and C.
- **Answers come only from approved documents,** version by version. Search is plain keyword search, on purpose, so every answer can name its source.
- **When a model fails:** a daily spend cap, a circuit breaker, a timeout and two retries. Then Kunji falls back to its offline mock and labels the result.

## What an agent sees

One button. It drafts when the reply box is empty, improves a rough draft, or checks a finished reply without rewriting it. The draft lands in the normal reply box, and sending is always the agent's own act. Agents see only their own activity, and nothing ranks them.

## In numbers

- **1,473** automated tests in 46 files.
- **33** invented test cases, checked by an outside support specialist.

## My part

It was my idea and my decisions, and I directed the build. An outside reviewer checked the test cases and the wording of the rules.

**Models:** adapters for Anthropic, OpenAI and Ollama. The default is an offline mock, so it runs with no key.

**Built with:** `Next.js` `React` `TypeScript` `Prisma` `SQLite` `Tailwind` `Zod` `Vitest`

## Not done yet

- It has never handled a real conversation, and it refuses real customer text by design.
- No time saving measured. Its impact calculator starts at zero.
- It runs on a laptop. No hosting, no company sign-in.
- The Kurdish rules wait for a native speaker's review.

---

I can walk you through the code on a call: [arman@byarman.site](mailto:arman@byarman.site)

More of my work: [byarman.site](https://byarman.site) &nbsp;·&nbsp; [github.com/arman-mahabad](https://github.com/arman-mahabad)
