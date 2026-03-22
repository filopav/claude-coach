# Training System — Setup & Workflow

A Claude Code-powered training system that reads your history, adapts to how you actually feel, and uploads structured workouts directly to your Garmin watch.

---

## Setup

### 1. Install MCP servers in Claude Code

```bash
# Read training data from Garmin
uvx --python 3.12 --from git+https://github.com/Taxuspt/garmin_mcp garmin-mcp-auth
claude mcp add garmin --command uvx -- --python 3.12 --from git+https://github.com/Taxuspt/garmin_mcp garmin-mcp

# Upload + schedule workouts to Garmin
claude mcp add garmin-workouts-mcp npx garmin-workouts-mcp
```

### 2. Fill in your athlete profile

Edit `athlete-profile.md`:
- Your HR zones (find in Garmin Connect → Profile → HR Zones)
- Your goal event and date
- Available training days
- Any injury history

### 3. Copy plan + log templates each week

```bash
cp plans/2026-W11.md plans/2026-W12.md
cp log/2026-W11-log.md log/2026-W12-log.md
```
Then update the week numbers inside the files.

---

## Weekly Workflow

### During the week — after each session
Open `log/YYYY-WXX-log.md` and fill in the session:
- Did you do it or skip it?
- How did it feel (1–10)?
- Any notes (pace, HR, why you skipped)

Takes ~2 minutes. This is the most important habit.

### End of week — Sunday evening
Fill in the **Week Summary** section at the bottom of the log:
- Sessions completed
- Overall fatigue
- Sleep quality
- Life stress
- Anything Claude should know

### New week — Sunday/Monday
Open Claude Code in this directory and say:

> **"Read my W11 plan and log, then generate a W12 plan and upload the workouts to Garmin."**

Claude will:
1. Compare what was scheduled vs. what happened
2. Adapt the new week based on your feedback
3. Generate and save `plans/2026-W12.md`
4. Upload structured workouts to Garmin Connect
5. Schedule them on the right dates (→ syncs to your watch)
6. Update `athlete-profile.md` with any new patterns

---

## Other Useful Prompts

**Mid-week adjustment:**
> "I tweaked my knee on Thursday. Read this week's plan and adjust the remaining sessions."

**Fatigue check:**
> "Look at the last 4 weeks of logs and tell me if I'm accumulating fatigue."

**Pre-race taper:**
> "My race is in 10 days. Read my last 6 weeks and generate a taper plan."

**Honest assessment:**
> "Based on my logs, am I actually progressing or just going through the motions?"

---

## File Structure

```
training/
├── CLAUDE.md                  ← Claude's instructions (don't edit)
├── README.md                  ← This file
├── athlete-profile.md         ← Your baseline + Claude's pattern notes
├── plans/
│   ├── 2026-W11.md            ← What was scheduled
│   └── 2026-W12.md
└── log/
    ├── 2026-W11-log.md        ← What actually happened
    └── 2026-W12-log.md
```

---

## Tips

- **The log is everything.** Claude is only as smart as the context you give it. A 2-minute log entry after each session is what makes the whole system work.
- **Be specific about how you felt.** "Felt tired" is less useful than "HR was 10bpm higher than usual at the same pace" or "couldn't hold threshold pace after interval 3."
- **Don't skip the week summary.** The life stress / sleep fields matter — they're often the explanation for a bad training week.
- **Garmin auth tokens expire ~every 5 min.** When uploading a full week, do it in one Claude Code session without long pauses.
