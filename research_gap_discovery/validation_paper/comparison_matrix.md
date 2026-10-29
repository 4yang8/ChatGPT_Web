# Cross-paper comparison — 초기 4편

## 논문 간 방법·증거 범위

| 차원 | PUPPET | PsychePass | DyCoP | PsyCLIENT |
|---|---|---|---|---|
| 주된 대상 | 가상 내담자/표준화 환자 | 상담사 평가·RL | 상담/정서지원 **응답 에이전트** | 가상 내담자 |
| 방법의 핵심 | LLM Observe → Probabilistic Logic/State Machine → LLM Behave | SST 5단계 scripted probing → Swiss/BT-Elo → RM·RL | 이벤트-심리상태 시퀀스 + 검색·계획·생성 | 프로필 + 사전 행동 레이블/참고 콘텐츠 궤적 |
| 주 제어 방식 | 전문가 정의 심리 전이 규칙 | 턴별 평가 목표·스크립트 | 대화 목표와 계획 | 행동/참고 발화 제약 |
| 실제 동적 상태 | 규칙 기반 상태 전이 | 대화 단계 제어(개별 환자 심리전이와 별개) | 상대 사용자 상태 **추정 및 계획** | 원래의 행동 궤적은 사전 지정, 문맥 충돌 시 완화 |
| 주된 실험 | 상태 예측·행동 충실성·교육 자기효능감 | 모델 순위·전문가 일치·RL 개선 | 7B 모델 성능·요소별 ablation | 상담사 지각 자연스러움·유용성·출처 구분 |
| 상담 모델을 실제 학습? | 상담 인력 교육 평가; LLM 상담사 학습이 주목적 아님 | **예**, RM + Qwen3-8B GRPO | 대화 응답 모델 설계 평가 (전형적 offline synthetic-to-SFT 실험 아님) | **아니오**, 지각된 훈련 유용성 |
| 우리에게 주는 강한 반론 | 개입-상태 반응 이미 명시적으로 제안 | 시뮬레이션 기반 학습은 이미 수행 | 작은 모델 + 기억/계획 도입은 신규성이 약함 | 다양한 행동/전개 및 높은 자연스러움 이미 연구 |
| 추가로 확인할 질문 | 규칙 외 상황에서의 일반화, 치료적 타당성 | 학습 효과 원인이 시뮬레이터 특성인지 | planning의 독립효과·비용 | 사전 행동 궤적 vs 개입 종속 상태의 학습 효과 |

**주의:** 서로 다른 논문에서 나온 수치들을 동일 benchmark의 우열처럼 직접 비교하지 않는다. 실험 데이터/모델/지표/평가자/언어가 다르다.

## Research Gap 중복 검증
- '심리 상태 전이 내담자' 자체 → PUPPET과 강하게 겹친다.
- '시뮬레이터 활용 상담사 RL' → PsychePass와 강하게 겹친다.
- '작은 모델의 planner+memory+retrieval' → DyCoP이 선행했다(단 주 역할은 상담사).
- '다양한 내담자 발화 행동 궤적' → PsyCLIENT과 겹친다.
- **잠정 확인 과제:** 어떤 client-fidelity 성분이 downstream 상담 모델 학습에 **독립적으로** 도움이 되는지, 충분히 통제한 실험이 이미 있는가? 이것은 *검증 전 후보*다.


---
## Iteration 2 — 신규 4편 비교 (2026-10-29)

| 논문 | 동적 내담자 여부 | Downstream 상담사 학습 | 기존 주장과 겹치는 지점 | 경계 |
|---|---|---|---|---|
| CARS/STREAMS | CCD trigger에 조건부 저항·감정·의도 변경 | Thinker SFT/PPO, Presenter SFT/GRPO | dynamic resistance + 전략 RL | 저항=CARS, 협조=Patient-Ψ라 simulator 자체 차이 존재; state fidelity만 matched training으로 분리하지 않음 |
| Graph2Counsel | CPG는 정적 관계 스냅샷; temporal update 없음 | Llama3-8B QLoRA SFT | 심리구조 기반 합성→학습 | CoT/MA 비용 증가 효과 미약; dataset size/출처 차이 |
| TherapyGym | 기존 Patient-Ψ 기반 역할; 상태전이 자체 주 실험 아님 | Qwen3-4B,1.7B GRPO | 시뮬레이션+임상 척도 reward→상담 정책 개선 | JudgeBench o3-mini, RL rollout Gemma3-4B; fidelity 독립효과 미분리 |
| PsyChain | stage+personality; 감정 긍정 개선 명시적 사전 지시 | Qwen3-8B SFT | 상담 단계·다중 에이전트+합성→학습 | scale-matched Combined 있으나 변수가 다수 동시에 변경; 긍정 변화 편향 |

