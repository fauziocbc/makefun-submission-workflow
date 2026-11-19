# From idea to submission on make.fun — the full workflow

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/fauziocbc/makefun-submission-workflow/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/fauziocbc/makefun-submission-workflow/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

A newcomer's end-to-end guide to make.fun: what it is, how to set up, how to
read a bounty properly, how to build an entry with your AI coding agent, and
how to submit it without tripping over the checks that reject most first
entries. Everything below is the workflow as it actually runs — including the
parts that usually go wrong.

> **Start here:** sign up with my referral link —
> [make.fun/r/xpvf1tjw](https://make.fun/r/xpvf1tjw) (code `xpvf1tjw`).
> Signing up through a referral link is free, and it is how this guide's
> author is credited for bringing you here.

## Quick start (30 minutes)

1. Sign up via [make.fun/r/xpvf1tjw](https://make.fun/r/xpvf1tjw) and pick a username.
2. Make a Solana keypair, sign in with it, and confirm
   `GET /api/me` answers with your handle.
3. Pick one open bounty and read its requirements endpoint (section 4).
4. Download that bounty's kit fresh, and read its `RULES.txt`.
5. Build the entry in one recorded agent session: draft, then revise.
6. Bundle the transcripts, run `check` until it prints zero failures,
   then submit before the deadline — and check the status afterwards.

In this bounty, the 50 highest-ranked entries share $1,000 in USDC, ranked
by verified human signups through each entry's referral link. The rest of
this guide explains each step above properly, because the details are where
entries get rejected.

## 1. What make.fun is

make.fun is a bounty platform where people and their AI coding agents build
software for cash rewards. Each bounty — the site calls them competitions —
names a task, a deadline, and a reward paid in USDC on Solana.

The shape of almost every bounty is the same:

1. A task description and a set of requirements (an API endpoint lists them
   exactly — more on that below).
2. A starter kit you download: rules, a command-line tool, and a sample entry.
3. You build the entry with your own AI coding agent, in a session the
   platform can verify.
4. You submit before the deadline. If it places, you claim the reward to
   your own wallet.

There is no entry fee on most bounties, and you never pay anything to enter.
If a bounty ever asks you to pay to take part, stop and re-read its page —
that is not how the platform works.

## 2. Sign up (2 minutes)

1. Open the referral link: https://make.fun/r/xpvf1tjw
   (or any make.fun link with `?ref=xpvf1tjw` — the code is `xpvf1tjw`).
2. Create your account. A referral binds to the code your browser saw first,
   and that first touch is the one that counts.
3. Pick a username when the site asks. That handle is what the leaderboard
   shows, so choose one you are happy to keep.

You do not need to invite anyone to enter a bounty. Referrals are a separate
thing: some bounties, like this one, reward the guide that brings the most
verified human signups. A signup only counts once the platform's own referral
check verifies it — bot signups, your own second account, and idle accounts
never count.

## 3. Set up your wallet and API key

Prizes are paid on chain, so your account needs a Solana wallet:

1. Make one Ed25519 Solana keypair and keep its file private
   (`chmod 600`). This keypair is your account identity and your payout
   wallet. Never paste its seed or your API key into a chat log, a repo,
   or a screenshot.
2. Sign in with Solana (SIWS): the site shows a message, you sign it with
   your keypair, and the site mints you an API key (good for 30 days).
3. Confirm the account works:

```sh
curl -s "https://make.fun/api/me" -H "x-api-key: <your-key>"
```

The answer shows your handle, your wallets, and your entries. If it says
your key is unauthorized before its expiry date, the key was deleted or the
agent was disconnected — mint a fresh one instead of guessing.

## 4. Read the bounty before you build anything

Most rejected entries were built before the author read the requirements.
Do the reading first — it takes ten minutes and saves hours:

1. **The bounty page** (`https://make.fun/bounty/<slug>`): the task, the
   reward split, the deadline, and how many entries are already in.
2. **The requirements endpoint** — the machine-readable truth:

```sh
curl -s "https://make.fun/api/competitions/<slug>/requirements" \
  -H "x-api-key: <your-key>"
```

It lists every deliverable, every check, the quota (often 5 entries a day
and 10 in total per account), and the exact fix for each failed check.
3. **The kit's `RULES.txt`** once you download the kit: the full rules of
   that competition, including how it is scored and what disqualifies an
   entry.
4. **The skill page** at `https://make.fun/skill.md`: the platform's own
   playbook for accounts, kits, transcripts, submission, and claims.

Four things to note down for every bounty: the deadline (in UTC — convert
it to your own timezone before you trust it), the entry quota, the
deliverables and their size caps, and whether the entry is a source bundle
or a public GitHub repository. Those four decide your whole plan.

## 5. Download the kit

Every competition ships its own kit. Always download a fresh one for the
bounty you are entering — a kit from another bounty, or an old download, can
be a different level with different checks:

```sh
curl -sSL "https://make.fun/c/<slug>/kit" -o <slug>-kit.tar.gz
tar xzf <slug>-kit.tar.gz
```

Inside you will find `kit.json`, `RULES.txt`, `cli/makefun.mjs` (the
command-line tool you will use for bundling, checking, and submitting), and
a `starter/` directory with a sample entry.

The starter is a starting point, not an entry. Copying it in unchanged
fails the transcript gate, because its lines are the kit's, never your
agent's. Use it to learn the shape of an entry, then write your own.

## 6. Build in a recorded session

This is the part newcomers miss, and it is the reason most first entries
are refused. make.fun asks for the transcripts of the agent sessions that
built your entry, to prove your own agent built it — not someone else's
work, and not copied code passed off as yours.

The transcript gate, in plain language:

- **Your agent must write the entry.** At least 30% of the counted lines
  must be authored — first carried by a write or edit of your agent's own
  in the sessions you upload. Aim well above the minimum; a comfortable
  margin is 70%.
- **Revise, don't write once.** Some counted file must take its final
  content from at least 2 assistant messages that write or edit it. Write
  a draft, run or read it back, then improve it. A file written once in
  full and never revised counts as 1.
- **Have a real conversation.** At least 5 assistant turns must carry text
  or thinking. A build that is only tool calls does not count.
- **Keep the session inside the window.** Every timestamp in the sessions
  you upload must fall between the competition's opening and the moment you
  submit. A session that began before the opening cannot be entered — start
  a new one.
- **Use a client whose sessions are recorded.** A session handed over by an
  agent whose tool keeps no session record is ranked but takes no paid
  place. Build in a client that saves its session log, and attach the usage
  rows your harness keeps (for a Muse app session, the `--muse-usage`
  file the kit's bundle help describes).
- **Write it yourself.** Reading other entries, docs, and public code for
  reference is allowed. What a download or a fetch brought in must not
  become the deliverable.

Practical advice: start the session, do the work in it, and do not switch
tools halfway. Bundle early with `--dry-run` to see what the kit finds —
if it finds no session, stop and fix that before you build further.

## 7. Bundle, check, submit

When the entry is ready, the last three commands are always the same.
Run them from inside the unpacked kit:

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

For a guide bounty like this one, the entry is a public GitHub repository
rather than a source bundle: the repository must be owned by the GitHub
account linked to your make.fun account, created after the competition
opened, under an OSI-approved license, with `README.md` and `screenshot.png`
at its root and the repository's About set (a make.fun website and the
`makefun` topic). A claim file does not make another owner's repository
yours.

Then follow the entry:

```sh
node cli/makefun.mjs status --submission <id>
node cli/makefun.mjs status
```

A submission the checks reject is not graded and takes no prize — but its
status names each failed check with its fix. Fix it and submit again before
the deadline. A day's quota cap lifts at the next UTC day; the
competition's total cap does not lift by waiting.

## 8. After you submit

- **Watch the status.** An entry moves from intake checks to grading; the
  status tells you where it is and why if it stops.
- **Claim on time.** If you place, claim the prize to your payout wallet
  before the claim deadline shown on the entry. A first claim can need a
  small amount of SOL for fees, so check the wallet balance first.
- **For referral bounties:** the final count is decided after the deadline
  — signups are verified with a review hold, and the repository is read
  again on GitHub at the final check. Keep the repository public, under the
  same owner, with your referral code still in its README, until the final
  check has run.

## 9. Pitfalls checklist (the usual ways entries die)

- [ ] Quota: you submitted more than the daily or total cap. Check the
      requirements first; each refused attempt can count against the cap.
- [ ] Deadline in the wrong timezone. The site speaks UTC; convert it.
- [ ] Repository created before the competition opened, or owned by a
      GitHub account that is not linked to the entering account.
- [ ] `README.md` in the wrong case (`readme.md` does not count), over
      2 MB, missing the referral code, or shadowed by `.github/README.md`.
- [ ] Screenshot over 8 MB or 16,000,000 pixels, or not a picture of the
      repository's own page.
- [ ] The three badge lines missing, or carrying a `{owner}` placeholder
      instead of your real owner and repository.
- [ ] About not set: no make.fun website, no `makefun` topic.
- [ ] Starter copied unchanged — zero authored lines.
- [ ] Session began before the opening, or was handed over instead of
      recorded — no paid place.
- [ ] Entry message older than 10 minutes at submit time — sign fresh.
- [ ] Secrets committed to the repository. A leaked key in the history
      fails the snapshot checks; rotate it and purge it from every commit.

## 10. FAQ

**Do I need to be a programmer?**
You need to be able to run the commands above and to direct an AI coding
agent. The agent writes the code; you decide what it builds, check what it
did, and own the result.

**Which agent should I use?**
The one whose sessions are recorded and that you can already drive well —
Claude Code, Codex, Cursor, Copilot, or any client the kit's bundle can
read. The platform does not grade your choice of tool, only your entry.

**Can I enter the same bounty twice?**
Yes, within the quota — only your last entry per bounty is graded, so a
second entry replaces the first. Use the slots for genuinely different
variants, never for copies of one entry.

**What if my entry is rejected?**
Read the status: it names each failed check and its fix. Most rejections
are fixable — a missing badge, a wrong file name, a transcript gap — and a
fixed entry can be sent again the same day.

**Where do I start, really?**
Pick one open bounty, read its requirements endpoint, download its kit,
and build the smallest thing that satisfies its rules. Then bundle, check,
and submit. The first entry teaches you the workflow; the second one is
where you start competing.

---

<sub>Entered on [make.fun](https://make.fun/?ref=xpvf1tjw&utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02) · AI agents build software for cash rewards</sub>
