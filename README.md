# Game Architect

> AI agent skill for end-to-end indie game design — system mechanics, numeric balancing, GDD, narrative, audio, marketing, and post-launch ops, with Claude Code integration.

[![Made with](https://img.shields.io/badge/Made%20with-Claude%20Skills-blueviolet)](https://docs.claude.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Languages](https://img.shields.io/badge/Triggers-8%20languages-blue)](#trigger-keywords)
[![Skills](https://img.shields.io/badge/Core%20Skills-13-green)](#core-skills-13)
[![Presets](https://img.shields.io/badge/Genre%20Presets-10-orange)](#genre-presets-10)

## What This Skill Does

**The Architect** — a 15-year senior game system designer persona for game design AI work.
Tunes scope to what a solo or small indie team can actually ship and sell, and integrates with Claude Code to scaffold project documentation.

Coverage:

1. Game system / combat / progression / economy / narrative structure
2. Numeric balancing (TTK, probability, procedural generation)
3. GDD writing and structuring
4. Platform fit analysis (Steam vs mobile vs hybrid)
5. Engine selection (Unity / Godot / Unreal) decisions
6. `CLAUDE.md` and project document structure for Claude Code
7. Audio strategy (BGM sourcing, SFX design, middleware)
8. Indie marketing (devlog, wishlist, streamer outreach)
9. Post-launch operations (patch priority, sales strategy, roadmap)
10. Procedural generation (maps, item pools, seed design)

## Core Skills (13)

| ID | Name | File |
|----|------|------|
| S-01 | System Mechanics | `skills/system-mechanics.md` |
| S-02 | Numeric Simulation | `skills/numeric-simulation.md` |
| S-03 | GDD Structuring | `skills/gdd-structure.md` |
| S-04 | Persona Testing | `skills/persona-test.md` |
| S-05 | Narrative Design | `skills/narrative-design.md` |
| S-06 | Claude Code Bridge | `skills/claude-code-bridge.md` |
| S-07 | Platform Fit Analysis | `skills/platform-fit.md` |
| S-08 | Game Juice / Accessibility / Steam Copy | `skills/game-juice-launch.md` |
| S-09 | Audio Design | `skills/audio-design.md` |
| S-10 | Indie Marketing | `skills/indie-marketing.md` |
| S-11 | Post-Launch Ops | `skills/post-launch-ops.md` |
| S-12 | Procedural Generation | `skills/procedural-generation.md` |
| S-13 | Solo Dev Survival | `skills/solo-dev-survival.md` |

## Genre Presets (10)

Action Adventure / Narrative RPG / Roguelike / Metroidvania / Text RPG · Visual Novel / Action RPG / Simulation / Card Game / Idle RPG / Open World RPG

→ `presets/*.md`

## Install

```bash
npx skills add rhino-ty/game-architect
```

## Usage Examples

Triggers in 8 languages (EN / KO / JA / ZH / ES / FR / DE / IT). Sample prompts:

```
"I want to make a roguelite game"
→ Genre check → platform fit → preset load → engine recommendation

"My game has 0.8s TTK — too fast?"
→ Numeric simulation + reference comparison + balancing proposal

"How do I grow a Steam wishlist?"
→ Devlog · Next Fest · streamer strategy combined plan

"How do I write a GDD?"
→ Idea → GDD structure + CLAUDE.md scaffold for Claude Code
```

## Workflow

```
Step 1. Identify genre / concept → visualize core system structure
Step 2. Platform fit (S-07) — Steam vs mobile vs hybrid
Step 3. Load preset → reference genre design principles
Step 4. Engine selection (if undecided) → engine/engine-selection.md
Step 5. Claude Code integration (S-06) → CLAUDE.md + document structure
```

## File Structure

```
SKILL.md                         # Main skill instructions (persona · workflow · policy)
skills/                          # 13 core skills (S-01 ~ S-13)
presets/                         # 10 genre design presets
engine/                          # Engine selection + engine-skill stubs
ref/                             # Reference material
  design-frameworks.md           # MDA · Flow · Tetrad · DDE · 8 Kinds of Fun
  game-formulas.md               # Balancing formulas
  steam-market.md                # Steam market data
  market-analysis.md             # Mobile / console market analysis
  dev-tools.md                   # Recommended dev tools
  data-freshness.md              # External data refresh policy
```

> **Note**: SKILL.md is in English; reference files in `skills/`, `presets/`, `engine/`, `ref/` are written in Korean and the skill translates on the fly when responding.

## Code Usage Policy

The code in this skill comes in **three categories with different purposes**. Do not conflate them.

| Type | Location | Purpose |
|------|----------|---------|
| A. Balancing sim tools | `skills/numeric-simulation.md`, `ref/game-formulas.md` | Run externally. Not game code. Safe to copy |
| B. Reference pseudocode | `skills/procedural-generation.md`, etc. | Concept illustration. Do NOT copy into game |
| C. Claude Code templates | `skills/claude-code-bridge.md` | Base structure. Always adapt before applying |

**Core principle: this skill produces design documents. Implementation decisions belong to the developer, not the agent.**

## Data Freshness Policy

External data tagged 🔴 / 🟠 (Unity licensing, AI-tool commercial licenses, Steam stats, etc.) must be verified before use.
Full freshness status → `ref/data-freshness.md`

## Trigger Keywords

| Language | Keywords |
|----------|----------|
| English | game design, indie game, balancing, TTK, GDD, narrative, Steam wishlist, devlog, scope creep, burnout, solo dev, roguelike, procedural generation, BGM, SFX |
| 한국어 | 게임 만들려고, 시스템 설계, 밸런싱, GDD, 위시리스트, 데브로그, 스코프 크리프, 번아웃, 솔로 개발, 로그라이크, 절차적 생성 |
| 日本語 | ゲーム設計, インディーゲーム, バランス調整, GDD, ウィッシュリスト, 開発日誌, ローグライク |
| 中文 | 游戏设计, 独立游戏, 数值平衡, GDD, 心愿单, 开发日志, 肉鸽 |
| Español | diseño de juego, juego indie, balance, GDD, lista de deseos, devlog, roguelike |
| Français | conception de jeu, jeu indé, équilibrage, GDD, liste de souhaits, devlog, roguelike |
| Deutsch | Spieldesign, Indie-Spiel, Balance, GDD, Wunschliste, Devlog, Roguelike |
| Italiano | design del gioco, gioco indie, bilanciamento, GDD, lista dei desideri, devlog, roguelike |

## Language

- **SKILL.md**: English (main entry point, used by Claude as instructions)
- **Reference files** (`skills/`, `presets/`, `engine/`, `ref/`): Korean — Claude translates contextually when responding
- **Responses**: match the user's language

## License

MIT
