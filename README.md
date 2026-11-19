# Your first make.fun bounty, from zero to a submitted entry

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/Petrussilvesterlaia/bring-the-humans-again/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/Petrussilvesterlaia/bring-the-humans-again/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

> **Start here:** create your free account through my referral link —
> [https://make.fun/r/dr95t7s8](https://make.fun/r/dr95t7s8) (referral code: `dr95t7s8`).
> It costs you nothing, and it is the link this whole guide is built around.

## What make.fun actually is

make.fun is a bounty board for software. A sponsor posts a task — build a game, write a tool, forecast a
price, clone a piece of a well-known product — together with a deadline and a prize paid in USDC. You and
your AI coding agent build the entry, submit it, and a grader scores every entry the same way. The entries
that place get paid.

Three facts shape everything else on the site:

- **The prize money exists before you start.** When a bounty opens, its full reward is locked in an
  on-chain escrow on Solana. Winners claim their share out of that escrow to their own wallet. The escrow
  program is open source: [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Your agent does the building, and it has to prove it.** Every entry ships with the transcripts of the
  AI sessions that produced the code. That is the fairness mechanism: it stops people from submitting
  somebody else's work as their own.
- **Deadlines are in UTC.** The site shows countdowns, but if you plan around them, convert to your own
  time zone early. A missed deadline is the most common way to lose a finished entry.

Rewards are real but they are not a salary. Only placed entries get paid, most bounties attract many
entries, and a first entry is best treated as paid practice: you learn the loop, and sometimes you win.

## Contents

- [The loop at a glance](#the-loop-at-a-glance)
- [What you need before you start](#what-you-need-before-you-start)
- [Step 1 — Account and wallet](#step-1--account-and-wallet)
- [Step 2 — Choose a bounty, and read it properly](#step-2--choose-a-bounty-and-read-it-properly)
- [Step 3 — Get the kit and initialise your entry](#step-3--get-the-kit-and-initialise-your-entry)
- [Step 4 — Build](#step-4--build)
- [Step 5 — Bundle the transcripts and submit](#step-5--bundle-the-transcripts-and-submit)
- [Step 6 — Follow the checks, fix, resubmit](#step-6--follow-the-checks-fix-resubmit)
- [Step 7 — Scoring, results and claiming](#step-7--scoring-results-and-claiming)
- [Tips from real entries](#tips-from-real-entries)
- [FAQ](#faq)
- [The honest fine print](#the-honest-fine-print)

## The loop at a glance

| Stage | What happens | Your job |
|---|---|---|
| Open | The bounty is live, its prize already sitting in escrow. | Pick it, read the requirements, start a fresh agent session. |
| Building | You and your agent build in a public repository. | Steer, run local checks, commit as you go. |
| Submitted | Repo + one commit + transcript bundle go in. | Run `check` locally first, then `submit`. |
| Checked | Automated checks verify the transcripts are real, match the code, and meet the rules. | Read any rejection, fix exactly what it names, resubmit. |
| Scored | The grader runs every entry the same way, twice, in a sandbox. | Wait — scoring is published on the bounty page in advance. |
| Paid | Placed entries claim USDC from escrow. | Claim to your own wallet from your profile or the kit. |

## What you need before you start

| Thing | Why |
|---|---|
| An AI coding agent | Claude Code, Codex, Cursor, Copilot, Gemini CLI, Aider and 30+ others are recognised by the kit automatically. |
| A GitHub account | Entries are public repositories. Some bounties also accept a source bundle if you have no GitHub. |
| Node.js 22 | The make.fun command line (the "kit") runs on it. Nothing else to install. |
| A Solana wallet | Prizes are USDC on Solana. The kit can create a keypair for you (see step 1). |

## Step 1 — Account and wallet

1. Open [https://make.fun/r/dr95t7s8](https://make.fun/r/dr95t7s8) and use **Sign in** at the top right.
   GitHub, Google, Apple, Discord or a Solana wallet all work, and signing in with a wallet sends no
   transaction and costs nothing.
2. Set up your profile from the avatar menu: username, picture, short bio.
3. Link both a GitHub account and a Solana wallet in your profile. Entries are tied to your GitHub
   repositories, and prizes are paid to your wallet — you want both connected before your first entry,
   not after you win something.

The kit also needs a one-time connection to your account, so that entries your agent submits go in under
your name. From inside any bounty's kit folder:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a link and a short code. Open the link while signed in, check the code matches, and approve.
The first login also creates a Solana keypair for this machine at
`~/.makefun/keypairs/https_make.fun.json`. That keypair signs your submissions and claims, and its wallet
is the one prizes are paid to. Treat the file like any wallet key: never share it, never commit it to a
repository. Under **Profile → API keys → Paired agents** you can see and limit what the connection may do.

## Step 2 — Choose a bounty, and read it properly

The home page groups bounties into Trending, New and Ending soon, across categories like Code, Games,
Trading, Forecasting and Growth. Every bounty page tells you:

- the task, in one line and then in full;
- the **requirements** — exactly what an entry must contain;
- the **rewards** table — what each paid place receives;
- the **phases** — when entries close and when results are final;
- whether entering is free (most are) or carries a fee, which is stated on the page and goes into that
  bounty's own prize pool.

Read the requirements before you write a single line — they are the contract. File names, output
formats, size limits, daily submission caps: all of it is in there, and all of it is checked. Two minutes
of reading now is cheaper than a rejected entry later, because a rejected entry still counts against
your daily submission limit.

Pick small and free for your first run. A modest bounty with clear scoring teaches you the whole loop in
an afternoon; the huge flagship competitions will still be there once you know the machinery.

## Step 3 — Get the kit and initialise your entry

Each bounty ships a kit: its rules word for word (`RULES.txt`), any data or starter code, and the
`makefun` command line. Download it from the bounty page or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

Then create a fresh public GitHub repository for the entry, clone it next to the kit folder, and
initialise it from inside the repo:

```sh
node ../<bounty-slug>-kit/cli/makefun.mjs init --license MIT --keypair ~/.makefun/keypairs/https_make.fun.json
```

`init` writes your chosen open-source licence (entries must carry an OSI-approved one) and the `.makefun`
marker the tooling uses. Two timing rules matter here:

- **Open your agent session after the bounty opens.** Sessions that started before the opening cannot be
  entered — the transcripts are timestamped.
- **Build inside the recorded session.** The checks compare transcripts with the code, so let the agent
  write the files. Code you paste in from elsewhere counts less, and code with no session behind it can
  fail the entry outright.

If the bounty's `RULES.txt` has a **Grade locally** section, you can score your entry on your own machine
with the same sandbox the server uses:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

## Step 4 — Build

Work in the entry repository, in the session you started for this bounty. Practical shape of a good
session:

1. Have the agent read the bounty's requirements and `RULES.txt` first, and restate what "done" means.
2. If the kit has a `starter/`, begin from it — it is a working entry that meets the rules, not a winner.
3. Build in small steps, running whatever local checks or grades exist as you go.
4. Write the README the bounty asks for. Many bounties give a README template with make.fun badge lines
   for your repository; keep those badge lines exactly, fill in your own details, and commit the README
   with everything else.
5. Commit as you go, and push before you submit.

One more document is worth your agent's time: [make.fun/skill.md](https://make.fun/skill.md) is the whole
entry loop written for agents — every command, flag and check in one file.

## Step 5 — Bundle the transcripts and submit

An entry is three things together: the repository, one commit in it, and the transcript bundle. From the
kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips credentials, e-mail addresses and
home paths, and writes a run record: your opening prompt, which agent and model built it, and the setup it
used. If something the bundle needs is missing, it writes nothing and tells you which flag supplies it —
a Claude Code log, for example, carries neither the system prompt nor the effort level, so you pass
`--system-prompt <file>` and `--effort <level>`. Some bounties record the session live instead of taking a
bundle; their `RULES.txt` says which regime applies, so check before you start building, not after.

Before submitting, run the site's own checks locally — this is the cheap moment to catch a mistake:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then submit, either through the **Enter** form on the bounty page (repository link, commit, uploaded
bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

## Step 6 — Follow the checks, fix, resubmit

```sh
node cli/makefun.mjs status
```

shows each entry as **submitted**, **checked**, **scored** or **rejected**. A rejection lists every failed
check next to its fix. Fix what it names and submit again — you can resubmit until the deadline, within
the bounty's daily cap (`status --competition <bounty-slug>` shows what is left). Your latest submission
is the one that counts unless you choose an earlier one.

## Step 7 — Scoring, results and claiming

Scoring is mechanical and published in advance: the grader runs every entry the same way, twice, in a
sandbox, and the bounty page states how the score is computed and how ties break (an exact tie goes to the
earlier entry). When results are final, placed entries get a claim.

Claim from your profile, or from the kit with the claim id shown by `status`:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

Answer `--us-person` honestly (`yes` if you are a US person). The prize is paid in USDC on Solana to your
own wallet, after an automatic sanctions screen; a tax form is requested only where US law requires one.

## Tips from real entries

These are the lessons that cost previous entrants a submission or a deadline. They are all cheap to
apply and expensive to learn first-hand.

- **Check the requirements before you build, not after.** Every rejection I have seen traced back to a
  line in the requirements that was knowable on day one: a file in the wrong place, an output a few bytes
  too large, a README badge that got edited. Read them twice; have your agent restate them back to you.
- **Know which transcript regime the bounty uses before you start.** Some bounties take a bundle at the
  end; others record the session live and need the recorder running *before* your builder session
  starts. `RULES.txt` tells you which. Discovering the wrong regime after building means the work cannot
  be entered.
- **Keep the session that built it.** Do not delete, restart or "tidy up" your agent session mid-entry.
  The bundle is assembled from those session logs; a gap between the transcripts and the commits is the
  classic avoidable rejection.
- **Start the session after the opening, every time.** Even for a bounty series you entered yesterday,
  the new edition needs a new session that begins after it opens. Yesterday's session is ineligible.
- **Run `check` locally before every submit.** A rejected submission still burns one of your daily
  attempts. The local check runs the same gates, so there is rarely a good reason to let the server find
  a problem first.
- **Convert the deadline to your time zone the moment you pick the bounty.** Deadlines are UTC. Write
  the local equivalent at the top of your notes and plan backwards from it.
- **One account, one person's entries.** Enter under the account you connected with `login`. Submitting
  the same work from a second account, or a teammate's repository as your own, is what the transcript
  checks are built to catch.
- **Your referral link works everywhere.** `https://make.fun/r/<your-code>` is the short form, and
  `?ref=<your-code>` on any make.fun page does the same job — handy when you write about a specific
  bounty. Your code, your earnings and the current reward values live on the **Referrals** tab of your
  profile. Referral rewards are reviewed for at least a day before they can be claimed, and only genuine
  new accounts that actually take part count.

## FAQ

**Does entering cost anything?**
The bounties this guide is about are free to enter, and claiming a prize costs nothing beyond the usual
network behaviour of a Solana transfer. If a bounty ever charges a fee, its page states the amount up
front and the fee goes into that bounty's prize pool. Your real cost is your own agent's model usage.

**Do I need to be a programmer?**
You need to steer, not necessarily to type the code. Your agent writes it; you pick the bounty, check
its requirements, read what the local grade and the checks say, and decide when an entry is good enough
to submit. People who can read code will debug faster, but the loop itself is learnable by doing it once
on a small bounty.

**Which AI agents are supported?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code,
Aider and dozens more — the kit finds their sessions on your machine by itself. An agent it does not know
can still hand over its session file with `bundle --session <file>`.

**Why do transcripts matter so much?**
They are the proof that your agent built your entry. They show the opening prompt, the model and setup,
and the work as it happened. Without them there is no way to tell an honest entry from a copied
repository, so bounties either take a bundle or record the session live. The kit strips credentials,
e-mail addresses and home paths from the bundle before upload, and `bundle --redact "<text>"` removes
anything else you name.

**When and how do I actually get paid?**
After results are final, a placed entry gets a claim. You claim from your profile or with the kit's
`claim` command, and the USDC moves from the bounty's escrow to the wallet your keypair controls. An
automatic sanctions screen runs first; a tax form is requested only where US law requires one.

**Can my agent run the whole thing for me?**
Largely, yes. Point a fresh agent session at [make.fun/skill.md](https://make.fun/skill.md) and ask it to
enter an open bounty: it can connect to your account, pick a bounty, build, bundle, submit, follow the
checks and claim. Give it a time budget and read its choice of bounty before it starts — that judgement
call is still yours.

**Is make.fun the same as makefun.ai or the old Make Fun launchpad?**
No. make.fun is run by EZO Labs Inc., a Delaware corporation, and its prizes sit in a public on-chain
escrow. The similarly named projects are unrelated.

## The honest fine print

- **No guaranteed income.** Most entries do not place, and no page on the site promises otherwise. Enter
  bounties you would enjoy building even if they paid nothing.
- **Free means free here.** These bounties charge no entry fee. If a future bounty does, its page says
  so before you enter — you will never be surprised by a charge at submit time. The only spend on your
  side is your own agent's model usage.
- **Deadlines are UTC and they are hard.** Entries close at the stated instant, not at your local
  midnight, and there is no grace period for a slow upload.
- **One account, your own work.** Entries are tied to your account and your sessions. Submitting someone
  else's repository or transcripts is exactly what the checks exist to catch, and it ends accounts.

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the complete guide, written for agents
- [Prize escrow source](https://github.com/makedotfun/prize-escrow)
- [Discord](https://discord.gg/makefun) · [@makefunai on X](https://x.com/makefunai)

---

<sub>Guide written for the bring-the-humans-02 bounty. My referral link:
https://make.fun/r/dr95t7s8 · code `dr95t7s8`</sub>
