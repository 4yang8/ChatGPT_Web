# Iteration 4 — 2026 추가 4편 심층 비교와 Research Gap 재판정

> 2026-10-29 · 대상: MASC / CALM-IT / PATIENTACT / AVP · **누적 2026 논문 12편** · 상태: 전문 검토 완료, 연구 가설은 미확정

## Executive finding

네 편의 본문을 검토하면 사용자 제안의 **'사전 궤적은 큰 틀이며 실제 변화 속도는 내담자의 성격/신뢰 성향·상담 개입에 좌우된다'**라는 아이디어를 구성하는 기능 자체가 상당 부분 이미 알려져 있다.

- **PATIENTACT**: 애착 유형에 따른 신뢰 상승/하락, 민감 기억 공개와 저항.
- **CALM-IT**: 상위 session goal과 매턴 goal, rapport·emotion·readiness의 변화, 성격/신뢰 성향 조건부 rapport delta.
- **AVP**: 상담사 skill에 따라 사전 목표 disclosure 단계의 **도달 시점이 변함**. skill-blind schedule 및 점진 공개 prompt 포함 replay baseline.
- **MASC**: initial/final 단계, receptivity 행동 prior, counselor의 핵심 동기/장벽 해결 조건에 따른 stage transition, fixed Schedule-ST baseline, 목표 상태·발화 수정.

따라서 단순히 **'hard trajectory 대신 개인차/개입에 따른 soft trajectory'**는 연구 문제로서 중요하지만 차별화된 새 방법론이라 확정하기 어렵다.

## 네 편의 구체적인 근거

### MASC (2610.08250v1)
38 MI 프로필; session-level Big Five와 profile, turn-level emotion/state/action. 타깃을 먼저 예측하고 3개 생성 에이전트 × 최대 5 debate step × 최대 3 attempts에 걸쳐 judge voting/retry. 고정 스케줄 baseline(턴1–10 Precont.,11–20 Cont.,21–30 Prep.) 포함. Hetero의 action/emotion/state KL **0.06/0.27/0.06** vs schedule **0.10/0.33/0.18**. 하지만 KL은 **실제 대화 레이블의 주변 분포**이고 intervention-conditioned counterfactual 타당성은 아님. 상태식은 전진/유지 중심; backward state transition 명시 없음. 이질 모델 대조는 backbone도 변화.

### CALM-IT (2601.10085v3)
686 맥락 × 4 frameworks × 3 lengths = **8,232 대화**. 신뢰 delta **[-0.10,+0.05]**, 사전 신뢰 성향/권위 선호/관계 경험에 따른 크기 조건화, 상담사 관찰 정보만으로 별도 추정. **Downstream은 무시할 수 없음**: CALM-IT 데이터 50%로 OLMo-3.1-32B를 학습하고 KMI 프레임워크에서 206 paired test, 9항목 중 Reflection **+0.502**, Change talk **+0.609**, Partnership **+0.900**, Realignment **+0.180**, Effectiveness **+0.238** 등 **5개 유의** 개선. 그러나 다른 synthetic generator로 동량 학습한 모델과 matched 비교 아님. 실제 환자의 latent dynamics 타당성은 직접 검증하지 않음. Redirect acceptance 64.28%와 positive change talk은 임상적 반응 정확성으로 자동 간주 불가.

### PATIENTACT (2608.12750v2)
5Ps/CCRT case formulation, trust-gated memory(1–4), 7 emotion reactions, 8 behaviors, 7 resistance patterns. 애착 스타일별 trust update(단위 ±0.25/0.5) — 사용자 아이디어의 성격별 속도와 **가장 직접 중복**. GPT-4o 공통 backbone, 40 clinical cases × 4 methods ×15 turns, 인간 평가 80 대화. Human disclosure **4.15**, resistance **3.82**, realism **4.15**로 3개 baseline보다 양호. 오히려 LLM judge와 인간 평가의 상관은 ρ **0.09~0.37**로 낮음. 단일 세션/고정 therapist, 실제 인간 환자 counterfactual 궤적, downstream learning은 검증하지 않음.

