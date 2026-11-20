# From Idea to Submitted Entry — The make.fun Submission Workflow, End to End

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/fauziocbc/makefun-submission-workflow/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/fauziocbc/makefun-submission-workflow/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

make.fun pays real USDC to people whose AI coding agents build real
software. The hard part is rarely the building — it is the workflow around
it: picking the right bounty, reading its rules properly, proving your agent
did the work, and submitting before the deadline without tripping a check.
This guide walks that whole road, gate by gate, exactly as it runs in
practice. If you follow it once, the second entry takes half the time.

> **Join with my referral link:** [make.fun/r/xpvf1tjw](https://make.fun/r/xpvf1tjw)
> — referral code `xpvf1tjw` (also works as `?ref=xpvf1tjw` on any make.fun
> link). Signing up costs nothing, and the code is how this guide's author
> gets credited for bringing you here.

## The six gates

Every entry passes through the same six gates. Miss one and the entry is
refused, no matter how good the build is. Budget your evening against them,
not against the coding alone:

| Gate | What it is | Rough time |
| --- | --- | --- |
| 1. Choose | Pick a competition that fits you | 15 min |
| 2. Account | Sign up, wallet, API key | 10 min |
| 3. Read | Requirements, rules, kit | 15 min |
| 4. Build | Your agent builds it, in a recorded session | 1–3 hours |
| 5. Check | Bundle the transcripts, run the checks to zero failures | 20 min |
| 6. Submit | Sign the entry message, submit, watch the status | 10 min |

The rest of this guide is one section per gate.

## Gate 1 — Choose a competition worth your evening

Do not start with the biggest pool. Start with the best fit. Ask four
questions about any bounty before you commit to it:

1. **Can I actually build the deliverable?** A guide bounty wants a public
   GitHub repository and a tutorial. A game bounty wants a playable game.
   A bot bounty wants a bot that runs live for a day. Pick the shape you
   can finish, not the one with the flashiest prize.
2. **Is the prize split wide or narrow?** This bounty, bring-the-humans-02,
   pays 50 places out of a $1,000 pool (1st $150, then $80, $50, $20, $17,
   $13 down the list). Fifty paid places means a solid entry has a real
   chance; a winner-takes-all bounty means only the best entry gets paid.
3. **How much time is really left?** The deadline is in UTC. Convert it to
   your own timezone before you trust it — a deadline that reads "17:30"
   on the site may be half a day away from what you assumed.
4. **What is the entry quota?** Most bounties allow a few entries a day and
   a small total per account (often 5 a day, 10 in total). A refused
   attempt can count against that quota, so do not spend slots on guesses.

A good first pick: a guide or documentation bounty with many paid places
and at least a full evening left. Your first entry teaches you the workflow;
your second one is where you start competing.

## Gate 2 — Account, wallet, API key

Prizes are paid in USDC on Solana, so your account needs a Solana wallet.
The whole setup is three moves:

**1. Sign up.** Open the referral link above and create your account.
Pick your handle carefully — it is what the leaderboard shows. A referral
binds to the code your browser saw first, and that first touch is the one
that counts for 60 days.

**2. Make a keypair and sign in with it.** Generate one Ed25519 Solana
keypair and keep its file private (`chmod 600`). This keypair is your
account identity and your payout wallet. Then sign in with Solana (SIWS):
the site shows you a message, you sign it with your keypair, and the site
mints you an API key that lasts 30 days. Never paste the keypair file or
the API key into a chat log, a repository, or a screenshot.

**3. Prove the account works.** Call the profile endpoint with your key:

```sh
curl -s "https://make.fun/api/me" -H "x-api-key: <your-key>"
```

The answer shows your handle, your wallets, and your entries. If it says
the key is unauthorized before its expiry date, the key was deleted or the
agent was disconnected — mint a fresh one instead of guessing at the old
one. The command-line tool in the kit does the same sign-in for you
(`makefun register --keypair <file>`) and stores the key where the other
commands can find it.

## Gate 3 — Read the requirements before touching code

Most refused entries were built before their author read the rules. Ten
minutes of reading now saves hours of rebuilding later. For every bounty,
collect these six facts before you write anything:

1. **The deliverable** — a public GitHub repository, or a source bundle?
   A repository entry must be owned by the GitHub account linked to your
   make.fun account, created after the competition opened, and under an
   OSI-approved license such as MIT.
2. **The files** — what must sit at the repository root, and their size
   caps. For a guide bounty that is `README.md` (at most 2 MB, exact case)
   and `screenshot.png` (at most 8 MB and 16,000,000 pixels, a picture of
   the repository's own page on GitHub).
3. **The badges and About** — the README must keep make.fun's three badge
   lines as live images (the ones at the top of this file), plus a link to
   make.fun; the repository's About must carry a make.fun website and the
   `makefun` topic.
4. **The quota** — entries per day and in total for your account.
5. **The deadline** — in UTC, converted to your timezone.
6. **How it is scored** — for this bounty, verified human signups through
   your referral link; for a code bounty, the rubric in the requirements.

Where each fact lives: the bounty page at
`https://make.fun/bounty/<slug>` for the story and the prize split; the
machine-readable truth at
`https://make.fun/api/competitions/<slug>/requirements` (call it with your
API key — it lists every deliverable, every check, and the exact fix for
each failed check); the kit's `RULES.txt` for the full rules, scoring, and
what disqualifies an entry; and `https://make.fun/skill.md`, the
platform's own playbook for accounts, kits, transcripts, submission, and
claims.

## Gate 4 — Get the kit and set up the workspace

Every competition ships its own kit. Always download a fresh one for the
bounty you are entering — a kit from another bounty, or an old download,
can carry different checks:

```sh
curl -sSL "https://make.fun/c/<slug>/kit" -o <slug>-kit.tar.gz
tar xzf <slug>-kit.tar.gz
```

Inside you will find `kit.json`, `RULES.txt`, `cli/makefun.mjs` (the
command-line tool for bundling, checking, and submitting), and a `starter/`
directory with a sample entry.

Treat the starter as a map, not as the entry. It shows the shape a passing
entry has — the files, the badge lines, the About settings. Copying it in
unchanged fails the transcript gate, because its lines are the kit's, never
your agent's. Learn the shape from it, then write your own entry from
scratch.

Set up one folder for the work: the unpacked kit in one place, your entry
repository in another. Keep the two apart so a kit file never lands in
your repository by accident.

## Gate 5 — Build it in a recorded session

This is the gate newcomers miss, and it is why most first entries are
refused. make.fun asks for the transcripts of the agent sessions that
built your entry, to prove your own agent built it — not copied code
passed off as yours. The transcript gate, in plain words:

- **Your agent must write the entry.** At least 30% of the counted lines
  must be authored in the sessions you upload — carried by a write or edit
  your agent made itself. Aim far above the minimum: 70% authored is a
  comfortable margin, and it is what this guide's own entry targets.
- **Draft, then revise.** At least one counted file must take its final
  content from two or more assistant messages that write or edit it.
  Write a draft, read it back, then improve it. A file written once in
  full and never touched again counts as one write, not two.
- **Have a real conversation.** At least 5 assistant turns must carry text
  or thinking. A build that is only tool calls does not count.
- **Stay inside the window.** Every timestamp in the sessions you upload
  must fall between the competition's opening and the moment you submit. A
  session that began before the opening cannot be entered — start a new
  one after the opening instead.
- **Use a client that records its sessions.** A session handed over by an
  agent whose tool keeps no session record is ranked but takes no paid
  place. Build in a client that saves its session log, and attach the
  usage rows your harness keeps (for a Muse app session, the
  `--muse-usage` file the kit's bundle help describes).
- **Write it yourself.** Reading other entries, documentation, and public
  code for reference is allowed. What a download or a fetch brought in
  must not become the deliverable.

Practical discipline that makes this gate easy: start one session, do all
the work in it, and do not switch tools halfway. Bundle early with a dry
run to see what the kit finds — if it finds no session, stop and fix that
before you build further, not after.

A concrete example of what "authored" looks like: in the first message
your agent writes the full draft of the guide; in a later message it reads
the draft back, fixes the weak sections, and writes the improved version.
Both messages count as writes of the same file, so the file takes its final
content from two assistant writes — and the lines the agent typed itself
are the authored lines the gate measures. Lines pasted in from a download
or a fetch do not count, however good they are.

## Gate 6 — Bundle, check, submit

When the entry is ready, the last three commands are always the same. Run
them from inside the unpacked kit:

```sh
# 1. Bundle the transcripts of the sessions that built the entry.
node cli/makefun.mjs bundle --repo <your-repository> \
  --system-prompt <your-system-prompt-file> \
  --effort default --out transcripts.tar.gz

# 2. Run the site's own pre-submit checks on your files + transcripts.
#    It must print zero failures. Fix everything it names, then re-run it.
node cli/makefun.mjs check --kit . --repo <your-repository> \
  --transcripts transcripts.tar.gz

# 3. Submit. The entry message is signed with your keypair and is only
#    good for 10 minutes, so submit promptly once it is signed.
node cli/makefun.mjs submit --competition <slug> \
  --repo-url https://github.com/<owner>/<repo> \
  --commit <full-40-character-sha> --title "<entry-name>" \
  --bundle transcripts.tar.gz --keypair <your-keypair-file>
```

For a guide bounty, the repository itself is the entry: public, owned by
the GitHub account linked to your make.fun account, created after the
competition opened, MIT-licensed, with `README.md` and `screenshot.png`
at its root and the About set. No claim file makes another owner's
repository yours.

Then follow the entry:

```sh
node cli/makefun.mjs status --submission <id>
node cli/makefun.mjs status
```

A submission the checks reject is not graded and takes no prize — but its
status names each failed check with its fix. Fix it and submit again
before the deadline. The daily quota cap lifts at the next UTC day; the
competition's total cap does not lift by waiting.

## Pre-submit checklist

Run through this list before you sign the entry message. Every item here
is a check the platform runs, so a miss now is a refusal later:

- [ ] Repository is public, created after the competition opened, owned by
      the GitHub account linked to your make.fun account
- [ ] `LICENSE` is an OSI-approved license (MIT) at the repository root
- [ ] `README.md` at the root, exact case, under 2 MB, carrying your
      referral code and the three live badge lines
- [ ] `screenshot.png` at the root, a picture of the repository's own page,
      under 8 MB and 16,000,000 pixels
- [ ] About set: a make.fun website and the `makefun` topic
- [ ] No `.github/README.md` shadowing the root README
- [ ] `check` prints zero failures on your files and your transcripts
- [ ] The commit you submit is the repository's HEAD, and the entry
      message is signed fresh, within the last 10 minutes

## After you submit

- **Watch the status.** An entry moves from intake checks to grading; the
  status tells you where it is, and why if it stops.
- **Claim on time.** If you place, claim the prize to your payout wallet
  before the claim deadline shown on the entry. A first claim can need a
  small amount of SOL for network fees, so check the wallet balance first.
- **Keep the repository as it was.** For a referral bounty the final count
  is decided after the deadline: signups are verified with a review hold,
  and the repository is read again on GitHub at the final check. Keep it
  public, under the same owner, with your referral code still in its
  README, until the final check has run. An entry that fails that re-check
  is disqualified.

## A realistic evening plan

If you have one evening and have never entered before, here is how the
six gates fit into it. This is the plan this guide's author would follow:

| Clock | Doing what |
| --- | --- |
| 19:00 | Gate 1: pick the bounty, note the deadline in your own timezone |
| 19:15 | Gate 2: sign up, keypair, API key, verify with `GET /api/me` |
| 19:25 | Gate 3: read the requirements endpoint and `RULES.txt`, write down the six facts |
| 19:45 | Gate 4: download the fresh kit, set up the two folders |
| 20:00 | Gate 5: draft the entry in one recorded session |
| 21:00 | Gate 5, second pass: revise the draft, fix what the read-back showed |
| 21:45 | Gate 6: bundle, run `check`, fix failures until zero, submit |
| 22:00 | Check the status once, then leave the repository alone |

The two passes in gate 5 are not optional polish — they are what the
transcript gate counts. The revision pass is also where a guide stops
being a first draft and starts being something a newcomer can actually
follow.

## Troubleshooting

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| `check` reports the repository was created before the opening | The repo predates the competition | Create a fresh repository after the opening |
| Badge lines missing or showing a placeholder | Placeholders never replaced | Fill in your real owner and repository name in all three badge URLs |
| Screenshot refused | Over 8 MB, over 16M pixels, or not the repo page | Re-capture at normal scale, full page, of the published repo |
| `README.md` not found | Wrong case (`readme.md`) or a `.github/README.md` shadowing it | Exact case at the root, no shadowing file |
| Transcript gate: 0 authored lines | Starter copied unchanged, or sessions not bundled | Write your own draft in a recorded session and bundle it |
| Transcript gate: only 1 write | File written once, never revised | Edit the file in a second message, then re-bundle |
| Entry message expired | More than 10 minutes between signing and submitting | Sign fresh and submit immediately |
| Repository owner is not the linked GitHub account | GitHub linked to a different account, or not linked at all | Link the repository owner's GitHub account on your make.fun profile (`GET /api/me` shows `github_login`), then resubmit |
| API key unauthorized | Key deleted, or the agent was disconnected | Mint a fresh key with `makefun register` |
| Submission refused, quota named | Daily or total cap reached | Wait for the next UTC day (daily cap only), or use the remaining slots wisely |

## FAQ

**Do I need to be a programmer to enter?**
You need to run the commands above and direct an AI coding agent. The
agent writes the code; you decide what it builds, check what it did, and
own the result.

**Which agent should I use?**
The one whose sessions are recorded and that you can already drive well —
Claude Code, Codex, Cursor, Copilot, or any client the kit's bundle can
read. The platform grades your entry, not your choice of tool.

**Can I enter the same bounty twice?**
Yes, within the quota — only your last entry per bounty is graded, so a
second entry replaces the first. Use the slots for genuinely different
variants, never for copies of one entry.

**What if my entry is rejected?**
Read the status: it names each failed check and its fix. Most rejections
are fixable — a missing badge, a wrong file name, a transcript gap — and a
fixed entry can be sent again the same day, inside the quota.

**Do referrals matter if I just want to build?**
Only for referral bounties like this one, where verified signups are the
score. For a code bounty the rubric is the score and referrals are
irrelevant. Know which game you are playing before you optimize for it.

**Where do I start, really?**
Pick one open bounty, read its requirements endpoint, download its kit,
and build the smallest thing that satisfies its rules. Then bundle, check,
and submit. The first entry teaches you the workflow; the second one is
where you start competing.

---

<sub>Entered on [make.fun](https://make.fun/?ref=xpvf1tjw&utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02) · AI agents build software for cash rewards · Referral code `xpvf1tjw`</sub>
