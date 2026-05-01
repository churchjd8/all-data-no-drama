# Cynthia AI — Hub & Spoke Server Architecture

**For:** Joshua & Ricki
**Date:** May 1, 2026

---

## The Big Picture

Every client gets their own isolated server. One central hub holds everything that makes us *us* — our templates, processes, voice profiles, env configs, and shared learnings. Each client server pulls from that hub automatically, so every engagement feels like one continuous operation, even though the data is separated.

Think of it like a franchise model: the playbook lives at HQ, but each location runs independently.

---

## Why Separate Servers Per Client

**Security & trust.** Client A's data never touches Client B's environment. No accidental leaks, no shared credentials, no crossed wires. If a client ever asks "who has access to our data?" the answer is clean: only their server.

**Simplicity.** Each server is a self-contained workspace. No complex folder permissions, no "make sure you're in the right directory." You open the client's server, everything in front of you is theirs.

**Scalability.** Adding a new client = spinning up a new server. Removing a client = shutting one down. No untangling dependencies. No "wait, which config belongs to who?"

**Parallel work.** Joshua can be working on Jasons server while Ricki is building automations on CrossCourt's. No merge conflicts, no stepping on each other's toes.

**Audit trail.** Each server has its own git history. Every change is tracked per-client. Clean records if anyone ever needs to look back.

---

## The Hub

The hub is a single git repo — our agency's brain. It holds everything shared across clients.

```
cynthia-agency-hub/
├── CLAUDE.md                          # agency-wide AI instructions
├── .env.shared                        # shared API keys (Anthropic, Cynthia, etc.)
├── cynthia-tools/                     # our proprietary tool suite
│   ├── cynthia-meet/                  # meeting intelligence
│   ├── cynthia-crm/                   # client relationship management
│   ├── cynthia-reach/                 # outreach automation
│   └── ...                            # future Cynthia products
├── memory/
│   ├── MEMORY.md                      # index
│   ├── voice_profile_joshua.md        # Joshua's writing voice
│   ├── agency_processes.md            # how we run engagements
│   ├── feedback_global.md             # learnings that apply to all clients
│   └── agency_identity.md             # who we are, how we position
├── templates/
│   ├── onboarding_checklist.md        # standard client onboarding steps
│   ├── proposal_template.md           # base proposal structure
│   ├── discovery_questions.md         # intake questions for new clients
│   ├── weekly_update_template.md      # client communication template
│   └── project_kickoff.md             # first-session framework
├── scripts/
│   ├── spin-up-client.sh              # one command to create a new client server
│   ├── sync-hub.sh                    # pulls latest hub into a client server
│   └── install-tools.sh              # installs Claude Code, OpenCrawl, etc.
└── playbooks/
    ├── ai_ops_audit.md                # how to run an AI ops assessment
    ├── website_build.md               # website project playbook
    ├── automation_build.md            # automation project playbook
    └── content_system.md              # content pipeline playbook
```

**What lives here:**
- Cynthia tools (Meet, CRM, Reach, and any future products)
- Agency identity & voice profiles
- Process templates & playbooks
- Shared environment variables
- Global learnings & feedback
- Onboarding & setup scripts

**What NEVER lives here:**
- Client-specific data
- Client credentials or API keys
- Client deliverables or content
- Anything that would be a problem if another client saw it

---

## The Spokes (Client Servers)

Each client server is its own workspace with a standard structure:

```
client-[name]/
├── CLAUDE.md                          # client-specific AI instructions
├── .env                               # client-specific API keys & credentials
├── hub/                               # ← git submodule pointing to the hub
│   └── (entire hub repo, auto-synced)
├── memory/
│   ├── MEMORY.md
│   ├── client_voice_profile.md        # their brand voice
│   ├── client_context.md              # org structure, goals, stakeholders
│   └── project_notes.md              # ongoing project context
├── deliverables/
│   └── (whatever we're building for them)
└── assets/
    └── (their brand assets, docs, etc.)
```

Each client's `CLAUDE.md` includes a line like:

```markdown
# Shared Agency Knowledge
Read from ./hub/ for agency-wide processes, templates, and voice profiles.
Apply the agency playbook, then layer in this client's specific context.
```

This means Claude Code on any client server automatically knows our full agency playbook AND the client's specific world.

---

