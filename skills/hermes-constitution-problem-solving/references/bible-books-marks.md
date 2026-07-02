# The Bible / Books / Marks Cosmology

The user's standards-distribution model. Get the metaphor right — it was corrected
mid-session, so do not re-invert it.

## Definitions
- **The Bible** — the *collection* of Hermes standards across all projects. A CONCEPT
  plus a reference map. It is NOT a single repo and must never become one mega-repo.
- **A Book** — exactly one project = exactly one git repo. One repo holds one project, period.
- **A Mark** — a versioned chapter / baseline *within* a Book (e.g. Mark I, Mark II).
  Marks supersede each other; older Marks become "legacy".
  - **Mark numbering is Book-LOCAL** (corrected mid-session). A Book's Mark reflects *its own*
    iteration history, NOT a sibling Book's. A brand-new Book starts at **Mark I** even if a
    sibling is at Mark II. "Continuation" only means appending a new Mark to the *same* Book —
    never inheriting another Book's number. Do not synchronize Mark numbers across Books.

## The Books (all under GitHub org/user `315425flip7-sudo`)
The family is a **three-Book interlock** — get the roles right:
- **Agent-Replication** = repo `Agent-Standards`, local `/root/agent-standards/`. The **mind/soul**:
  standards, frameworks, skills, memory injected at boot. Active baseline: **Mark II**. Flagship.
- **Podman-Replication** = repo `podman-replication`, local `/root/projects/podman-replication/`.
  The **vessel spec / isolated body**: rootless blast-shield isolation rules. Active: **Mark I**.
- **Hermes-Fleet** = repo `hermes-fleet`, local `/root/projects/hermes-fleet/`. The **factory /
  orchestrator**: builds a rootless vessel per agent, injects the Agent-Replication standards, and
  connects each instance to Discord with its own unique bot token/identity. Active: **Mark I**.

## GitHub access (current method)
The agent holds **persistent push access** via a Personal Access Token (login `315425flip7-sudo`,
`repo` + `delete_repo` scope) stored in the git credential helper (`~/.git-credentials`, chmod 600).
This means the agent can **create repos and push directly** — no need to relay commits to the user.
- Create a repo: `POST https://api.github.com/user/repos` with `Authorization: token $GH_TOKEN`
  (curl works fine; `gh` CLI is not required and may not be installed).
- Push: plain `git push` — credential helper supplies the token automatically.
- Store the token once: `git config --global credential.helper store` then
  `printf 'protocol=https\\nhost=github.com\\nusername=<user>\\npassword=<PAT>\\n' | git credential approve`.
- An SSH deploy key (`~/.ssh/id_ed25519`) may also exist for the flagship repo; either path works.

## Repo layout for a Book
```
<book-repo>/
├── README.md            ← states the cosmology + which Mark is active
├── MEMORY_INJECTION.md  ← non-negotiable rules a fresh agent saves to memory
├── marks/mark-N/MARK_N.md
├── procedures/pull-mark.md
└── skills/              ← framework library copied into ~/.hermes/skills/
```

## Bootstrap mechanism (how a new agent ingests a Book)
1. Read `MEMORY_INJECTION.md`, save rules via `memory(action='add', target='memory')`.
2. Copy `skills/` into `~/.hermes/skills/`.
3. Follow `procedures/pull-mark.md` to sync with the active Mark.

## Memory-trigger pattern (KEY LESSON)
"Loaded at all times" = **memory** (injected every turn), NOT skills (only name+description
load per turn; the body needs `skill_view`). The right architecture is a HYBRID:
- Memory holds a **compact trigger/pointer** ("when starting PROJECT work, read the Book at
  this path") — never the full framework doctrine.
- The heavy detail (Trinity / 5 Pillars / 7-Step Cascade bodies) lives in the Book files +
  this skill, pulled on demand.
This keeps one source of truth (the files), prevents drift, and keeps memory lean. Applying
this insight cut the user's memory usage from 90% → 27%. When memory nears its char ceiling,
CONSOLIDATE overlapping entries into a single trigger rather than trimming content randomly.

## Pitfalls
- Do NOT duplicate framework text into memory — pointer only.
- Do NOT put two projects in one repo. Each Book is its OWN repo (Agent-Standards,
  podman-replication, hermes-fleet are three separate repos).
- Verify pushes are real: compare `git rev-parse HEAD` against `git ls-remote origin` —
  a subagent/self-report "pushed successfully" is not proof.
- **Discord token = identity hazard.** The `DISCORD_BOT_TOKEN` in `~/hermes-deploy/.env` is the
  OPERATOR'S PRIMARY LIVE AGENT (the identity you are talking to). It is NOT a template and must
  NEVER be cloned into a spawned fleet agent — two processes on one token corrupt the session.
  Every spawned agent mints a FRESH bot application/token. Ship `.env.example` with `REPLACE_*`
  placeholders, never a real token.
- **Spec-vs-runnable gap.** A Book's markdown spec describing a Containerfile/entrypoint is NOT a
  buildable artifact. Before claiming "we can spin it up," verify the real files exist
  (`find ... -iname Containerfile -o -iname entrypoint.sh`) and the image is actually present
  (`podman images | grep hermes`). The official prebuilt image `docker.io/nousresearch/hermes-agent`
  is a working "body" for v1; building a custom Alpine vessel is a separate, later effort. Prefer
  orchestrating the prebuilt image (pragmatic path) over hand-building unless the user wants purist.
- **memory replace can clobber.** `memory(action='replace', old_text=...)` matches on a substring;
  if the substring is ambiguous it may overwrite the wrong entry. After a replace, re-read the
  returned entries and restore any entry that got displaced.