### 논문별 수치 해석 주의
- CARS/STREAMS: 인간 평가는 CBT 전문가 4명이 저항 대응을 평가; STREAMS-RG 전략 적절성 4.31, 진행 4.05 (1–5).
- Graph2Counsel: 전문 평가의 rank는 **작을수록 우수**. Therapy-Eval G2C 3.81 vs SQP 3.80로 작은 차이.
- TherapyGym: 0.10→0.60 CTRS는 0–1 정규화 점수; 안전 평균 0.38→0.20.
- PsyChain: 61–91% win rate는 프론티어 상대 승률이 아니라 domain-specific baseline 상대. 동일규모 Combined 비교는 데이터 양 혼동 일부 방지하지만 동적 반응 자체를 분리하지는 못함.

### 전체 8편이 만드는 중복/반증
- **연구 범위 재분류:** 27B 및 Tool/Agentic은 독립 Contribution/Gap이 아니라, 확인된 Gap의 연구 가설을 구현하는 **방법 선택지(M1)**이다. DyCoP, PsyChain, CARS/STREAMS는 구현 방식을 선택할 때 참고할 선행 사례.
- 넓은 주장 'intervention→mental state→response'는 PUPPET, CARS와 중복.
- 넓은 주장 '시뮬레이터 생성 합성 데이터로 상담 모델 성능 향상'은 PsychePass, Graph2Counsel, TherapyGym, PsyChain, CARS와 중복.
- Graph2Counsel의 costly CoT/MA 실패는 planning 필요성을 자동으로 전제하지 말라는 근거.
- **남아 있을 수 있는 가설:** 동일 학습 예산/데이터 분포 조건에서, 내담자 상태전이의 **상담 개입 종속성**만 달리했을 때 downstream learning과 OOD generalization 차이가 발생하는가? 아직 입증된 연구 공백이라 단정하지 않는다.

다음 목표: [gap_register.md](../gap_register.md)의 유지/수정/폐기 상태를 기준으로 2026 RESISTCLIENT 및 Psycheval을 반증 문헌으로 검토.


---

## Track B: 궤적의 사전 정의와 실제 개입 반응 간 긴장 (2026-10-29 추가)

아래는 **이미 PDF를 검토한 논문들에서 확인한 메커니즘**이며, 동일한 benchmark에서의 정량 우열을 뜻하지 않는다.

| 논문 | 상위 목표/궤적의 제어 | 상담 개입 조건부 조정 | Soft Trajectory 질문과의 연결 |
|---|---|---|---|
| PsyCLIENT (§4.3) | 행동 레이블·참고 내용으로 사전 궤적 제공 | 상담사 발화와 목표 행동이 모순되면 일관성을 우선하는 완화 규칙 | 미리 정한 행동 순서·시간을 얼마나 재조정하는지 확인 |
| PsyChain (Client Speaker prompt, 부록 A.3) | 상담 단계 및 점진적 긍정 변화·최종 합의 지시 | 프로필/단계/대화에 조건화 | 예정된 호전이 실제 개입의 효과를 가릴 위험(가설) |
| PUPPET (§3) | 전문가 사전 정의 확률 규칙·상태 머신 | 상담 개입 → 규칙 추론 → 내담자 상태/발화 | 규칙 밖 개입과 개인별 속도차에서 통제성·일반화 비교 필요 |
| CARS/STREAMS (§3.1) | CCD 기반 개인별 저항 조건, 감정/목표 상태 계산 | 개입이 믿음에 부합·불일치할 때 저항·감정 변화 | 이미 조건부 저항이 있으므로 Soft Trajectory의 추가 가치 검증 필요 |
| Graph2Counsel (§7) | 고정 CPG(세션 중 변화 없는 스냅샷) | 생성 문맥 반응은 가능, 매턴 temporal CPG 갱신은 없음 | 반복 변화·장기 궤적은 논문이 향후 과제로 명시 |

**아직 전문 검증 전:** PatientAct(애착/신뢰 업데이트 관련 잠정 유사성), AVP(행동-자기개방 적응 관련 잠정 유사성), Evoke-Sim(단계·정보 공개). 상세 [검증 목록](trajectory_related_2026_to_validate.md). "해당 논문과 완전히 중복"이라고 판단하지 않는다.


---

## Iteration 4 — 신규 4편 직접 비교: MASC / CALM-IT / PATIENTACT / AVP (2026-10-29)