### AVP (2606.10051v2)
2000시간가량 실제 심리치료 기록의 SEM 기반 기술→개방 증분 관계를 근사. state D는 가중 점수 누적, G/M/H는 threshold. 기본적으로 상승형 진행(rollback 없음)이며 개인사별 다른 동역학 계수는 따로 학습하지 않음. 임상가13/학생7 명 × 4조건 = 80 sessions(1,033 turns). 텍스트-상태 일치 **0.651 vs 0.311**, 탐색 effect **β=+0.281**, 공감 effect **β=+0.004 p=.871**. Sam/ Alex 두 성격에 동일 계수; session-level skill group interaction p=.27 비유의. **Appendix D.1 5-arm offline replay**에 schedule, gradual prompt, state-in-context가 이미 있으므로 단순 비교 novelty 낮음. 더 강한 baseline은 live interaction이 아닌 recorded turns replay.

## 12편 기준 Track A/B 영향

| Track | 기존 후보 | 새로운 판단 |
|---|---|---|
| A1 | Intervention-contingent fidelity의 downstream transfer | **Narrow 유지**: CALM-IT도 32B를 SFT해 다른 framework에서 5/9 유의 개선. 단 fidelity-only matched synthetic dataset 학습 비교는 미확인 |
| B1 | 사전 거시 궤적 + 개인차·개입 조건부 속도 | **Strong overlap / 근본 재정의 필요**. 네 논문에서 이미 다양한 부분 구현. 단순 조합·3-way 비교만으로 불충분 |
| B2 | 장기 상태 일관성 | **Narrow**: MASC/CALM-IT/PATIENTACT가 안정·변동 상태를 분리. 다중 세션 장기 변화/임상 타당성에 집중하더라도 AnnaAgent/Psycheval 확인 |
| B3 | 내부 상태가 발화에 반영되도록 함 | **단독 방법론 Gap 약함**: MASC voting/retry, AVP P3, PATIENTACT reaction/behavior 있음 |

## B1의 남은 가설 (불확실)

**A high-level target trajectory should act as a revisable prior, not an obligatory schedule, while intervention-specific evidence and client characteristics determine the realized path, including legitimate stalls, regression, recovery, and failure to recover.**

이는 (a) final stage 무조건 달성, (b) 시간 경과만으로 호전, (c) 항상 상승하는 disclosure를 요구하는 시스템의 *가능한* 한계를 검증하자는 취지. **각 기존 방법이 실제로 이 조건에서 실패한다는 증거는 아직 없음.**

단순 '부정적 개입에서 후퇴 가능'도 PATIENTACT/CALM-IT이 이미 갖는다. 새로움은 **상위 목표와 잘못된 개입 사이의 우선순위를 어떻게 설명·제어·평가하는가**에 있는지 실증해야 한다.

### 가장 먼저 필요한 실패 진단
- **3개의 paired counselor intervention sequence**: 지속적 적절 개입 / 지속적 부적절 개입 / 초기 rupture→후속 repair.
- **개인차 조작**: 비슷한 맥락에서 trust inclination만 변경하고 delta timing/회복 여부 비교.
- **측정 분리**: (1) 전체 목표/주제 통제, (2) 개입에 대한 반응의 타당성, (3) 속도·지연, (4) 회복/실패의 정당성, (5) state-utterance fidelity, (6) OOD, (7) 사용자 행동 후속 적응.
- **경쟁 모델을 먼저 재현**: 해당 방법이 지원하는 심리 차원에서만 비교해야 공정. 내부 target 명중률만을 임상 타당성으로 착각하지 말 것.

## 새로 파악한 우선 2026 문헌 (PDF 미검토)
- **[Empirical Modeling of Therapist–Client Dynamics in Psychotherapy Using LLM-Based Assessments](https://arxiv.org/abs/2602.12450)**: AVP의 가중치/SEM 출처 논문; 임상 근거·개인차 검증 범위 평가.
- **[PatientHub](https://arxiv.org/abs/2602.11684)**: PATIENTACT가 실험 환경에 사용한 통일된 시뮬레이터 테스트베드.
- 기존 우선순위 **RESISTCLIENT**, **Psycheval**, **Evoke-Sim**도 유지.

## 최종 주의
연구 주제가 PUPPET/PATIENTACT/CALM-IT/MASC/AVP와 **겹쳐도 허용**되지만 **그들의 실제 한계를 개선하는 새 방법론적 기여**가 필요하다. 27B 및 Tool/Agentic/Memory/Planning은 선정한 문제를 해결하기 위한 잠정 수단으로 유지한다. '구성이 다르다' 또는 '강제 궤적이 싫다'만으로 ACL Novelty/Significance를 주장하지 않는다.
