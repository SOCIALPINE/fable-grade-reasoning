# fable-grade-reasoning

A verification-first reasoning discipline for **Claude Opus-family models**, distilled from a controlled differential analysis of reasoning traces between a Fable-class model and Claude Opus 4.8 across code-generation and AI-research-math missions.

Every rule in this skill exists because its absence produced a real, documented failure. The evidence lives in [`skills/fable-grade-reasoning/references/case-studies.md`](skills/fable-grade-reasoning/references/case-studies.md).

## What it does

On qualifying tasks (non-trivial code, algorithm implementation, mathematical derivation, numerical work, debugging, AI research), the skill enforces:

- **Grounding** — a number is `[OBSERVED]` only if a tool actually executed; everything else is `[PREDICTED]`. Fabricated observations are treated as the worst possible outcome.
- **Template audits** — retrieving a known implementation or textbook derivation is fine; inheriting its flaws silently is not. Named standard algorithms count as templates, and implementation variants (e.g. Adam's epsilon placement) must be declared.
- **Paper-before-code** — invariants and divergence conditions pinned before writing recursive/graph/cycle-handling code.
- **Independent verification** — dual-path derivations, invariant cross-checks, dimension audits, SE-unit numeric spot-checks, exact boundary classification (CONTRACT / MARGINAL / DIVERGE).
- **A doubt ledger** — every residual doubt gets a disposition (`CLOSED` / `OPEN-COST` / `OPEN-ENV` / `OPEN-CHOICE`), and cheap doubts must be closed, not documented.
- **Claude Code patch loop** — reproduce-first, minimal-diff, full-suite re-verification for patching and upgrading codebases (or this skill itself).
- **Frontier mode** — assumption inventories, reduction checks, sanity ladders, equal-budget baselines, and multi-seed SE-unit judgment for novel-field / new-architecture research.
- **Language protocol** — reasons internally in English, responds in the user's language.

## Model gate

The skill self-gates by model family (see `STEP 0` in SKILL.md):

| Model | Behavior |
|---|---|
| Claude Opus family (4.8, 4.x, later) | Applies in full |
| Claude Fable / Mythos class | Skips itself (it was distilled *from* Fable-class traces) |
| Other models | Optional; Opus is the primary target |

## Install

### Claude Code (marketplace — recommended)
```
/plugin marketplace add SOCIALPINE/fable-grade-reasoning
/plugin install fable-grade-reasoning@fable-grade-reasoning
```

### Claude Code (manual)
```bash
git clone https://github.com/SOCIALPINE/fable-grade-reasoning.git
cp -r fable-grade-reasoning/skills/fable-grade-reasoning ~/.claude/skills/
```
Use the project-level `.claude/skills/` directory instead for a per-project install.

### Claude.ai (web/desktop)
Upload `dist/fable-grade-reasoning.zip` (or the `.skill` file) in the skills section of Settings, if your plan/organization allows custom skills.

### Claude API
Upload the skill via the Skills API — see Anthropic's documentation on creating custom skills.

## Evidence-based development

This skill was built and is maintained under its own rules:

- **v1.0** — initial distillation from six audited missions (autodiff engine, softmax/CE gradient derivation, GD convergence proof, numerically-stable losses, ambiguous-spec design, KL derivation). Includes the fabricated-`[OBSERVED]` episode and the canary methodology that eliminated it.
- **v1.1** — patched after a **pre-registered, single-pair A/B comparison** (Adam-from-scratch mission, skill vs no-skill, n=1 per condition). In that pair, the skill condition exhibited 4 targeted behaviors the control lacked (grounded output transcripts, boundary-limit resolution, a non-vacuous invariance test, a doubt ledger) with no cost regressions. This is **existence-level evidence, not a measured effect size** — one generation per condition cannot separate skill effect from sampling noise. Both conditions missed the template-variant audit (Adam's epsilon placement), so Phase 2 was strengthened accordingly. A standardized harness (multiple missions × multiple generations per condition, scripted via API, blind-scored objective metrics) is the planned path to a real effect measurement.
- **v1.2** — added the Claude Code patch/upgrade loop and Frontier Mode (novel fields and architectures). These two sections generalize audited principles and are marked as such pending their own field evidence.

## Disclaimer

This is an **unofficial, community-made skill**. It is not affiliated with or endorsed by Anthropic. "Claude", "Claude Opus", and related model names belong to Anthropic. The skill is provided as-is under the MIT license; test it in your own environment before relying on it.

---

# 한국어 요약

Fable급 모델과 Claude Opus 4.8의 사고 흔적을 통제 비교 분석해 증류한 **검증 우선 사고 규율** skill입니다. 모든 규칙은 문서화된 실제 실패 사례를 근거로 합니다(case-studies.md). 사전 등록된 단일 쌍(조건당 1회, n=1) A/B 비교에서 목표 행동들이 관측되었으나, 이는 존재 수준의 증거이지 측정된 효과 크기가 아닙니다 — 규격화된 다중 생성 측정은 예정되어 있습니다. Opus 계열 모델에서만 발동하고 Fable/Mythos 계열은 자동으로 건너뜁니다(모델 게이트). 내부 사고는 영어로, 응답은 사용자의 언어로 수행합니다. 설치는 위 Install 섹션을 따르세요 — Claude Code에서는 마켓플레이스 등록 두 줄이면 됩니다.
