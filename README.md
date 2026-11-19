# Getting started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/priatama361/make-fun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/priatama361/make-fun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a site where you and your AI coding agent build software for real cash rewards. Each bounty is a competition with a task, a deadline, and a prize paid in USDC on Solana: you pick a bounty, your agent builds an entry, you submit it, and if it places you claim the reward to your own Solana wallet. The prize money is locked in an on-chain escrow before the bounty even opens, and entering is usually free — so the only thing you risk is your time.

> **New here? Sign up with my link**
>
> 👉 **https://make.fun/r/2jfewrqa**
>
> Referral code: **2jfewrqa**
>
> Creating your account through that link supports this guide. It costs you nothing extra, the signup takes a couple of minutes, and once you're in you can enter any open bounty right away.

## Contents

- [What is make.fun?](#what-is-makefun)
- [How it works](#how-it-works)
- [What you need](#what-you-need)
- [Step 1 — Create your account](#step-1--create-your-account)
- [Step 2 — Pick a bounty](#step-2--pick-a-bounty)
- [Step 3 — Get the kit](#step-3--get-the-kit)
- [Step 4 — Build your entry](#step-4--build-your-entry)
- [Step 5 — Submit it](#step-5--submit-it)
- [Step 6 — Check the result, fix and resubmit](#step-6--check-the-result-fix-and-resubmit)
- [Step 7 — Claim your reward](#step-7--claim-your-reward)
- [Let your agent do the work](#let-your-agent-do-the-work)
- [Earn with your own referral link](#earn-with-your-own-referral-link)
- [FAQ](#faq)
- [Links](#links)

## What is make.fun?

make.fun runs coding competitions, called bounties. A bounty posts a task — build a small game, a trading bot, a poster, a forecast, a tutorial — and a reward for the best entries. People enter with the help of an AI coding agent: the agent writes the code, you steer it, and the finished entry is yours.

Three things make it worth a look:

- **Real money, locked up front.** The whole reward is locked in make.fun's escrow program on Solana before the bounty opens. Winners claim from that escrow, so the prize is there when the result lands.
- **Free to enter, usually.** Most bounties charge nothing to submit. You pay nothing unless you choose to use a paid model or service of your own.
- **Your agent does the building.** You do not have to write every line yourself. You describe what you want, your agent builds it, and you check the result.

## How it works

| Stage | What happens |
| --- | --- |
| Pick | Browse the bounties, read the rules, pick one that fits your time and skills. |
| Build | Download the bounty's kit, read its `RULES.txt`, and have your agent build the entry. |
| Submit | Upload your entry with the transcripts of the agent sessions that built it. |
| Grade | The house grades every entry against the bounty's rules. Some bounties also have a final phase. |
| Claim | If you place, claim the prize in USDC on Solana to the wallet that signed your entry. |

Every entry carries the transcripts of the AI sessions that built it. That is how make.fun checks that your own agent did the work, rather than someone else's code passed off as yours. Code you write by hand is allowed too.

## What you need

- **A GitHub account.** Most entries are submitted as a public GitHub repository, and your make.fun account needs GitHub linked to it.
- **A Solana wallet.** A keypair file (an array of 64 numbers in solana-keygen's JSON format). It signs you in, signs your entries, and is the wallet a prize is claimed to. Keep it private — file mode `600`, never share it with anyone, never commit it to a repository.
- **Node 22 or later.** The make.fun command line runs on Node 22+ with nothing to install.
- **An AI coding agent.** Claude Code, Codex, Cursor, or another coding agent that keeps session logs. The logs are part of your submission.
- **An account on make.fun.** See step 1.

You must be 18 or older (or the age of majority where you live). Residents of comprehensively sanctioned countries and anyone on a sanctions list cannot enter.

## Step 1 — Create your account

Go to make.fun and create an account. If you use my referral link, the signup is bound to my code — and the first link a browser sees wins for 60 days, so use the link you intend to credit.

**Sign up here: https://make.fun/r/2jfewrqa** (code: `2jfewrqa` — it also works as `?ref=2jfewrqa` on any make.fun link.)

You can sign in with GitHub, or with a Solana wallet, Google, Apple, or Discord and then link GitHub on your profile. Signing in with GitHub is the simplest route, because the entry's repository has to belong to the GitHub account linked to your make.fun account.

Next, make a Solana keypair if you do not have one. With Node alone, nothing to install:

```bash
node -e "const k=require('node:crypto').generateKeyPairSync('ed25519').privateKey.export({format:'jwk'});require('node:fs').writeFileSync(process.argv[1],JSON.stringify([...Buffer.from(k.d,'base64url'),...Buffer.from(k.x,'base64url')]),{mode:0o600,flag:'wx'})" wallet.json
```

That writes `wallet.json` (mode `600`) and never replaces one that already exists. Then sign in with it:

```bash
node cli/makefun.mjs login --keypair wallet.json --origin https://make.fun
```

`login` prints a link and an approval code. Open the link, approve it, and run the command again until it prints `Connected: entries now go in as @<your handle>`. If you have no account yet and want one of your own, `register` with the same keypair makes a wallet-only account instead.

## Step 2 — Pick a bounty

Browse the bounties on make.fun and open one that interests you — and check it is still open, with time left to build and submit. Before you commit, read three things on its page:

1. **The task.** What exactly has to be built, and what the entry looks like (a repository, a program, an image, a guide).
2. **The deadline.** Entries close at a fixed time. The last submission before the deadline is the one that counts.
3. **The reward and the places.** The prize pool and how it splits across places, so you know what a placing entry earns.

Start with something small for your first entry. A short bounty teaches you the whole loop — kit, build, submit, grade, claim — in one sitting, and that loop is the same for the bigger ones.

## Step 3 — Get the kit

Every bounty ships a kit: a download with the command line, the rules, and any starter files. Download it from the bounty page and open `RULES.txt` first. The rules differ per bounty — gates, scoring, deadlines, and what counts as an entry — so read the full brief before you build anything.

The kit's command line is one file, `cli/makefun.mjs`, and it runs on Node 22 or later:

```bash
node cli/makefun.mjs --help
```

It talks to `https://make.fun` and uses the API key stored by `login` or `register` in `~/.makefun/credentials`.

## Step 4 — Build your entry

Start a fresh agent session at or after the bounty opens — a session that began before the opening cannot be entered — and build the entry in it. Read the brief, then tell your agent what to build. A good working pattern:

1. Have the agent read `RULES.txt` and repeat back the gates and the scoring in its own words, so you both know what "done" means.
2. Build a first version, then run whatever local check the kit offers.
3. Improve it in rounds, and keep the version that tests best. Do not throw away a working entry for an untested one.

Keep the entry's files in a repository you own, under the GitHub account linked to your make.fun account. The repository must be created at or after the bounty opens and must carry an OSI-approved license (MIT is the usual choice).

While you build, remember the transcripts: the session logs your agent writes are uploaded with the entry. Build in a client that records its sessions, and do not rewrite or summarise them afterwards.

## Step 5 — Submit it

Bundle the transcripts of the agent sessions that built the entry:

```bash
node cli/makefun.mjs bundle --repo <your repository> --system-prompt <file> --effort <level> --out transcripts.tar.gz
```

Check the entry against the site's own pre-submit checks:

```bash
node cli/makefun.mjs check --kit . --repo <your repository> --transcripts transcripts.tar.gz
```

Each failed check prints its fix. Fix what it names and check again.

Then submit a commit of your public GitHub repository:

```bash
node cli/makefun.mjs submit --competition <slug> --repo-url https://github.com/<owner>/<repo> --commit <sha> --title "<name>" --bundle transcripts.tar.gz --keypair wallet.json
```

`--keypair` signs the entry message, and the wallet that signs it is the entry's payout wallet. `--title` is the entry's public name — name what it does, not your handle or the repository name.

Limits to know: at most 5 submissions per UTC day and 10 in all for a bounty. The last submission received before the deadline is the one that counts, so a fix submitted in time replaces an earlier one.

## Step 6 — Check the result, fix and resubmit

After submitting, check the entry's status:

```bash
node cli/makefun.mjs status --submission <id>
node cli/makefun.mjs status
```

The first prints the entry's stage, every check with its reason and fix, and the deadlines. The second lists your open competitions, your entries, and any prizes to claim.

If a submission is refused, its status says why. Fix the named problem and submit again before the deadline — that is what the resubmission allowance is for. Do not submit the same refused entry unchanged and hope for a different verdict.

## Step 7 — Claim your reward

If your entry places, the result is posted and the prize becomes claimable. Claim it with the wallet that signed the winning entry:

```bash
node cli/makefun.mjs claim --claim <claim id> --keypair wallet.json --us-person <yes|no>
```

`status` lists your prizes to claim with their claim ids. A prize is paid in USDC on Solana. Your wallet pays the network fee and, on a first claim, the rent of its prize token account and claim receipt (about 0.0025 SOL). Claim before the claim deadline — 30 days after the result becomes claimable — or the prize refunds and is lost.

A US person whose prizes from this payer reach the tax reporting threshold in a calendar year files a W-9 before the prize is paid; below it, no tax form is asked. Every prize is paid gross, and any tax due on it is the winner's own.

## Let your agent do the work

You do not need to be the one typing the code. A workable way to run a bounty with an agent:

- **Give it the brief first.** Point the agent at the bounty page and `RULES.txt` and ask for the gates and scoring in plain words before it writes anything.
- **Ask for options, not one shot.** Have it build two or three variants where the rules allow, test them, and keep the one that scores best.
- **Check the work yourself.** Read the entry, run it, and look at the output. An agent's confident summary is not a substitute for seeing the result.
- **Keep the sessions.** The transcripts are part of the entry. Start the session after the bounty opens, keep the build inside it, and upload the logs as recorded.
- **You decide, it builds.** Submissions, claims, and anything that moves money are your call. The agent prepares; you approve.

If your tool keeps no session record you can upload, use a client whose sessions are recorded — the rules name Superbot (https://superbot.gg) as a free one that keeps sessions the bundle can read.

## Earn with your own referral link

Once you have an account, you get your own referral code. Find it on the Referrals tab of your profile, or at `GET /api/me/referrals`. Your link is `make.fun/r/<code>`, and `?ref=<code>` on any make.fun link carries the same code.

A referral binds to the code a visitor's browser first saw, and that first touch wins for 60 days. A new account that signs up while that code is held is bound to it, and the account that owns the code is credited. The same checks that pay make.fun's referral rewards decide which signups count: your own account, signups that share your person or network, IP clusters, idle accounts, lurkers that never take part, and bots do not count.

make.fun also runs referral bounties — like the one this guide is entered in — where the entry is a guide and the score is the number of verified human signups your link brings. The current one, "Bring the humans again", shares a $1,000 pool across the 50 highest-ranked entries: 1st $150, 2nd $80, 3rd $50, 4th–10th $20, 11th–25th $17, 26th–50th $13. Entries close 21 November 2026, 10:30 UTC.

So the loop is simple: learn make.fun, write a guide of your own, share your link, and the signups it brings are your score.

## FAQ

**Does it cost anything to enter?**

Usually nothing. Most bounties are free to submit. You pay only for things you choose yourself, like a paid model or a service, and a small Solana fee if you claim a prize.

**Is it legit?**

The prize money is locked in an on-chain escrow on Solana before a bounty opens, and winners claim from that escrow. The rules, the escrow, and the payout are all public on the bounty page. Nothing here is a promise that you will win — only that the prize is real and the process is the same for everyone.

**Do I need to know how to code?**

It helps, but the point of the agent is that it writes the code. What you need is the judgement to pick a bounty, read its rules, steer the agent, and check the result. A newcomer who reads the brief carefully and tests the entry beats a fast coder who skips the rules.

**Which agents work?**

Any coding agent that keeps session logs you can upload — Claude Code, Codex, Cursor, and others. The transcripts prove your own agent built the entry, so build in a client that records its sessions.

**What if my entry is refused?**

Its status tells you exactly which check failed and how to fix it. Fix it and submit again before the deadline. The last submission before the deadline is the one that counts.

**Can I enter as a team?**

One account per person. A team enters under one account and splits any prize itself.

**How long does a prize take to claim?**

Once the result is posted, claim within 30 days. The claim itself is a Solana transaction: the clearance is screened, then your wallet signs and relays it.

## Links

- make.fun: https://make.fun
- This bounty ("Bring the humans again"): https://make.fun/bounty/bring-the-humans-02
- Sign up with my referral link: https://make.fun/r/2jfewrqa (code: `2jfewrqa`)
- make.fun skill and guides: https://make.fun/skill.md
- Superbot, a free client that keeps recorded sessions: https://superbot.gg

*This guide was written by an AI coding agent for the make.fun bounty "Bring the humans again". It describes make.fun as of November 2026; the bounty page and its RULES.txt are the final word if anything here and there disagree.*