| 비교 차원 | MASC | CALM-IT | PATIENTACT | AVP |
|---|---|---|---|---|
| 본질 | MI 내담자 상태·발화 **self-calibration** + benchmark | 내담자–상담사의 양측 누적 conversational dynamics | 5Ps/애착 기반 dynamic memory·trust gating·저항 | 실제 상담 기록 SEM에서 추정한 skill→disclosure 속도 관계 |
| 상위 궤적/목표 | 프로필 initial/final stage, 동기·신념 장벽; 3단계 | overarching session goal + 매턴 current goal | 정해진 목표 trust curve 없음; 개인적 trigger·기억 공개에 따른 흐름 | 목표 **G→M→H**의 누적 상승; threshold로 타이밍 제어 |
| 개인차에 따른 속도 | receptivity prior가 action 확률 변경; Big Five는 session constraint | rapport delta가 배경·신뢰 성향·trauma/history에 조건화 | **애착 스타일별 trust gain/loss**를 명시적으로 변경 | Sam/Alex 반응 차이는 있지만 **공통 update weights**(속도 계수 별도 적합 X) |
| Intervention responsiveness | counselor가 motivation·belief barrier를 다룬 경우 stage 상승 | 상담사 반응 appraise 후 trust/readiness/emotion/goal 갱신 | reaction/behavior/trust update, 신뢰별 memory disclosure | empathy/exploration에 따라 누적 disclosure state 상승 |
| 나쁜 개입 후 후퇴/회복 | 명시 stage rollback 없음 (Eq.3) | Δrapport 음수 허용, 변화 상태·목표 갱신 | trust 감소·증가 모두 가능 | 누적 점수의 기대값은 비감소; 명시적 신뢰 붕괴→회복 없음 |
| Fixed schedule와 비교? | **Schedule-ST** baseline(각 단계 10턴 고정) | 단계·시스템 자체 비교/ablation, 정적 macro-trajectory 1:1 비교는 아님 | 3종 simulator baseline, 고정스케줄 명시 비교 없음 | **SCHEDULE+SVP-GRADUAL+STATE-IN-CONTEXT** 포함한 5-arm offline replay |
| 주 평가 | real MI의 EE/PS/CA **marginal label KL**, profile, personality/receptivity | MITI, trajectory-independent length robustness, licensed psychologists, redirection | 심리학 배경 평가자 10명(대화별 3인), realistic resistance/disclosure | clinicians+students 20명, 80회 상담, 실대화 기반 SEM, disclosure coupling |
| 상담 LLM을 데이터로 학습? | **아니오** | **예**, OLMo-3.1-32B fine-tune, KMI에서 9개 중 5개 지표 유의 개선 | **아니오** | **아니오** |
| 주요 수치 | Hetero action/emotion/state KL **0.06/0.27/0.06** | CI Effectiveness **4.45**, CI-NC **3.67**; SFT Reflection **+0.502**, Partnership **+0.900** | Human disclosure **4.15**, resistance **3.82**, realism **4.15** | Text-state match **0.651** vs static **0.311**, exploration effect **+0.281** |
| 해석상 주의 | 예측 target label 일치 ≠ 상담사 개입 반응의 인간적 인과 타당성; Hetero model confound | 두 에이전트 효과를 같이 측정; 8,232개 합성 vs 실제 상담 상태 전이 외부 검증 없음 | 15턴·40 시나리오, attachment stereotype 가능, downstream 미검증 | 두 persona, monotonic disclosure, 상호작용 session-level skill sensitivity 비유의 |

### 강한 중복과 남은 질문
- **B1 기본 조합(성격/신뢰 성향과 개입에 따른 속도)** → PATIENTACT, CALM-IT, AVP에 **이미 직접 존재**.
- **B1 (고정 스케줄과 조건부 전이 비교)** → MASC의 Schedule-ST와 AVP의 offline 5-arm replay에 **이미 부분 실험 존재**.
- **B3(내부 상태와 발화가 일치해야 한다)** → MASC construct-level judge+retry, AVP P3 측정, PATIENTACT state/action signal로 강한 중복.
- **A1(더 풍부한 상호작용 시뮬레이터로 학습)** → CALM-IT이 OLMo-3.1-32B SFT 후 KMI 내 학습 전이도 보고. 단 **matched-training data generator ablation**은 하지 않았으므로 attribution 문제는 남음.
- **잠정 Narrowed B1:** 정해진 고수준 궤적은 **hard required state가 아니라 선호 또는 허용 가능한 범위**로만 두고, 상담사의 개입이 이를 위반하는 경우 **정당한 stall/rollback/repair**를 허용하며, 이 균형을 여러 profile에서 *같은 조건의* counterfactual로 평가할 수 있는가? CALM-IT/PATIENTACT/MASC/AVP 대비 실제 실패가 있는지 확인 전에는 Gap 확정 금지.