## New Client Onboarding: What Happens

When we sign a new client, one command kicks off the entire setup:

```bash
./spin-up-client.sh [client-name]
```

**What that script does:**

1. **Spins up the server** — provisions a new cloud dev environment on Hetzner

2. **Installs the toolchain:**
   - Claude Code (our AI operating layer)
   - OpenCrawl (for scraping/auditing the client's existing web presence)
   - Any other standard tools we use

3. **Clones the hub as a submodule** — pulls in all agency templates, playbooks, voice profiles, shared .env variables. The client server immediately has our full operational context.

4. **Scaffolds the client workspace** — creates the standard folder structure (`memory/`, `deliverables/`, `assets/`), drops in a starter `CLAUDE.md` with the client name, and creates placeholder memory files.

5. **Copies shared .env values** — API keys for our tools (Anthropic, Cynthia platform, etc.) get pulled from the hub so we're not manually configuring every time.

**After the script runs:**
- Either of us can open that server from any computer and start working immediately
- Claude Code already knows our agency playbook via the hub
- We fill in the client-specific context (voice profile, goals, stakeholders) during the discovery/kickoff phase
- The client is fully operational within minutes, not hours

---

## How It Stays in Sync

### Hub → Spoke (automatic)
Each time we open a client server or run `sync-hub.sh`, the latest hub gets pulled in. New templates, updated processes, fresh learnings — they flow to every client automatically.

### Spoke → Hub (intentional)
When we learn something on a client project that applies universally, we deliberately push it to the hub:

- "This proposal structure converts better" → update `templates/proposal_template.md` in the hub
- "Always ask about existing tech stack in discovery" → update `templates/discovery_questions.md`
- "Claude works better when we give it X context" → update `memory/feedback_global.md`

This is intentional, not automatic. We don't want client-specific details leaking into the shared brain. We curate what goes upstream.

### The Feedback Loop
```
Client work generates learnings
    → We identify what's universal vs. client-specific
    → Universal learnings get pushed to the hub
    → Hub syncs to all client servers
    → Every future engagement benefits
    → Repeat
```

This is how the agency gets smarter with every client. The hub is a living, evolving playbook — not a static template library.

---

## Both of Us, Anywhere

Because each client server is a cloud environment with git-backed state:

- **Joshua** can work from his laptop in San Diego
- **Ricki** can work from his setup wherever he is
- We can even work on the same client simultaneously if needed
- All changes are tracked via git — full history of who did what and when
- Picking up where the other left off is seamless — Claude Code's memory persists on the server, not on our local machines

No "can you send me that file?" No "which version is current?" The server IS the source of truth.

---

## Visual Summary

```
                    ┌─────────────────────┐
                    │                     │
                    │   CYNTHIA AGENCY    │
                    │       HUB           │
                    │                     │
                    │  templates/         │
                    │  playbooks/         │
                    │  voice profiles/    │
                    │  shared .env        │
                    │  global learnings   │
                    │  setup scripts      │
                    │                     │
                    └────────┬────────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              ▼              ▼
     ┌────────────┐  ┌────────────┐  ┌────────────┐
     │  CLIENT A  │  │  CLIENT B  │  │  CLIENT C  │
     │  Server    │  │  Server    │  │  Server    │
     │            │  │            │  │            │
     │  hub/ ←────│──│── synced ──│──│── from hub │
     │  memory/   │  │  memory/   │  │  memory/   │
     │  delivers/ │  │  delivers/ │  │  delivers/ │
     │  .env      │  │  .env      │  │  .env      │
     └────────────┘  └────────────┘  └────────────┘
           ↕              ↕              ↕
      Joshua OR       Joshua OR      Joshua OR
       Ricki           Ricki          Ricki
     (any computer)  (any computer) (any computer)
```

---

## TL;DR

- **One hub** = our agency brain (templates, processes, learnings, shared config)
- **One server per client** = isolated, secure, simple
- **Hub syncs to all spokes** = every client gets our latest and greatest
- **Universal learnings flow back to hub** = the agency gets smarter with every engagement
- **One script to onboard** = new client goes from signed to operational in minutes
- **Either of us, any computer** = cloud servers mean location doesn't matter

The architecture grows with us. Client #3 benefits from everything we learned on clients #1 and #2. Client #10 benefits from everything before it. The playbook compounds.
