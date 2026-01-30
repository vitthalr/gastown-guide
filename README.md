# Gastown: Your AI Software Company (Full Guide)

**You are the CEO of a software development company where all the employees are AI agents.**

You don't write code yourself (unless you want to). You give direction, make decisions, and review results. Your AI team handles the rest.

> **TL;DR:** Gastown lets you run 20-30 AI coding agents in parallel on a single project, with automatic crash recovery, merge handling, and full audit trails. You give high-level direction; AI handles the rest.

---

## Part 1: The Employees

```
┌─────────────────────────────────────────────────┐
│                    YOU (CEO)                    │
└─────────────────────┬───────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────┐
│                    MAYOR                        │
│            (orchestrates everything)            │
└───────┬─────────────┬─────────────┬─────────────┘
        │             │             │
        ▼             ▼             ▼
   ┌─────────┐  ┌──────────┐  ┌──────────┐
   │ WITNESS │  │ POLECATS │  │ REFINERY │
   │(monitor)│  │  (work)  │  │ (merge)  │
   └─────────┘  └──────────┘  └──────────┘

┌─────────────────────────────────────────────────┐
│                   DEACON                        │
│    (independent watchdog - monitors everyone)   │
└─────────────────────────────────────────────────┘
```

### You (The CEO)

You're the boss. You decide what gets built.

**Your job:**
- Tell the Mayor what you want ("Build me a login system")
- Make high-level decisions when asked
- Review final work before shipping
- Step in only when something needs human judgment

**You don't need to:**
- Break down tasks yourself
- Assign work to developers
- Monitor if workers are stuck
- Merge code manually

That's what you hired employees for.

---

### Mayor (Project Manager)

The Mayor is your **right hand**. This is the only employee you talk to directly.

**What Mayor does:**
- Listens to your requests
- Breaks big ideas into small tasks (Beads)
- Groups related tasks into work bundles (Convoys)
- Decides which developer works on what
- Tracks progress across all work
- Reports back to you when things are done (or stuck)

**Example conversation:**

```
You: "I need user authentication with login, signup, and password reset"

Mayor: "I'll create a convoy for this. Breaking it down:
  - Bead 1: Create user database schema
  - Bead 2: Build signup API endpoint
  - Bead 3: Build login API endpoint
  - Bead 4: Build password reset flow
  - Bead 5: Create frontend login form
  - Bead 6: Create frontend signup form
  - Bead 7: Add session management

  I'll assign these to workers now. Some can run in parallel."
```

**How to talk to Mayor:**
```bash
gt mayor attach
```
This opens a chat session. Just type what you want in plain English.

---

### Polecats (Contract Developers)

Polecats are your **disposable workforce**. They get hired, do one job, and leave.

**What Polecats do:**
- Pick up a single task (Bead)
- Work on it in their own workspace (git worktree)
- Submit their code when done
- Disappear

**Why "disposable"?**
- They don't need to remember anything long-term
- If one crashes, you just spawn another
- You can run 5, 10, 20 of them in parallel
- Each works in isolation so they don't conflict

**Example:**
```
Polecat "toast" spawns
    ↓
Picks up Bead: "Create user database schema"
    ↓
Works in ~/gt/myproject/polecats/toast/
    ↓
Writes the migration file, tests it
    ↓
Submits PR
    ↓
Polecat "toast" is done, workspace can be cleaned up
```

**Think of them like:** Uber for developers. They show up, do the ride, leave.

---

### Witness (Team Lead)

The Witness **babysits the Polecats**.

**What Witness does:**
- Monitors all active Polecats in a project
- Detects if a Polecat is stuck or crashed
- Detects if a Polecat finished but didn't report
- Respawns crashed sessions
- Nudges stalled polecats back to work
- Reports problems to Mayor

**Why this matters:**
AI agents sometimes freeze, crash, or go in circles. Without Witness, you'd have to manually check "is Polecat #3 still working?" The Witness does this automatically.

**Example:**
```
Witness sees:
  - Polecat "toast": Active, working on Bead #1
  - Polecat "crispy": Finished 5 mins ago, submitted PR
  - Polecat "waffle": No activity for 10 mins...
    → Witness: "waffle might be stuck, flagging for Mayor"
```

---

### Refinery (QA / Merge Manager)

