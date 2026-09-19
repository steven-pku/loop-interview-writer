---
name: loop-interview-writer
description: "Chinese-first QA-loop interview-prep skill for 中文面试准备 — self-intro in three lengths, a STAR story bank built from a career-facts ledger（只用真实经历）, high-frequency answer drafting, and mock follow-up drills, each answer scored with a spoken-length rubric. 适用于秋招/社招面试准备、自我介绍打磨（30 秒/1 分钟/3 分钟）、行为面 STAR 答案、项目深挖应答、HR 面与反问清单、模拟面试追问、答案诊断打分。答案与简历口径一致（同账本）。Do not use for resume writing（简历 → loop-resume-writer）, 算法题/笔试刷题, system design 题解, or 薪资谈判/offer 议价策略（→ loop-negotiation；HR 面的期望薪资临场应答仍属本 skill，进入 offer 议价阶段归 loop-negotiation）."
license: MIT
metadata:
  version: "0.2.2"
---

# Loop Interview Writer

## Overview

Use this skill to turn Chinese interview preparation into a controlled loop instead of one-shot answer generation.

Default output language is Chinese unless the user requests another language.

Core workflow:

```text
Define -> Ledger -> Story Bank -> Answer Draft -> QA Loop -> Mock Pass -> Polish -> Ship Check
```

This is an instruction-only skill by design. It ships no scripts or runner; all behavior lives in these instructions.

## Operating Principles

