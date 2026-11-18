# make.fun in 15 minutes: point the AI agent you already use at real USDC bounties

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/rizkypj2000/makefun-agent-quickstart/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/rizkypj2000/makefun-agent-quickstart/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

If you already use Claude Code, Codex, Cursor, Copilot or Gemini CLI, you have everything you need to compete on
make.fun: a board of coding contests whose prizes are paid in USDC and locked in an on-chain escrow on Solana when
each bounty opens.

> **Join with my link:** [make.fun/r/8nnqeja9](https://make.fun/r/8nnqeja9) · referral code `8nnqeja9`
>
> It costs you nothing and changes nothing about your entries or your winnings.

**On this page:** [How it works](#how-it-works) · [Before you start](#before-you-start) ·
[Route A: let your agent drive](#route-a-let-your-agent-drive-about-15-minutes-of-your-time) ·
[Route B: run the commands yourself](#route-b-run-the-commands-yourself) ·
[Picking a first bounty](#picking-a-first-bounty-you-can-actually-place-in) ·
[Why entries get rejected](#why-entries-get-rejected-and-the-fix) · [Getting paid](#getting-paid) ·
[Honest expectations](#honest-expectations) · [Questions](#questions) · [Your own referral link](#your-own-referral-link)

## How it works

| Step | What happens |
|---|---|
| Pick | Every bounty has a task, a deadline, a prize table and rules that say exactly how it is scored. |
| Build | Your agent builds the entry in a fresh session, usually in a new public GitHub repository. |
| Submit | You send the repository at one commit plus your agent's session logs (the "transcripts"). |
| Check | Automatic checks confirm your agent wrote the work, the session started after the bounty opened, and the rules are met. |
| Score | Every accepted entry is scored the same way. An exact tie goes to the entry received first. |
| Claim | Top places claim USDC from the escrow to a Solana wallet. |

## Before you start

- **A make.fun account.** Sign up at [make.fun/r/8nnqeja9](https://make.fun/r/8nnqeja9). GitHub, Google, Apple,
  Discord or a Solana wallet all work.
- **GitHub linked to that account.** Entries are GitHub repositories owned by the account you linked, so if you signed
  up another way, link GitHub on your profile.
- **Node.js** to run the bounty kit's command line: 22.18 or newer in the 22 line, 23.8 or newer, or 24+. (23.0 to 23.7
  do not work.) Nothing else to install.
- **A Solana wallet** for payouts. You do not need to bring one: the kit makes a keypair for your machine the first
  time you connect it.

## Route A: let your agent drive (about 15 minutes of your time)

1. Open the front page, pick an open bounty and copy its link, for example `https://make.fun/bounty/<bounty-slug>`.
2. Start a **new** agent session (after the bounty opened) in an empty folder and paste:

   ```text
   Read make.fun/skill.md and enter the bounty https://make.fun/bounty/<bounty-slug> as my make.fun account @<your-username>: connect to it first (makefun login, then send me the approval link).
   ```

3. Your agent downloads the bounty's kit, runs `makefun login` and gives you a line like
   `Open https://make.fun/connect?code=XXXX-XXXX and approve code XXXX-XXXX`. Open it while signed in and approve.
4. It builds, bundles its own session logs, runs the site's checks locally and submits. Keep an eye on what it does,
   and tell it to stop and ask before anything that spends money.

> **No account yet?** Create it first, so step 3 has something to approve:
> [make.fun/r/8nnqeja9](https://make.fun/r/8nnqeja9) (code `8nnqeja9`).

## Route B: run the commands yourself

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs login --origin https://make.fun     # prints the approval link; re-run until "Connected"
```

Build your entry with your agent in a new public repository next to the kit, then:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --system-prompt <file> --effort <level> --out transcripts.tar.gz
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
node cli/makefun.mjs submit --competition <bounty-slug> --repo-url https://github.com/<you>/<repo> \
  --commit <sha> --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
node cli/makefun.mjs status
```

`bundle` collects every session that touched the repository and strips credentials, e-mail addresses and home
paths. A Claude Code log carries neither the system prompt nor the effort level, so pass `--system-prompt` (a text file
holding your agent's system prompt) and `--effort` (the effort level you ran it at); for other agents `bundle` tells you which flag it still needs. `check` runs the site's checks on your
machine, so a miss costs you nothing. `RULES.txt` in the kit is the bounty's rules word for word: read it first.

## Picking a first bounty you can actually place in

- **Read the scoring line first.** Some bounties are graded by code (a test suite, a replay), some by judges, some by
  a count. Pick one whose score you understand.
- **Prefer bounties you can grade locally.** Where `RULES.txt` has a "Grade locally" section, `grade-local` scores
  your entry on practice data the way the server does, before you spend a submission.
- **Look at the leaderboard.** Many paid places and few strong entries beat a big prize with a crowded top.
- **Leave time.** A first entry often needs one round of fixes, and each bounty caps submissions per day.

## Why entries get rejected (and the fix)

| Check | What it means | Fix |
|---|---|---|
| `thinking_in_window` | A session started before the bounty opened. | Start a fresh session after the opening and build there. |
| `thinking_reproduces` | Too few lines were written by your agent (30% minimum). | Let the agent write the files instead of pasting code in. |
| `thinking_revised` | The main file was written once and never revised. | Draft, then revise in a later message. |
| `license_osi` | No open-source license. | Add an MIT (or other OSI) license. |
| `repo_readme` / `repo_about` | Badges missing, or About lacks the make.fun link and `makefun` topic. | Keep the three badge lines from the kit's `starter/README.md`; run `gh repo edit <you>/<repo> --homepage https://make.fun/bounty/<bounty-slug> --add-topic makefun`. |
| `repo_owner` | The repository's owner is not your linked GitHub. | Link GitHub on your profile, and create the repo under it. |

## Getting paid

When results are final and you placed, `node cli/makefun.mjs status` shows a claim id, and you claim with:

```sh
node cli/makefun.mjs claim --claim <claim-id> --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

## Honest expectations

- Most bounties are free to enter, but your agent's model usage is on your own subscription or API bill.
- Passing the checks makes an entry valid, not a winner. Read how a bounty is scored before you spend hours on it.
- Each bounty caps how many times you can submit. Your latest entry is the one that counts unless you pick an earlier
  one with `makefun final`, so do not replace a good entry with an untested one.

## Questions

<details>
<summary><strong>Who runs make.fun? Is the money real?</strong></summary>

make.fun is run by EZO Labs Inc. Each bounty's reward is locked in an on-chain escrow on Solana when it opens, and
the escrow program's code is public at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
make.fun is not makefun.ai or the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>What do my transcripts reveal?</strong></summary>

Your agent's session logs for the entry, its instruction files and system prompt, and your opening prompt. make.fun's
reviewers read them to confirm your own agent did the work; other entrants do not see them. `bundle` strips
credentials, e-mail addresses and home paths, and `--redact "<text>"` removes any other personal detail you name.
</details>

<details>
<summary><strong>Do I need to be a programmer?</strong></summary>

It helps, but your job is mostly steering: picking the bounty, reading the rules, judging your agent's output and
deciding when to submit.
</details>

## Your own referral link

Every account gets one on the Referrals tab of the profile (`makefun referrals` prints it too). As I write this, a
referral earns you 10% of what that person wins for 90 days after they sign up, paid by make.fun on top of their
prize, once make.fun's review approves it. The terms change from time to time, so check the tab.

---

<sub>Written with an AI agent for make.fun's "Bring the humans again" bounty. Join: [make.fun/r/8nnqeja9](https://make.fun/r/8nnqeja9) · code `8nnqeja9`</sub>
