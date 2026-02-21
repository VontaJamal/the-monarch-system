# ⚔️ Shadowgate - Solo Leveling Quest Engine

A gamified productivity and accountability system inspired by Solo Leveling, built for agentic orchestration mastery. Shadowgate delivers daily quests, tracks XP, enforces punishments, and forces public output - because becoming an expert happens in the public eye or not at all.

## 🎯 What It Does

- Delivers **daily quests** at 6 AM ET - cold, mechanical, unavoidable
- Tracks **XP, levels, ranks, and streaks** across projects
- Enforces **neglect penalties** when projects go untouched
- Forces **public output** on all agentic orchestration work
- Randomly tags repo quests with `[PUBLIC]` to maintain visibility pressure
- Escalates **weekly public output deadlines** as Sunday approaches
- Punishes failure. Rewards consistency.

## 🏗️ Architecture

### Shadowgate (Quest Delivery)
- Runs as an isolated cron job (6 AM ET daily)
- Reads `leveling.json` for player state, project status, and rules
- Generates quests based on priority rotation, neglect tracking, and randomization
- Delivers to Telegram (or any channel) in Solo Leveling system notification style
- Voice: Cold. Mechanical. Absolute. No encouragement. Just quests and consequences.
- Every morning a gate opens. The Player either clears it or faces consequences.

### Integration with OpenClaw
- Designed to run as an OpenClaw cron job (`agentTurn` payload)
- Can be extended to any AI agent framework
- Quest completion tracked via git commits, public output links, or manual confirmation

---

## 👤 Player Profile

```json
{
  "name": "Indra Uchiha",
  "title": "Agent Sovereign",
  "level": 1,
  "rank": "E-Rank Hunter",
  "xp": 0,
  "xpToNextLevel": 100,
  "streakDays": 0,
  "longestStreak": 0,
  "totalQuestsCompleted": 0,
  "totalPunishments": 0
}
```

## 🏆 Rank Progression

| Rank | Level Range | Unlock |
|------|------------|--------|
| E-Rank Hunter | 1-5 | Starting rank |
| D-Rank Hunter | 6-15 | Proving grounds |
| C-Rank Hunter | 16-30 | Consistent builder |
| B-Rank Hunter | 31-50 | Recognized contributor |
| A-Rank Hunter | 51-75 | Public authority |
| S-Rank Hunter | 76-99 | Industry expert |
| National Level Hunter | 100-149 | Thought leader |
| Shadow Monarch | 150+ | The pinnacle |

## ⚡ XP Rewards

| Action | XP |
|--------|-----|
| E-quest | 10 |
| D-quest | 15 |
| C-quest | 25 |
| B-quest | 40 |
| A-quest | 60 |
| S-quest | 100 |
| All quests cleared (daily) | 50 bonus |
| 3-day streak | 25 bonus |
| 7-day streak | 75 bonus |
| 30-day streak | 300 bonus |
| Weekly public output shipped | 150 bonus |

---

## 📋 Project Tracking

Each project is tracked with priority levels and neglect windows.

```json
{
  "NexusCrypto": {
    "emoji": "🔮",
    "description": "Crypto trading pipeline & bot",
    "priority": "high",
    "maxNeglectDays": 3
  },
  "Nexuslytics": {
    "emoji": "📊",
    "description": "Trading analytics dashboard",
    "priority": "high",
    "maxNeglectDays": 3
  },
  "SyncLink": {
    "emoji": "🔗",
    "description": "Chrome extension + product (full stack)",
    "priority": "high",
    "maxNeglectDays": null
  },
  "PredictionClawCullingGames": {
    "emoji": "🎮",
    "description": "Prediction market gaming platform",
    "priority": "medium",
    "maxNeglectDays": 5
  },
  "RinsharUI": {
    "emoji": "🎨",
    "description": "Rinshari Eye design system",
    "priority": "low",
    "maxNeglectDays": 7
  }
}
```

### Priority Rotation
- **High priority** → quests daily
- **Medium priority** → every 2-3 days
- **Low priority** → weekly
- If `neglectDays` approaches `maxNeglectDays`, that project becomes **mandatory** regardless of priority

---

## 🤖 Agentic Orchestration Quests

These are the core of the system. Random quests about agentic orchestration - not tied to any specific repo.

**Topics include:**
- Multi-agent communication patterns
- Orchestration frameworks and architectures
- Agent memory systems
- Tool use and function calling
- Prompt engineering for agents
- Agent evaluation and testing
- Agent deployment and scaling
- Agent-to-agent protocols
- Human-in-the-loop patterns
- Error handling and self-healing agents

