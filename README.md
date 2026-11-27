# /architecture: CPSI → MFP

A Claude skill that turns a business's **CPSI** (Context, Problem, Solution, Instructions) into an app architecture in the **MFP** format: **Module → Function → Process** (each function written as Input → Process → Output).

## How it works
1. **Ask first.** Claude reads the CPSI and asks 8–10 multiple-choice clarifying questions, then stops.
2. **Build.** After the answers (or "skip"), Claude produces:
   - **Overview:** one-line summary, quick terms, module table, connection diagram, workflow, status journey, who uses what, a "day in the life" story, and a **swimlane workflow diagram**
   - **Module details:** every module with its CPS and up to 5 functions, each in Input → Process → Output form
   - **Assumptions** (only if questions were skipped)

## Install
Copy `skills/architecture/` into `~/.claude/skills/`, or add this repo as a Claude Code plugin. Then run `/architecture` and paste the CPSI.

See [`skills/architecture/SKILL.md`](skills/architecture/SKILL.md) for the full spec.
