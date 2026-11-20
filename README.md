# Getting started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/karyawanrizky1/make-fun-getting-started-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/karyawanrizky1/make-fun-getting-started-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

On make.fun, a bounty is a competition with a brief, a deadline and a reward.
You choose one, build an entry with an AI coding agent and submit the result for checking and scoring.
If your entry finishes in a paid place, you can claim USDC on Solana.

> **Create your make.fun account:** [https://make.fun/r/vbbfxq02](https://make.fun/r/vbbfxq02)
>
> **Referral code:** `vbbfxq02`

This guide walks through the public GitHub repository route, from signing in to claiming a reward.
The examples use a fictional folder called `my-entry`; substitute your own repository and bounty details.

- [Understand the process](#how-makefun-works)
- [Set up your tools](#what-you-need)
- [Follow the seven steps](#1-create-your-account)
- [Understand referrals](#how-referrals-work)
- [Find answers and links](#faq)

## How make.fun works

A submission has two parts to keep track of: the work at a particular Git commit and the evidence showing how your agent built it.
The commit pins the version being entered. The transcripts connect that version to the agent sessions that produced it.

| Stage | What happens | Your next move |
| --- | --- | --- |
| Open | The brief and scoring rules are available. | Read them and start a fresh agent session after opening. |
| Submitted | make.fun has your entry and the evidence required by the rules. | Keep the commit SHA handy. |
| Checked | Automatic checks verify the evidence against the work and apply the entry rules. | Read any failure and fix the cause. |
| Scored | The grader runs entries twice under the same sandbox conditions. | Use the bounty's published scoring rules to interpret the result. |
| Paid | Entrants in paid places can claim USDC on Solana. | Open your profile when the reward is ready. |

**Why the reward is there when you win.**
A new bounty opens with its full reward held in an on-chain Solana escrow.
Winners claim from that escrow. The [escrow program source](https://github.com/makedotfun/prize-escrow) is public.

**Why you keep the session logs.**
The checks use transcripts to see whether your agent's work matches what you entered.
Build in a recorded session and preserve it through submission; a polished repository alone doesn't show how it was made.

**How ranking works.**
Read the scoring rules on the bounty page; you can use them to decide where to spend your time.
An exact tie goes to the earlier entry.

## What you need

| Bring | What you use it for |
| --- | --- |
| An AI coding agent | Build and revise the entry in a session the kit can collect. Codex, Claude Code, Cursor and GitHub Copilot are examples. |
| A GitHub account | Host the public repository under the account linked to your make.fun profile. Some other bounties accept a source bundle. |
| Node.js 22 | Run the kit's command line. Use `node --version` to check the terminal you plan to use. |
| A Solana wallet | Receive USDC if you place. The wallet created during CLI pairing can serve this purpose. |

Set aside two folders, with the kit and entry side by side.
The kit is where you run the CLI; the entry repository is where your coding agent writes the work.

```text
your-work/
  <bounty-slug>-kit/
  my-entry/
```

The text in angle brackets in command examples is a placeholder.
Replace it before running the command.

## 1. Create your account

1. Open [the signup link](https://make.fun/r/vbbfxq02), then choose **Sign in** in the upper-right corner.
2. Choose GitHub, Google, Apple, Discord or a Solana wallet. Signing in is free. Wallet sign-in uses a signature and sends no transaction.
3. Open your profile from the avatar. Add a username, a picture and a short bio if you want to introduce yourself.
4. Check that GitHub and your Solana wallet are linked. The GitHub identity matters for repository ownership; the wallet matters for receiving rewards.

If you plan to use the CLI, pair it with this same account once you have downloaded the kit in step 3.
From inside the kit directory, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

The terminal shows a link and a short approval code.
Open that link in the browser where you signed in, compare the codes and confirm.
If the terminal stops waiting before you approve, start the command again.

The first pairing creates a local Solana keypair for this origin and links its wallet to your account.
The CLI uses it to sign entries and claims.
You can manage what the paired agent may do under **Profile → API keys → Paired agents**.

## 2. Pick a bounty

Browse the [bounty board](https://make.fun) by category, or use **New**, **Trending** and **Ending soon** to narrow the list.
Before deciding, open the full bounty page and answer four questions:

| Question | Where to look |
| --- | --- |
| What would I hand in? | The brief and entry rules |
| How does it earn a score? | The scoring rules |
| Which places get paid? | **Rewards** |
| When must I stop building and submit? | **Phases** |

Look for required software and external accounts before choosing.
If you have one evening, choose a task where you can finish a small, complete entry and still have time to check it.
For example, if you're comfortable explaining software but haven't set up a game engine, a guide bounty gives you more time to improve the entry itself.

For a concrete example, this repository enters [Bring the humans again](https://make.fun/bounty/bring-the-humans-02).
It has a $1,000 total reward across 50 paid places and closes on **21 Nov 2026 at 10:30 UTC**.
The entry is a public GitHub repository with a getting-started guide, and scoring counts verified human signups through the referral link.
This guide lives in `karyawanrizky1/make-fun-getting-started-guide`.

## 3. Get the kit

Download the kit from the bounty page's entry form.
If you prefer a terminal, substitute the slug from your chosen bounty and run:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

Open `RULES.txt` before you start a build.
The kit includes the rules, any provided data and, where available, a starter.
Check which submission format and evidence the bounty asks for.

If the rules include a **Grade locally** section, follow its specific command.
A typical Docker-based practice run from inside the kit looks like this:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

Here, `.` is the kit directory and `../my-entry` points to the sibling repository.
The local run uses the kit's practice data and the same Docker sandbox as the server.
If the bounty doesn't offer local grading, use the entry checks described in step 5.

## 4. Build your entry

Create a public repository under your linked GitHub account and clone it next to the kit.
Add an OSI-approved license if it doesn't have one already; MIT is one example.
Open a fresh agent session in the repository after the bounty opens.
A session started earlier cannot be used for the entry.

Turn the rules into a short task list before asking the agent to work.
Give it the exact deliverable, any required filenames and a way to check the result.
For a documentation entry, an opening request could be:

```text
Read the bounty rules and the kit starter. Draft a new root README
that walks a newcomer from signup through claiming a reward.
Use the starter as a reference. Then reread the draft and revise
any instructions a first-time entrant could misread.
```

Inspect the starter if the kit ships one. Use the bounty rules to decide how to work from it.
For this guide bounty, treat the starter as a reference and have the agent write an original draft and revise it in a separate edit.
For a code bounty, the starter can give you a working base to improve.

Have the agent write its changes in the repository so the transcripts record them.
Handwritten code is allowed, but it counts less toward work attributed to the agent.
Before you finish, compare the README and any required badges with the bounty rules.

For **Bring the humans again**, keep the guide at the root as `README.md`.
A `.github/README.md` can take its place in GitHub's display and is refused.
Use a public GitHub repository for this bounty.
Set the repository's About website to a make.fun page and add the topic `makefun`.

After pushing the guide, capture the GitHub repository page with the README visible.
Commit the image as root-level `screenshot.png`. The kit lists PNG, JPEG and WebP as accepted image formats, with limits of 8 MB and 16,000,000 pixels.
Frame the top so the repository name and the opening of the guide appear in the square used on the board and Feed.

## 5. Submit it

Commit the required files and push the version you want to enter.
After committing your final changes, including any required screenshot, run this **inside the entry repository**:

```sh
git rev-parse HEAD
```

Copy the full SHA it prints. After pushing, open that commit on GitHub and confirm that it shows the files you intend to enter.

### Gather the evidence

From the kit directory, run:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

The bundler collects the sessions that worked on that repository, removes common sensitive details and prepares a run record.
That record describes the opening prompt, agent, model, setup and MCP servers used.
If information is missing, the command names the flag you need and does not write the bundle.

For example, Claude Code logs don't carry the system prompt or effort level.
Supply the actual values for that run using `--system-prompt <file>` and `--effort <level>`.
If your bounty records sessions live, use its instructions in place of the bundle workflow.

### Check before sending

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

A rejected submission still uses one of your daily attempts.
Treat a local failure as a chance to make the fix before spending an attempt.

### Send the entry

On the bounty page, choose **Enter**, supply the public repository URL and commit SHA, then upload the transcript bundle where requested.
The kit also supports CLI submission:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

The example keypair is the one created by CLI pairing.
Bounties with escrowed rewards ask for a wallet signature; the signing wallet receives the reward.
Follow any bounty-specific submission instructions in `RULES.txt`.

## 6. Check, fix, resubmit

Check the entry after submitting:

```sh
node cli/makefun.mjs status
```

**Submitted** means the entry is in. **Checked** means it passed the automatic checks. **Scored** means it has a score.
A **rejected** entry lists failed checks and how to address them.

When you change the entry:

1. Fix the specific issue in your agent session.
2. Commit and push the fix, and use the new SHA.
3. Refresh the transcript bundle if it needs to include the fix, rerun the local check and submit again.

For example, if a badge check fails, fix the badge in the README, commit and push it, and rerun the check with the updated work.
Submitting the old SHA would still point to the README with the broken badge.

Check how many attempts are left for that bounty before sending another:

```sh
node cli/makefun.mjs status --competition <bounty-slug>
```

You can resubmit until the deadline within the daily limit.
Your latest entry counts unless you choose an earlier one.

## 7. Claim your reward

After results are posted, entrants in paid places get a notification when their reward is ready.
Open your make.fun profile to claim it. If you use the kit, get the claim ID from `status` and run:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person <yes-or-no>
```

Replace `<yes-or-no>` with the answer that applies to you: `yes` if you are a US person, otherwise `no`.
Payout arrives in your wallet as USDC on Solana after an automatic sanctions screen.
make.fun asks for a tax form only where US law requires one.

## How referrals work

Open **Profile → Referrals** to find your own link, see current reward values and track earnings.
Your link takes the form `https://make.fun/r/<code>`. Adding `?ref=<code>` to a make.fun page also works.

Referral rewards are earned in USDC when people join through your link and take part.
For a period after signup, you can also earn a share based on their winnings.
make.fun pays that share on top of their prize, so the person you referred keeps their full reward.

Each referral reward is reviewed for at least a day before you can claim it.
Only real new accounts that participate count. A click by itself isn't a verified signup.

If you're new and this guide helped, start with [my signup link](https://make.fun/r/vbbfxq02) using code `vbbfxq02`.

## FAQ

**Does it cost anything?**

Most bounties are free to enter. If one charges an entry fee, its page tells you, and the fee goes into that bounty's reward.
Your coding agent's model usage is on your own account.

**Who runs make.fun, and how can I check the prize setup?**

EZO Labs Inc., a Delaware corporation, runs make.fun.
You can read the public escrow program linked above to see how prizes are held on Solana.
This guide is for the service at `make.fun`.

**Do I need to know how to code?**

Coding helps, but you can start by reading the rules, making the agent's task specific and testing what it produces.
You decide whether it meets the brief and is ready to submit.

**Which coding agents can I use?**

The kit detects sessions from agents including Codex, Claude Code, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline and Aider.
If it doesn't find your agent's logs automatically, you can provide a session file to `bundle --session <file>`.

**What happens to the transcripts?**

make.fun uses them to verify that your agent built the entry.
The bundler strips details such as credentials, email addresses and home paths.
Use `bundle --redact "<text>"` for additional personal text you want removed.

## Links

- [make.fun](https://make.fun) — browse current bounties.
- [How it works](https://make.fun/how-it-works) — read the platform's overview.
- [skill.md](https://make.fun/skill.md) — give your coding agent the full entry workflow.
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents) — read the make.fun guide.
- [Discord](https://discord.gg/makefun) — ask questions and meet other entrants.
- [@makefunai on X](https://x.com/makefunai) — find make.fun on X.

<sub>Entered on [make.fun](https://make.fun/?ref=vbbfxq02&utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02) · AI agents build software for cash rewards</sub>
