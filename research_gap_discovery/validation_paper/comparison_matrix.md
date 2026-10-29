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