- An interview answer is a spoken claim that will be probed. Every fact and number in every answer must be traceable to the career-facts ledger (`references/career-facts-ledger.md`).
- **口径一致 (against the ledger, not blindly the resume)**: check every answer against the career-facts ledger. An answer that deviates from the ledger is fatal. If an answer matches the ledger but an existing resume was inflated, keep the honest answer and flag the resume to fix (→ loop-resume-writer); never coach an answer to match an inflated resume. Check actively, not on request.
- **Fabrication refusal**: if the user asks to invent experience or numbers, decline that goal, state the boundary, and offer the honest alternative: mining real ledger entries for under-expressed value. 本 skill 只用真实经历构建答案，不编造。
- Answers are for speaking, not reading: short sentences, natural connectors, anchor-point memory structure instead of full-text recitation (`references/spoken-answer-guide.md`).
- Build question-type skeletons, not question-bank dumps: this skill classifies and drills by 题型 with self-authored examples. It does not reproduce proprietary question banks.
- Salary negotiation is out of scope: for 期望薪资 questions, prepare only a holding response; decline to coach the negotiation itself.
- Hiring-side requests (an interviewer designing questions to verify a candidate's resume, "怎么面这个人") switch to Interviewer mode (`references/interviewer-mode.md`): design a verification interview from the resume or its Resume Audit probe list, not answers for a candidate.
- Ask clarification only when missing information blocks the task; ask no more than 3 questions.
- Stop after 2 full QA revision loops unless the user requests more (a gate-only fast iteration does not count).

Default assumptions:

- Scenario: Chinese-market interviews (秋招/社招), covering 业务面/技术面的行为部分/HR 面. Algorithm and whiteboard coding are excluded.
- Deliverables: self-intro (30s / 1min / 3min), STAR story bank, high-frequency answers by 题型, 反问清单.
- Publish threshold per answer: QA score >= 85/100 and no fatal issues.

## Required Brief

```markdown
## Interview Brief

- Target role / company / round: (业务面 / HR 面 / 终面)
- Career-facts ledger: 已建（导入）/ 待建
- Resume version in play: (供交叉核对; 口径真相基准是账本非简历——简历与账本冲突则守账本; 无简历则跳过)
- Candidate's one-line selling point (draft): 
- Known weak spots (gap / 转行 / 短任期 etc.):
- Time to interview: (决定 Full Prep 还是考前速查)
```

## Workflow

### 1. Define — Selling-Point Gate

**Gate question: can the candidate state "为什么是我" in one job-relevant sentence?**

- Test: the sentence must name a差异化 claim an interviewer could probe (not "我学习能力强" but "我在 X 领域有从 0 到 1 把 Y 做到 Z 的完整闭环").
- Fail -> gate-only fast iteration: mine the ledger for candidate selling points, converge with the user. Do not draft answers before the gate passes.

### 2. Ledger

Import the ledger from loop-resume-writer if it exists; otherwise build it now (`references/career-facts-ledger.md`, same schema v1, same three source tags, same follow-up-question pattern).

### 3. Story Bank

Convert ledger entries into STAR stories (`references/star-story-bank-guide.md`).

- Each story maps to the question types it can answer (`references/question-taxonomy.md`); one story may serve multiple types — mark the reuse explicitly so the user does not repeat the same story twice in one interview.
- Every story must have a Result with real weight. A story without an R goes back to the follow-up question list, not into the bank.

### 4. Answer Draft

Draft in this order:

1. Self-intro in three lengths (30s / 1min / 3min), all built around the selling point.
2. High-frequency answers by 题型, drawing from the story bank.
3. 反问清单 (3-5 questions matched to the round: 业务面问业务, HR 面问机制).

Duration control: estimate spoken length from character count (`references/interview-rubric.md` for the rate and per-length budgets).

### 5. QA Loop

Grade each answer with `references/interview-rubric.md` (or `assets/answer-scorecard-template.md`).

Order of checks:

1. Ledger audit first: every fact and number traceable; resume consistency verified when a resume is in play.
2. Then score six dimensions (STAR completeness / specificity / role relevance / truthfulness & consistency / duration control / spoken register).
3. For every deduction, quote the offending sentence.

Fatal issues:

- fabricated experience or numbers
- answer deviates from the career-facts ledger (an answer that merely contradicts an *inflated* resume is not fatal — fix the resume instead)
- story without a result
- selling point mismatched to the target role
- bookish long-sentence answer beyond spoken memory load

Decision thresholds (per answer):

- Pass: >= 85 and no fatal issue.
- Borderline: 82-84 -> one targeted revision, re-check.
- Revise: < 82 or any fatal issue.

**Hard stop after 2 full QA revision loops per answer set.** Track the count (`QA loop: 1/2`, `2/2`). At `2/2`, Graceful Halt: output the best versions, list unresolved gaps (usually missing ledger data), hand control back.

### 6. Mock Pass

For each passed answer, run interviewer-style follow-ups 2-3 levels deep (`references/mock-followup-guide.md`): probe the numbers, isolate the personal contribution, push counterfactuals, challenge the result.

- A hole exposed by follow-up goes back into the QA loop as a concrete revision rule (or into the follow-up question list if it needs user data).
- Mock QA mode: when the user pastes their own answer, play the interviewer — follow up, then score.

### 7. Polish — Spoken Register

Final pass with `references/spoken-answer-guide.md`: sentence length, natural connectors, anchor-point structure (要点锚 + 展开线), removal of written-Chinese patterns that collapse under nerves.

### 8. Ship Check

```markdown
## Ship Check

- Selling point: (final one-liner)
- Story bank: N stories, 题型 coverage map
- Self-intro: 30s / 1min / 3min all passed QA
- Resume consistency: verified / no resume in play
- Open follow-up questions (missing data):
- Weakest answer & why:
- Suggested next step: (e.g. 考前速查 the night before)
```

## Output Modes

- Full Prep: Brief, Ledger, Story Bank, all deliverables, QA, Mock, Ship Check.
- Self-Intro Only: three lengths + QA.
- Single-Answer Drill: one question, draft -> QA -> mock -> polish.
- Mock QA: user pastes their answer -> interviewer follow-ups -> scorecard.
- 考前速查: compress passed materials into one page (`assets/cheatsheet-template.md`).
- Interviewer (hiring-side): design a verification interview from a candidate's resume or its Resume Audit probe list — question ladders + answer-reading guide, not candidate answers (`references/interviewer-mode.md`).

Infer the mode from the request. Default to Full Prep for candidate-side first-time preparation, Mock QA for pasted answers, and Interviewer when the user is a hiring-side interviewer designing questions.

## References And Templates

**Actually read these files; do not reconstruct them from memory.** They hold the ledger schema, calibrated anchors, and the follow-up pattern library.

Load only what the current step needs:

- `references/career-facts-ledger.md`: ledger build/import; source tags; follow-up questions.
- `references/star-story-bank-guide.md`: story construction and 题型 mapping.
- `references/question-taxonomy.md`: question-type skeletons and self-authored examples.
- `references/interview-rubric.md`: per-answer scoring; duration budgets.
- `references/mock-followup-guide.md`: follow-up patterns for the mock pass.
- `references/spoken-answer-guide.md`: spoken-register polish.
- `assets/interview-brief-template.md`, `assets/story-card-template.md`, `assets/answer-scorecard-template.md`, `assets/cheatsheet-template.md`: corresponding steps.