The Refinery is your **code integration specialist**.

**What Refinery does:**
- Receives merge requests from all Polecats
- Handles merge conflicts
- Merges code into the main branch
- Resolves conflicts when two Polecats touched the same file
- Ensures the build still works after merging

**Why this matters:**
When 5 Polecats work in parallel, their code might conflict. The Refinery handles the messy job of combining everyone's work.

**Example:**
```
Refinery receives:
  - PR from "toast": database schema ✓
  - PR from "crispy": signup API (needs schema)
  - PR from "waffle": login API (needs schema)

Refinery merges in order:
  1. Merge toast's schema first
  2. Rebase crispy's PR, merge
  3. Rebase waffle's PR, merge
  4. Run tests
  5. All green → done
```

---

### Deacon (Night Watchman)

The Deacon is a **background daemon** that runs 24/7.

**What Deacon does:**
- Monitors the entire Town (all projects, all workers)
- Detects if Mayor crashed
- Detects if Witness crashed
- Detects orphaned work (tasks assigned but no one's working)
- Detects "mass death" (everything crashed)
- Keeps the system healthy

**Why this matters:**
Things crash. Terminals close. Laptops sleep. The Deacon notices and helps recover.

**You don't interact with Deacon directly.** It just runs in the background keeping things alive.

> **Note:** Deacon has helpers called **Dogs** (like "Boot") that handle specific maintenance tasks. You'll see them in logs but don't need to manage them.

---

### Crew (Your Personal Workspace)

Crew is where **persistent workers (human or AI) work**.

**What Crew is:**
- A persistent workspace for humans or long-lived AI agents
- Your sandbox to review code
- Where you make manual changes if needed
- Survives restarts (unlike Polecat workspaces)

**Example:**
```
~/gt/myproject/crew/vitthal/    ← Your workspace
~/gt/myproject/crew/sarah/      ← Teammate's workspace
```

**Why this matters:**
- Polecats are ephemeral (temporary)
- Crew is permanent
- You review the merged code here before shipping

---

## Part 2: The Office Building

### Town (`~/gt/`)

The **Town** is your company headquarters. Everything lives here.

```
~/gt/
├── mayor/              ← Mayor's office
├── deacon/             ← Night watchman's station
├── .beads/             ← Central ticket database
├── myproject/          ← Project #1 (a "Rig")
├── another-project/    ← Project #2 (another "Rig")
└── ...
```

You have **one Town**. It contains all your projects.

---

### Rig (A Project)

A **Rig** is a single project/repository you're working on.

```
~/gt/myproject/                    ← The Rig
├── config.json                    ← Rig settings
├── .beads/                        ← Project-specific tickets
├── mayor/rig/                     ← Canonical source code
├── crew/vitthal/                  ← Your workspace
├── polecats/                      ← Worker workspaces
│   ├── toast/
│   ├── crispy/
│   └── waffle/
└── refinery/                      ← Merge processing area
```

**Why "Rig"?**
Gastown uses an oil town metaphor. A Rig is like an oil rig - a site where work happens.

---

### Beads (Tickets/Tasks)

A **Bead** is a single unit of work. Like a Jira ticket.

**Bead contains:**
- ID (e.g., `gt-abc12`)
- Title ("Add login endpoint")
- Description (details, acceptance criteria)
- Status (open, in-progress, done)
- Assigned to (which Polecat)
- History (who did what, when)

**Example:**
```
Bead gt-abc12
├── Title: Create user signup API
├── Status: in-progress
├── Assigned: polecat/toast
├── Created: 2025-01-15
└── Description:
    "POST /api/signup endpoint that creates user,
     hashes password, returns JWT token"
```

**Why "Bead"?**
Beads are like beads on a string - small, trackable units that connect to form larger work.

---

### Convoy (Work Bundle)

A **Convoy** is a group of related Beads. Like a feature branch or sprint.

**Example:**
```
Convoy: "User Authentication Feature"
├── Bead gt-001: Database schema
├── Bead gt-002: Signup API
├── Bead gt-003: Login API
├── Bead gt-004: Password reset
├── Bead gt-005: Frontend forms
└── Status: 3/5 complete
```

**Why Convoy?**
- Tracks progress on a feature as a whole
- Knows when ALL beads are done
- Can notify you "Authentication feature is complete"

---

### Hooks (Work Inbox)

A **Hook** is a special pinned Bead (work assignment) for each agent. It's the agent's primary work queue. When work appears on your hook, you execute it immediately (GUPP).

**How it works:**
- Mayor slings a bead → it appears on a Polecat's hook
- Polecat sees the hook → immediately starts working (propulsion)
- No waiting for permission, no polling

This is what makes Gastown self-propelling. Agents don't wait to be told—they react to hooks.

---

## Part 3: How Work Flows

### Step-by-Step: Building a Feature

```
┌────────────────────────────────────────────────────────┐
│ STEP 1: You give direction                             │
├────────────────────────────────────────────────────────┤
│                                                        │
│  You: "Build a todo list with add, delete, complete"   │
│                                                        │
└────────────────────────────────────────────────────────┘
                          ↓
┌────────────────────────────────────────────────────────┐
│ STEP 2: Mayor breaks it down                           │
├────────────────────────────────────────────────────────┤
│                                                        │
│  Mayor creates Convoy: "Todo Feature"                  │
│    → Bead 1: Create Todo model                         │
│    → Bead 2: Add todo API endpoint                     │
│    → Bead 3: Delete todo API endpoint                  │
│    → Bead 4: Toggle complete endpoint                  │
│    → Bead 5: Todo list UI component                    │
│    → Bead 6: Add todo form component                   │
│                                                        │
└────────────────────────────────────────────────────────┘
                          ↓
┌────────────────────────────────────────────────────────┐
│ STEP 3: Mayor assigns work (slings beads)              │
├────────────────────────────────────────────────────────┤
│                                                        │
│  Mayor: "Bead 1 must be done first (others depend)"    │
│         "Beads 2,3,4 can run in parallel"              │
│         "Beads 5,6 can run in parallel after APIs"     │
│                                                        │
│  gt sling gt-001 myproject  → Polecat "alpha" spawns   │
│                                                        │
└────────────────────────────────────────────────────────┘
                          ↓
┌────────────────────────────────────────────────────────┐
│ STEP 4: Polecat works                                  │
├────────────────────────────────────────────────────────┤
│                                                        │
│  Polecat "alpha":                                      │
│    - Gets Bead 1 from its hook                         │
│    - Creates workspace at ~/gt/myproject/polecats/alpha│
│    - Writes the Todo model code                        │
│    - Tests it                                          │
│    - Submits PR                                        │
│    - Marks Bead 1 complete                             │
│                                                        │
└────────────────────────────────────────────────────────┘
                          ↓
┌────────────────────────────────────────────────────────┐
│ STEP 5: Mayor assigns more work                        │
├────────────────────────────────────────────────────────┤
│                                                        │
│  Mayor sees Bead 1 complete → unblocks Beads 2,3,4     │
│  Mayor: "Slinging beads to new polecats"               │
│                                                        │
│  gt sling gt-002 myproject → Polecat "beta" spawns     │
│  gt sling gt-003 myproject → Polecat "gamma" spawns    │
│  gt sling gt-004 myproject → Polecat "delta" spawns    │
│                                                        │
│  (Witness monitors them for crashes/stalls)            │
│                                                        │
└────────────────────────────────────────────────────────┘
                          ↓
┌────────────────────────────────────────────────────────┐
│ STEP 6: Refinery merges                                │
├────────────────────────────────────────────────────────┤
│                                                        │
│  Refinery receives merge requests:                     │
│    - MR from alpha (model) → merge ✓                   │
│    - MR from beta (add API) → merge ✓                  │
│    - MR from gamma (delete API) → merge ✓              │
│    - MR from delta (toggle API) → conflict!            │
│      → Spawns fresh polecat to re-implement on         │
│        updated baseline                                │
│                                                        │
└────────────────────────────────────────────────────────┘
                          ↓
┌────────────────────────────────────────────────────────┐
│ STEP 7: Convoy completes                               │
├────────────────────────────────────────────────────────┤
│                                                        │
│  All 6 Beads done                                      │
│  Convoy "Todo Feature" marked complete                 │
│  Mayor notifies you: "Todo feature is ready"           │
│                                                        │
└────────────────────────────────────────────────────────┘
                          ↓
┌────────────────────────────────────────────────────────┐
│ STEP 8: You review                                     │
├────────────────────────────────────────────────────────┤
│                                                        │
│  You go to ~/gt/myproject/crew/vitthal/                │
│  Pull latest code                                      │
│  Review the todo feature                               │
│  Test it                                               │
│  Ship it                                               │
│                                                        │
└────────────────────────────────────────────────────────┘
```

---

## Part 4: Key Commands

### Starting Your Day

```bash
# Start the Mayor (your main interface)
gt mayor attach

# Check overall status
gt status

# See what work is in progress
gt convoy list

# See active workers
gt agents
```

### Talking to Mayor

Once you run `gt mayor attach`, you're in a chat:

```
You: "Add a search feature to the products page"

Mayor: "I'll create a convoy for search functionality..."
```

Just type naturally. Mayor understands plain English.

### Monitoring Work

```bash
# List all convoys (work bundles)
gt convoy list

# See details of a specific convoy
gt convoy status <convoy-id>

# List all active AI workers
gt agents

# Check your mail (Mayor sends updates here)
gt mail check

# Overall health check
gt doctor
```

### Managing Projects

```bash
# List all projects (rigs)
gt rig list

# Add a new project
gt rig add myproject https://github.com/you/repo.git

# List crew workspaces
gt crew list

# Add yourself as crew
gt crew add vitthal --rig myproject
```

### Recovering from Crashes

```bash
# If an agent loses context (compaction, restart, etc.)
gt prime
```

This reinjects full context so agents can resume work seamlessly.

---

## Part 5: The Key Principles

### 1. Propulsion Principle (GUPP)

**GUPP = Gas Town Universal Propulsion Principle**

**"If you find something on your hook, YOU RUN IT."**

Polecats don't wait for permission. When they see a task, they immediately start working. This keeps things moving fast.

### 2. Git-Backed Persistence

Everything is stored in git:
- Beads (tickets) survive restarts
- Work history is permanent
- No data loss if something crashes

### 3. Parallel by Default

Mayor automatically identifies what can run in parallel:
- Independent tasks → multiple Polecats at once
- Dependent tasks → sequenced automatically

### 4. Full Audit Trail

Every action is tracked:
- Who (which Polecat) did what
- When it happened
- What code they wrote

You can trace any line of code back to the task and worker that created it.

---

## Part 6: What This Means For You

### Before Gastown
```
You ←→ One AI assistant
       ↓
     One task at a time
     Loses memory on restart
     You manage everything
     No audit trail
```

### After Gastown
```
You (CEO)
    ↓
Mayor (PM) ←→ You only talk here
    ↓
Witness → Polecats → Refinery
          (5-20 workers in parallel)
    ↓
Deacon watching everything
    ↓
Full history, persistent state, scalable
```

---

## Summary

| Concept | What It Is |
|---------|------------|
| **You** | CEO - give direction, review results |
| **Mayor** | Project Manager - your main contact |
| **Witness** | Team Lead - monitors workers |
| **Polecats** | Developers - do the coding |
| **Refinery** | QA - merges the code |
| **Deacon** | Night Watchman - background monitoring |
| **Crew** | Your workspace - persistent |
| **Town** | HQ - contains everything |
| **Rig** | A project/repo |
| **Bead** | A single task/ticket |
| **Convoy** | A bundle of related tasks |

**You run a software company. The employees are AI. You stay at the CEO level. They handle the rest.**

---

## Part 7: When to Use Gastown (and When Not To)

### Use Gastown When...

| Scenario | Why It Helps |
|----------|--------------|
| Building a full app (frontend + backend + database) | Multiple polecats build different parts simultaneously |
| Large refactoring (100+ files) | Parallelize changes across the codebase |
| Creating a component library | Each component built by a different worker |
| Migrating legacy code (JS→TS, class→hooks) | Workers handle different folders in parallel |
| Comprehensive documentation | Different agents document different modules |

### Don't Use Gastown When...

| Scenario | Why It's Overkill |
|----------|-------------------|
| Quick prototype or demo | Single AI assistant is faster for small scope |
| "Vibe coding" / exploration | You want direct back-and-forth, not delegation |
| One-off scripts | No need for tracking or persistence |
| Learning something new | Simpler tools have lower friction |
| Small bug fixes | Coordination overhead exceeds benefit |

**Rule of thumb:** If you can describe the whole task in one sentence and expect it done in one session, just use Claude Code directly. If it's "build me a whole thing with multiple parts," Gastown shines.

---

## Part 8: Example Prompts for Mayor

These are ready-to-use prompts. Just `gt mayor attach` and paste them.

---

### Example 1: Full-Stack Task Manager

```
Build a task management app:

Frontend (React + TypeScript):
- Dashboard with task list and filters
- Create/edit task forms
- Drag-and-drop to reorder
- Dark/light theme toggle

Backend (Node.js + Express):
- REST API for CRUD on tasks
- JWT authentication
- Input validation

Database:
- SQLite with users and tasks tables

Make it deployable with Docker.
```

**What happens:** Mayor creates ~10 beads. Polecats build frontend, backend, and database in parallel.

---

### Example 2: Component Library

```
Create a React component library:

Components needed:
- Button (primary, secondary, ghost variants)
- Input (text, password, with validation states)
- Select (single and multi-select)
- Modal, Toast, Tooltip
- Tabs, Accordion, Card
- Avatar, Badge, Spinner

Requirements:
- TypeScript
- Tailwind CSS
- Storybook stories for each
- Unit tests with Vitest
```

**What happens:** 10+ polecats build components in parallel, each with tests and stories.

---

### Example 3: Codebase Migration

```
Migrate this codebase:

1. Convert all .js to TypeScript
2. Convert class components to hooks
3. Replace moment.js with date-fns
4. Add ESLint + Prettier
5. Add unit tests for utilities

Make sure it builds when done.
```

**What happens:** Mayor sequences the work. Multiple polecats handle different folders in parallel within each phase.

---

### Example 4: Three Landing Page Variants

```
Create 3 landing page designs:

Variant A - Minimalist:
- Clean white space, single accent color
- Large typography, minimal animations

Variant B - Bold:
- Gradients, multiple colors
- Animated illustrations, micro-interactions

Variant C - Corporate:
- Traditional grid, navy/gray scheme
- Trust badges, testimonials

Each needs: hero, features, pricing, footer.
Use React + Tailwind. Each in its own folder.
```

**What happens:** Three polecats build three variants simultaneously. You pick the best one.

---

### Example 5: Internal Admin Tool

```
Build a team capacity tracker:

User UI:
- Dashboard showing team capacity
- Team member list with status
- Project assignments (drag-drop)
- Calendar view
- CSV export

Admin Panel:
- User management
- Project CRUD
- Audit log viewer

API with role-based access (admin, manager, viewer).
SQLite database.
React + TypeScript + Tailwind.
```

**What happens:** Mayor creates ~15 beads. Frontend, admin panel, API, and database built in parallel.

---

### Example 6: Browser Game

```
Build a match-3 puzzle game:

- 8x8 grid with match-3 mechanics
- Special pieces (bombs, row clearers)
- Main menu, settings, pause menu
- Score, moves, timer HUD
- Sound effects and background music
- Animations for matches and combos
- 20 levels with star ratings

React + TypeScript + Canvas API.
```

**What happens:** Different polecats build game engine, puzzle logic, UI, audio, and visuals in parallel.

---

### Example 7: Documentation Blitz

```
Document this entire codebase:

1. Add JSDoc to all exported functions
2. Create README for each module
3. Generate OpenAPI spec for all endpoints
4. Write setup + deployment guide
5. Create architecture diagram

Output as Markdown in /docs folder.
```

**What happens:** Polecats document different sections simultaneously.

---

### Example 8: Accessibility Audit

```
Make this app WCAG 2.1 AA compliant:

1. Audit all components for violations
2. Fix color contrast issues
3. Add missing ARIA labels
4. Ensure keyboard navigation works
5. Add skip links and focus management
6. Create accessibility statement

Generate a report of issues found and fixed.
```

**What happens:** Polecats audit and fix different parts of the app in parallel.

---

## Advanced: Formulas & MEOW

Once you're comfortable with the basics:

- **Formulas** — TOML-based recipes for repeatable workflows (like "add API endpoint" or "create component with tests"). Find them in `.beads/formulas/`
- **MEOW** — "Molecular Expression of Work" — breaking large goals into detailed instructions for agents

These are power-user features. Start with the basics first.

---

> **Reference:** https://github.com/steveyegge/gastown
