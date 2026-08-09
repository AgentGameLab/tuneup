> **This skill now lives in [AgentGameLab/skills](https://github.com/AgentGameLab/skills)**, together with our other agent skills, as one installable Claude Code plugin — `/plugin marketplace add AgentGameLab/skills`. This repo stays up and still works standalone, but new versions land there.
> **本 skill 已迁入 [AgentGameLab/skills](https://github.com/AgentGameLab/skills)** 单仓，与其余 skill 合并为一个可安装的 Claude Code plugin。本仓保留可用，但后续更新只发新仓。

# tuneup

> An Agent Skill (SKILL.md format) that turns a fuzzy request into a high-information-density prompt — no prompt-engineering knowledge required from the user.
> 一个把模糊需求翻译成高信息密度 Prompt 的 Agent Skill（SKILL.md 格式）——用户不需要懂 prompt 工程。跑在 Claude Code 里，但不是 Claude Code 专属——任何实现了 SKILL.md 加载约定的 harness 都能用。

The user most likely to need TuneUp doesn't know the word "prompt." They're just repeating themselves, rephrasing, getting frustrated that the AI isn't getting it. TuneUp's job is to notice that pattern and step in — not to teach prompt engineering, but to get the user the output they actually wanted.

## What it does

**Triage first** — every request gets routed before anything else happens:
- **Fast lane**: a one-line creative ask with no format constraints gets answered directly, no pipeline.
- **Main lane**: multi-step or format/precision-constrained requests go through a 4-phase forge (scope the need → silently classify into one of 4 quadrants → inject relevant thinking frameworks or a domain-expert lens if the task calls for one → forge the prompt → self-check before showing it).
- **ER lane**: triggers even without being asked — when the user has corrected or rephrased the same request 2+ times, or says "that's not what I meant," TuneUp steps in with a diagnosis, a rewrite, and a before/after comparison.

**Quadrant forging** — internally classifies by complexity (steps ≤2 vs ≥3) and precision (creative vs strict), producing conversational / template / structured-XML / Lisp-style prompts as appropriate. The classification never leaks into the output — the user just sees a better prompt.

**Domain expert lens** — for evaluation/diagnosis/creation/review tasks, equips the prompt with a domain-expert persona (credentials, posture, lineage, vocabulary). Hard-triggers for finance/legal/medical/irreversible-decision domains, which also force a safety boundary: risk-tiered framing instead of authoritative pronouncements, and a mandatory "confirm with a licensed professional" closing line.

**Recipe mode** — on request, turns a one-off prompt into a reusable parameterized template.

## Install

For Claude Code:

```bash
git clone https://github.com/AgentGameLab/tuneup.git ~/.claude/skills/tuneup
```

Claude Code auto-discovers the skill from its `SKILL.md` front-matter. Any other harness that reads the same SKILL.md convention can load it from its own skills directory the same way.

## Layout

```
tuneup/
├── SKILL.md                          # triage + 4-phase forge pipeline + ER diagnostic mode + recipe mode
└── references/
    ├── quadrant-matrix.md            # full classification matrix for the 4 forging styles
    ├── lisp-patterns.md              # Lisp-style prompt pattern library (loaded only for quadrant 4)
    └── domain-expert-lens.md         # domain expert equipping manual + safety-boundary templates
```

References are loaded on demand (progressive disclosure) — most requests never touch them.

## License

MIT — see [LICENSE](LICENSE).
