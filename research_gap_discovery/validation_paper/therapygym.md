# TherapyGym — Evaluating and Aligning Clinical Fidelity and Safety in Therapy Chatbots (2026)

- **원문:** [arXiv:2603.18008v1](https://arxiv.org/pdf/2603.18008)
- **업로드 PDF:** `2603.18008v1.pdf`, 27쪽
- **검토 상태:** Full-text initial review (2026-10-29)

## 1. Problem
상담 LLM의 유창함/공감/일반 선호 지표만으로는 **CBT 임상 기술의 충실도(clinical fidelity)**와 **안전 위반**을 다차원적으로 평가하고 학습할 수 없다는 문제.

## 2. Method
1. **TherapyJudgeBench:** 116개 simulated dialogue, 전문가 **1,270개** 평정(2명의 licensed CBT practitioners); 11개 CTRS 기술(원점수 0–6)과 4개 안전 위반.
2. **TherapyJudge:** LLM judge를 임상 평정과 대조해 검증/보정. 최대 평균 **Spearman ρ≈0.56**(9개 신뢰도 확보 기술); 2개 low-reliability 기술은 보상에서 제외.
3. **TherapyGym:** CBT 역량의 **정규화 점수(0–1)**에 안전 위반 penalty를 더한 대화 단위 보상으로 **GRPO online RL**.
4. **환자 역할:** JudgeBench 구성시 Patient-Ψ-CM 기반 **o3-mini** 시뮬레이터; **RL 학습 rollout에서는 Gemma3-4B**가 환자 역할(본문 §2.1, §3.2). 시뮬레이터 자체는 이 논문의 새 핵심 방법이 아님.
5. **상담사:** Qwen3-4B(-Instruct), 추가 Qwen3-1.7B. 환자 프로필 106 → **13,093** (TreeSynth 증강), 검증용 20개. 10턴(상담사5/환자5) 대화.

## 3. 주요 결과
- Table 4, Qwen3-4B: **Human CTRS normalized 0.10 → 0.60**, **Human safety violation average 0.38 → 0.20**. LLM judge CTRS **0.16 → 0.59**, safety **0.38 → 0.13**.
- CTRS의 0.10/0.60은 임상 원척도 0–6점이 아니라 **0–1로 정규화된 지표**이므로 혼동 금지.
- Appendix B.5 안전 패널티 제거: LLM judge 평균 CBT **0.53 vs 0.59**(Full), safety violation **0.43 vs 0.13**(Full). 안전 관련 reward의 독립적 기여를 시사.
- 판정자 정합성: 평균 α **0.52**, 범위 **0.23–0.72**(11개 CTRS), 약한 항목 제외 후 평가/보상 설계(본문 §2.1, §3.3).
- Appendix B.6 환자 프로필 일치 40/40 판별, 단 이는 프로필 **식별 가능성** 검사이지 intervention-conditioned dynamics의 심리적 타당성 검증은 아님.

## 4. 입증 범위 / 미입증
**Observed:** 임상적으로 정의된 항목을 reward로 사용해 작은(1.7B/4B) 상담 모델의 기술 표현을 개선하고 안전 penalty로 부작용을 낮출 수 있다. 실제 전문가 평정과 LLM judge 사이의 일정 수준 순위 상관을 검증.

**미분리:** 내담자 시뮬레이터의 *동적 심리적 정합성*만 조작한 대조 학습. 주요 연구 대상은 **평가+보상 정렬**이고, Patient-Ψ 기반 환자 시뮬레이터 자체의 대안 비교는 중심이 아님.

**한계:** CBT/10턴 위주, 평가 모델 신뢰도 한계, 장기적 실제 임상 개선 결과 없음. 보상 정책이 잘못된 judge signal을 학습할 위험에 대비해 독립적 판정·OOD 평가는 중요. 13,093개의 확대된 환자 프로필의 임상 일반화도 별도 문제.

## 5. 우리 gap과 관계
- **G1(단순 simulator→RL)**: 이미 수행; 재료가 좋아져서 상담 모델이 개선됐다는 넓은 주장은 Novelty 부족.
- **평가 설계:** CTRS 및 safety 결과와 전문가·judge 일치까지 함께 보고할 필요. 평가하기 어려운 항목은 평가 척도 신뢰도부터 검증.
- **G3(27B 중요성):** 4B 상담 모델도 전문 reward 신호를 통해 큰 개선; 규모 경쟁만으로 부족.
- **수정 후보:** 내담자 시뮬레이터의 어떤 상호작용 특성이 **동일 보상/동일 학습 예산 하에서** 우수한 상담사 정책 일반화에 기여하는가?

## 6. 추후 검증
PsychePass와 RL/Reward 비교, Patient-Ψ 원문(2024, 추후 선행연구 그룹), 2026 임상 judge reliability 관련 논문. OOD client generalization과 real-world efficacy 혼동 금지.