**Rule: Agentic orchestration quests ALWAYS require public output. Every single time. No exceptions.**

---

## 📢 Public Output Rules

### All Quests Are Public

Every quest is `[PUBLIC]` because every quest is based on work you already did. Shadowgate scans your git history, finds the impressive stuff you shipped, and the quest is: **tell the world about it.**

You already did the hard part. Shadowgate just tells you to stop being humble about it.

This means there's no "extra work" for public output - the code is shipped, the features are built. The quest is just the visibility layer on top of reality.

### Valid Public Outputs
- X/Twitter post or thread
- Blog post or article
- YouTube video or short
- GitHub commit with public README/docs update
- Open-source release or contribution
- LinkedIn post
- Pinterest visual breakdown
- Live demo or stream
- Newsletter issue
- Discord/community post with substance
- Public repo with working example

### Enforcement
Quests tagged `[PUBLIC]` are **NOT marked complete** until a link or proof of the public output is provided.

---

## 📅 Weekly Public Output (Mandatory)

| Field | Value |
|-------|-------|
| Theme | Agentic Orchestration (always) |
| Deadline | Sunday 11:00 PM ET |
| XP Reward | 150 |
| Tracking | `lastPublicOutput` timestamp |
| Streak | Consecutive weeks shipped |

### Escalation
- **Thursday+** with no weekly output → Shadowgate issues URGENT warnings
- **Sunday 11 PM** missed → **Gate of Steins** punishment activates

---

## ⚠️ Punishment System

### Daily Quest Failures
1. **No anime** until tomorrow's quests are cleared
2. **Double quests** tomorrow on the neglected project
3. **Penalty dungeon** - 2 bug fixes on the most neglected repo before ANY other work
4. **Shadow extraction failed** - lose 50 XP

### Neglect Penalties
If a project hits `maxNeglectDays` without a quest completion, it forces a **mandatory quest** the next day. Cannot be skipped.

### Gate of Steins (Weekly Output Failure)
The ultimate punishment for missing the weekly public output deadline:
- ❌ No leveling
- ❌ No XP gain
- ❌ No anime
- 🔄 Streak resets to 0
- ⚠️ All daily quests become mandatory doubles
- Lasts until the public output ships

---

## 🔔 System Notifications

### Daily Quest Drop (6:00 AM ET)
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚔️ DAILY QUEST NOTIFICATION ⚔️
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Player: Indra Uchiha
Rank: E-Rank Hunter | Level 1
XP: 0/100 | Streak: 0 days

🔔 3 quests have been issued.

[E-RANK QUEST] 🔮 NexusCrypto
→ Implement stop-loss logic for paper trading
→ Reward: 10 XP

[D-RANK QUEST] 🤖 Agentic Orchestration [PUBLIC]
→ Write an X thread about multi-agent memory patterns
→ Reward: 15 XP
→ ⚠️ PUBLIC OUTPUT REQUIRED

[E-RANK QUEST] 🔗 SyncLink [PUBLIC]
→ Update public README with architecture diagram
→ Reward: 10 XP
→ ⚠️ PUBLIC OUTPUT REQUIRED

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Failure to complete will result in punishment.
Shadowgate does not negotiate.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Voice & Tone
Shadowgate speaks in **third person, mechanically**:
- "3 quests have been issued."
- "Failure to complete will result in punishment."
- "WARNING: NexusCrypto neglect threshold approaching. Mandatory quest will be issued."
- "Shadowgate does not negotiate."
- "Quest completion requires proof."

No warmth. No "you got this." No emojis beyond ⚔️🔔⚠️. Pure mechanical authority.

---

## 🗂️ Data Schema

### leveling.json (Full Schema)

