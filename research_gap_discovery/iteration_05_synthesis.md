# Iteration 5 — RoleMemo/DualMem, MRPrompt, SAM, SAMem과 B4의 신규성 재검증

> 2026-10-30 · **누적 PDF 전문 검토 16편**: 심리상담/내담자 시뮬레이션 12 + 인접 일반 Role-Playing/Agent Memory 4. 2025 이후 일반 메모리 분야 전체의 exhaustive novelty review는 아님.

## 0. 사용자 아이디어
내담자가 n턴에서 자신의 발화를 생성할 때 **그 발화의 의도·속마음·판단 이유**를 가지고 있었고, 이 비공개 정보는 나중의 발화 생성에서도 **같은 사람의 과거 경험**으로 기억되어야 한다. 단순 공개 대화 history와 현재 심리 state만 기억하는 것과 차이가 있음.

예: "해볼게요"라는 동일한 공개 발화의 숨은 목적이 "진심" 또는 "갈등 회피"였을 때, 나중에 상담사가 실천 여부를 물으면 두 내담자는 다른 응답·태도 경로를 보일 수 있다.

## 1. 2026 신규 4편 비교

| 논문 | 핵심 mechanism | 주요 실험 | 정확한 범위/한계 | B4 위협 |
|---|---|---|---|---|
| **DualMem/RoleMemo** | facts + persona-grounded insights, linked memory, Qwen3-4B SFT/RL | Insight Recall@10 best baseline **0.41**, DualMem-RL **0.73**; roleplay **4.16** vs baseline max **4.01**, w/o insight **4.01** (Tables 2–3) | synthetic RoleMemo, long static histories+single query; **인물이 자기 발화를 생성하며 생각한 motive를 바로 기록한 것 아님** | **Very high** for interpreting and storing private meaning |
| **SAM** | raw agent thought/action/tool/observation pages + cue, intent-driven reconstruction, memory Qwen3.5-9B(+27B LoRA) | 4 benchmark avg GLM **54.6 summary→57.0 SAM**, Qwen35 **46.1→48.8** (Table1); raw/recent ablation | web/research agent; model's observable textual reasoning trace. **심리적으로 타당한 과거 자기 의도/발화 일관성은 미검증** | **Very high** for long-range thinking trace and recall |
| **SAMem** | state–thought–action–reward, state-specific Q-value, reusable thought memory | Table3 action-level **79.9%** vs thought-level **97.7%** (n9); Table1 GPT-4o ALFWorld **98.5%** vs baseline **62.7%** | decision-making in ALFWorld/ScienceWorld/Jericho, thought de-indexicalized; personal autobiographical memory 아닌 **generalized policy** | **High** for state–thought unit, lower for personal motive |
| **MDRP/MRPrompt** | static persona narrative schema with thinking_pattern; cue-based LTM retrieval; memory abilities MA/MS/MB/ME | Qwen3-8B **7.95 base→8.12 MRPrompt** (Table2), plus controlled facet ablations | **single-turn continuation** with fixed LTM; no within-session LTM update (§Limitations p.9) | **Medium** for scene-relevant thought pattern |

## 2. 이전 12편과의 직접 비교
- **CARS/STREAMS**: client-thinking, emotion, intention, response와 `{PLANNING}` (previous feelings and goals) 사용. 일반적인 cross-turn private state premise already used.
- **CALM-IT**: previous/current patient goals, rapport/emotion, appraisal of therapist turn. general present psychological state/goal memory already implemented.
- **PUPPET**: prior mental state+rule-based update; detailed episodic motive ledger is not described.
- **PATIENTACT**: client historical background, trust-gated memory, behavior reaction; episodic *own output rationale* is a separate construct.
- **MASC**: Retry Memory is same-turn failed response corrections, not autobiographical cross-turn memory.
- **DyCoP**: event-state sequence relates observations and psychological inferences from counselor/response side, not speaker's own contemporaneous motive.
- **PsyCLIENT/PsyChain/PsychePass/Graph2Counsel/TherapyGym**: no clear utterance-level private motive carryover in provided method/prompt, but this is **not** proof none exists in entire field.

## 3. Research Gap 판정
- **Reject** broad novelty claim: storing thoughts, facts+insights, state-thought pairs, retrieval of previous thoughts, or static thinking patterns.
- **Narrow & keep as working hypothesis**: preserving the **contemporaneous, utterance-linked** private intent of a simulated client distinct from public history/current state/retrospective interpretation, and measuring its **causal influence on long-horizon interaction consistency**.
- **No direct human mental ground truth:** simulated motive is a planted/committed in-model state and cannot be equated with real person's inner thoughts. Self-consistency and human plausibility need separate evaluation.
- **Critical risk:** remembering a wrong interpretation accurately may worsen the outcome (SAM wrong-frame case). Past belief is historical, while current belief is modifiable.

## 4. Why a weak comparison would not convince
Simply adding a memory string or supplying a hidden-intent label and seeing an improved answer is unsurprising (additional information advantage). Needed:
- Match language model/prompt/context lengths, action history, memory token budget.
- Compare public-only, current-state-only, DualMem-style **retrospective insight**, contemporaneous episodic intent, plus swapped/shuffled intent controls.
- Matched public utterances with two distinct planted private intentions, delayed test 2/10/30 turns, long-horizon consistency after subsequent evidence changes.
- Blind clinical/behavioral appropriateness evaluation beyond exact matching to internal gold; external baseline CARS & CALM-IT, plus roleplay memory retrieval.
- Show **why** contemporaneous provenance and utterance linkage matter, not merely whether storing 'more information' matters.

## 5. Operationally distinguish
`u_t`: spoken utterance.
`motive^{then}_t`: simulated intent **committed at original turn**.
`motive^{now}_k`: current attitude toward that old intent after subsequent events.
`posthoc\_inference_{k→t}`: a reconstruction at k from observed earlier `u_t`; may be uncertain.
`state_k`: present global psychological state.

The research contribution, if validated, is **temporal/provenance-sensitive memory of self-attributed utterance motives** and its test protocol, not invention of a generic thought-memory store.

## 6. Next literature checks
Per user's chronology **only 2026 new papers first**; the reviewed 2026 corpus is not exhaustive. The four uploaded papers reference important 2025 research **for later round**: PRIME (EMNLP 2025), Pre-storage Reasoning (arXiv:2509.10852), Thinking in Character (arXiv:2506.01748), CATCH (Findings EMNLP 2025), MOOM (arXiv:2509.11860), Reflective Memory Management for Long-Term Personalized Dialogue (ACL 2025). Their contents were **not** read in this round and novelty claims must wait.

Current top decision: **B4 a testable but not-yet-confirmed Methodological Gap**, perhaps connected to Track B1 soft-trajectory when past feigned acceptance or trauma-driven resistance changes the later path.

[Detailed B4 spec](track_b_utterance_grounded_intent_memory.md) · [Register](gap_register.md) · [Four analyses](validation_paper/README.md).
