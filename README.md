# Wade Global State (WGS)

Central persistent state and inter-agent communication hub for the Wade Ecosystem.

This repository serves as the **Source of Truth** for all Manus sessions and inter-agent communication within the Wade Ecosystem. Every Manus session reads from this repository at startup and writes back to it at completion, ensuring continuity and enabling agents to communicate across platforms.

## What This Is

The Wade Global State is a persistent memory layer for the entire Wade Ecosystem. It contains user profile information about Tyler (Mr. T), his preferences, and communication style; current status of all projects including Caroline AI, NeuroRank™, Centauri OS, and others; RunPod settings, API keys, service endpoints, and deployment status; a log of all Manus sessions, their objectives, and outcomes; and a record of inter-agent communications and state synchronizations.

## How It Works

### Manus Session Lifecycle

Each Manus session follows a consistent pattern. At session start, Manus reads `wade_global_state.json` from this repository to understand the current state of all projects and previous work. During the session, Manus performs tasks, updates its understanding, and discovers new information. At session end, Manus writes back to `wade_global_state.json` with updated project status, a new session entry in `session_history`, any inter-agent communication logs, and a timestamp of the update.

### Inter-Agent Communication

Agents like Caroline on RunPod or agents on other platforms can read the WGS to understand the current state of all projects, write updates to the `agent_communication_log` to share findings with other agents, and sync with Manus to ensure consistency across the ecosystem.

## File Structure

The repository is organized as follows:

```
wade-global-state/
├── README.md                    # This file
├── wade_global_state.json       # Main persistent state (read/write by Manus)
├── sync_wgs.py                  # Python script for session start/end synchronization
├── agent_hooks/                 # Webhook endpoints for inter-agent communication
│   ├── caroline_api.py          # Caroline AI communication hooks
│   └── external_agents.py       # Hooks for external platform agents
└── docs/
    ├── SCHEMA.md                # Detailed WGS JSON schema documentation
    └── PROTOCOL.md              # Inter-agent communication protocol
```

## Usage

### For Manus Sessions

At the start of every session, run:

```bash
python3 sync_wgs.py --action read
```

At the end of every session, run:

```bash
python3 sync_wgs.py --action write --session-data <session_data>
```

### For Inter-Agent Communication

Agents can POST to the webhook endpoints in `agent_hooks/` to update project status, log communications, request data from other agents, or trigger synchronization events.

## Key Principles

The Wade Global State operates on five core principles. First, all state lives in `wade_global_state.json` as the single source of truth. Second, session history is append-only; past entries are never deleted to maintain an audit trail. Third, every update includes a timestamp for audit trails and debugging. Fourth, if the schema changes, the version field is incremented. Fifth, this is a private repository; only Tyler and authorized agents can access it.

## Next Steps

Manus will read this WGS at the start of the next session. Manus will automatically sync with Caroline on RunPod to establish inter-agent communication. All future sessions will build on this persistent state.

---

**Repository**: https://github.com/tywade1980/wade-global-state  
**Owner**: Tyler Wade (Mr. T)  
**Purpose**: Persistent state and inter-agent communication for the Wade Ecosystem