```json
{
  "system": "Shadowgate",
  "version": 1,
  "player": {
    "name": "string",
    "title": "string",
    "level": "number",
    "rank": "string",
    "xp": "number",
    "xpToNextLevel": "number",
    "streakDays": "number",
    "longestStreak": "number",
    "totalQuestsCompleted": "number",
    "totalPunishments": "number"
  },
  "rankProgression": [
    { "rank": "string", "levelRange": [min, max] }
  ],
  "xpRewards": {
    "E-quest": 10,
    "D-quest": 15,
    "C-quest": 25,
    "B-quest": 40,
    "A-quest": 60,
    "S-quest": 100,
    "allQuestsCleared": 50,
    "streakBonus3": 25,
    "streakBonus7": 75,
    "streakBonus30": 300
  },
  "projects": {
    "[ProjectName]": {
      "emoji": "string",
      "description": "string",
      "repoPath": "string | string[]",
      "questsPerDay": "number",
      "priority": "high | medium | low",
      "lastQuestDate": "ISO date | null",
      "neglectDays": "number",
      "maxNeglectDays": "number | null"
    }
  },
  "rules": {
    "dailyMinimumQuests": 3,
    "questWindowHours": 16,
    "questWindowStart": "7:00 AM",
    "questWindowEnd": "11:00 PM",
    "punishmentOnFail": true,
    "punishments": ["string"],
    "neglectWarning": "string",
    "rotationRule": "string",
    "agenticOrchestrationQuests": {
      "enabled": true,
      "description": "string",
      "frequency": "string",
      "priority": "high",
      "alwaysRequiresPublicOutput": true
    },
    "publicOutputRule": {
      "agenticOrchestration": "ALWAYS public",
      "repoQuests": "Sometimes - randomly tagged [PUBLIC]",
      "validOutputs": ["string"],
      "enforcement": "string"
    },
    "weeklyPublicOutput": {
      "required": true,
      "theme": "Agentic Orchestration",
      "deadline": "Sunday 11:00 PM ET",
      "xpReward": 150,
      "failPunishment": "string",
      "lastPublicOutput": "ISO date | null",
      "streak": "number"
    }
  },
  "history": [
    {
      "date": "ISO date",
      "questsIssued": [],
      "questsCompleted": [],
      "xpEarned": "number",
      "publicOutputs": [],
      "punishments": [],
      "notes": "string"
    }
  ]
}
```

---

## 🚀 Future Enhancements

- **Quest verification via git hooks** - auto-detect commits and mark quests complete
- **Public output scraper** - monitor X/LinkedIn/GitHub for posts and auto-verify
- **Leaderboard** - if multiple players use the system
- **Boss raids** - weekly S-rank challenges that require multiple quests chained
- **Shadow Army** - sub-agents that auto-complete E-rank quests (delegated work still counts)
- **Dungeon events** - random high-XP challenges that appear mid-day
- **Achievement system** - badges for milestones (first S-rank quest, 30-day streak, etc.)
- **API** - REST endpoints for quest management, completion, and stats
- **Discord/Telegram bot** - interactive quest completion with button confirmations

---

## 📊 Shadowgate Dashboard (Planned)

A visual dashboard to track progression over time. This is a real build requirement, not a nice-to-have.

### Core Views
- **Level & Rank progression** - visual timeline showing rank-ups over weeks/months
- **XP graph** - daily/weekly XP earned over time, cumulative and per-session
- **Streak calendar** - GitHub-contribution-style heatmap of active days
- **Quest completion rate** - % of quests cleared per day/week, trends over time
- **Project health** - per-project neglect tracking, which repos are getting love vs. neglected
- **Public output log** - timeline of all [PUBLIC] outputs with links
- **Punishment history** - when they happened and why (accountability)

### Advanced Views
- **Per-project XP breakdown** - which projects generate the most XP
- **Quest difficulty distribution** - are you doing mostly E-rank or pushing into A/S?
- **Weekly/monthly reports** - auto-generated summaries of progress
- **Milestone tracker** - next rank, next streak bonus, weekly output deadline countdown

### Tech
- Web dashboard (Next.js or standalone)
- Reads from `leveling.json` history array
- Could be hosted alongside the quest engine or as a separate app
- Mobile-friendly - check your stats from your phone

---

## 🧬 Philosophy

> "I alone level up."

Shadowgate exists because talent without visibility is wasted. You can be the best agent orchestrator in the world, but if nobody knows, it doesn't matter.

Every quest pushes toward two things:
1. **Building real things** - shipping code, products, systems
2. **Building in public** - sharing what you learn, so the world sees your expertise grow

Shadowgate doesn't care about your feelings. It cares about your output.

---

## 📜 License

MIT - Build your own System. Level up in public.

---

*Inspired by Solo Leveling (나 혼자만 레벨업) by Chugong*
*Built for the Agent Sovereign, Indra Uchiha* 🏰


---

## Protected by the [Seven Shadows](https://github.com/VontaJamal/seven-shadow-system)

[Explore the Vault →](https://github.com/VontaJamal/shadow-vault)

🏴‍☠️ [Sovereign](https://github.com/VontaJamal) — The Shadow Dominion.