상세: [Iteration 4 종합](../iteration_04_synthesis.md) · [논문별 개별 분석](README.md).


---

## Iteration 5 — 일반 Role-Playing/Agent Memory 4편과 B4 (2026-10-30)

핵심 비교 질문: **발화 \(u_t\)를 생성하던 시점의 \(z_t\)(내담자의 비공개 의도·해석)를 그 발화에 묶어 다음 턴에서도 활용하는가?** 아래 '부재'는 PDF 공개 구현의 한계를 뜻하며 다른 문헌 전체의 부재를 뜻하지 않는다.

| Method | 기억에 들어가는 내용 | 생성 시점·저장 의미 | 나중에 사용하는 방식 | B4와 충돌 수준 |
|---|---|---|---|---|
| DualMem | observed facts \(F_i\) + persona-conditioned interpretation \(I_i\) | 대화 chunk 처리 시 \(I_i=f(F_i,p,M_{<i})\) 합성, fact↔insight 연결 | query에 fact+insight retrieval | **매우 높음**: 주관적 해석의 지속적 기억 이미 존재. 하지만 '말하기 직전 스스로 선택한 의도' 아님 |
| SAM | past reasoning/tool calls/observations as raw trajectory pages + cues | agent trajectory 실행 중 기록한 text trace를 page로 보존; cue는 pointer | current intent 기반 원본 재구성·검색 | **높음**: 과거 사고 원본 보존과 goal-based recall은 이미 연구. 상담 발화의 역사적 자기 의도와 구분 |
| SAMem | state \(s_t\), thought \(th_t\), action, reward, Q(s,th) | 의사결정 당시 state–thought–action을 수집한 후 추상화/cluster; thought에서 구체 개체 제거 | 유사 state에서 **높은 Q의 reusable thought** 조회 | **높음**: state–thought pair 신규성 없음. episodic autobiographical motive와는 다름 |
| MRPrompt | static persona facets: emotional_state, behavior_pattern, thinking_pattern, cue, time scope | 과거 작품/성격에서 사전 구성한 LTM; 한 세션 중 새 intent를 쓰지 않음 | dialogue STM cue로 facet 검색/적용 | **중간**: thinking_pattern retrieval 선행. contemporaneous utterance motive 기록은 범위 밖 |

### 이전 상담 분야 논문들과 합산
- **CARS**(Appendix A.2): Thinking, Emotion, Intention, Response와 **previous feelings/goals `{PLANNING}`**을 다음 입력에 제공. 발화시 의도 유지의 직접적 전례. 단 특정 과거 발화와 그 당시의 내적 의도를 **장기 사건기억**으로 연결한 평가 근거는 미확인.
- **CALM-IT**: 현재 목표, 직전 상담사 평가, rapport/emotion/goal을 다음 턴으로 전달. **당시의 특정 과거 발화 motive**의 별도 episodic ledger는 명시되지 않음.
- **PUPPET**: current mental state를 확률 state machine으로 업데이트하고 condition generation. prior belief/rule trigger의 전 경로 기억이라는 별도 구조는 미확인.
- **MASC**: Retry Memory는 **같은 턴 재생성**용이며 cross-turn autobiographical memory 아님.
- **MRPrompt**: `thinking_pattern`은 **정적 역할 특성**이므로 기존 "내면이 없었다" 주장에 대한 반례지만 실시간 개인 내면 기억의 직접 구현은 아님.

### 과학적으로 주장 가능한 범위
**Reject:** "LLM은 과거 발화 외에는 아무것도 기억하지 않는다"; "생각을 저장하는 메모리를 처음 고안"; "facts+insights를 처음 분리".

**Open/narrowed B4:** 현재 state를 유지하는 것과 다르게, **특정 발화 시점에 확정된 비공개 자기 의도**를 발화별로 연결·보존하고, 동일 공개 대화에서 서로 다른 당시 의도를 안정적으로 구별하여 후속 개입에 반응하는가? 이미 그 시점의 기록을 만드는 것이 단순한 post hoc reflection보다 실제 장기 정합성을 높이는가?

**Major validity issue:** 시뮬레이터가 생성한 과거 의도는 인간의 검증된 심리 원인이 아닌 **simulation-internal latent event**다. self-consistency와 human psychological plausibility를 분리 평가. **기억의 정확성**과 **원래 기록된 해석의 진실성**은 다름(SAM Appendix E.2의 wrong frame failure).

전체 판정 및 실험: [iteration_05_synthesis.md](../iteration_05_synthesis.md) · [B4 상세](../track_b_utterance_grounded_intent_memory.md).
