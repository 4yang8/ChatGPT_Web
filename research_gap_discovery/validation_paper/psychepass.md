# PsychePass — Calibrating LLM Therapeutic Competence via Trajectory-Anchored Tournaments

**출처:** Findings of ACL 2026, pp. 40082–40098. **검토:** 2026-10-29. **지위:** 분석 초안.

## 1. 저자가 제기한 문제
- **Process drift:** 자유 내담자 시뮬레이션은 평가하려는 역량을 충분히 유발하지 못하고 표면적 공감으로 표류.
- **Standard drift:** pointwise judge 점수가 불안정하고 모델을 잘 구별하지 못함(Introduction §1).

## 2. 제안 방법
1. 100개의 client profile, SST 기반 **5개 상담 단계**와 **12개 counseling competency**에 맞춰 내담자가 특정 상황을 유발하도록 **Scripted Probing**.
2. Swiss-system pairwise battle + Bradley–Terry 기반 global Elo estimation (평가 점수의 calibration).
3. battle 평가로 Reward Model을 학습해 Qwen3-8B 상담 모델에 **on-policy GRPO** 적용.

내담자 모델은 **Doubao-1.5-Pro**. 여기서 'trajectory'는 상담사 역량 평가에 쓰이는 **미리 설계된 Interaction/Probing path**와 모델의 **battle ranking**이다. '자율적으로 변화하는 내담자 심리 전이'와 같은 개념이라고 간주하지 않는다.

## 3. 실험 / 수치
- 12개 상담 모델(프론티어 3종 포함); 각 모델 100명 내담자와 40~50턴, 총 약 4,400턴.
- **Table 2:** GPT-5.2 Elo **143**, Gemini 3 Pro **138**, Claude Opus 4.5 **134**; 전반적으로 프론티어 모델 우세. 단, 개별 공감 지표에서는 도메인 모델이 경쟁.
- **Table 3:** 30개 case, 2명 전문가와 비교. 강/약 모델 간 κ는 높지만 비슷한 상위 모델끼리는 약 **0.57**로 낮아졌다.
- **§4.5:** GRPO 뒤 직접 비교 승/패/무 비율 **59.5:26.0:14.5**; 12차원 중 9개 개선, **Crisis/Suggestion/Ethics는 퇴행**. Abstract의 다른 승/패 요약(62:25)과 범위/분모를 혼동하지 않는다.
- held-out 100개 profile로 학습 후 일반화 평가(§3.3); 독립적인 임상 효과 증명은 아니다.

## 4. 증거가 말하는 범위
**입증:** 특정 역량을 테스트하도록 스크립트를 지정하고, 비교 평가로 reward 신호를 만들어 작은 상담 모델의 성능을 개선하는 **evaluation→training loop**가 가능함.

**미검증 질문:** 환자 시뮬레이터 내부의 '심리 변화 충실도'가 동일 데이터량·주제 다양성 통제 아래 *그 자체로* Downstream 개선을 일으키는가? 이 논문의 설계 중심은 능력 유발 범위와 비교 평가 calibration이다.

**저자 인정 한계:** 내담자가 대화를 주로 이끄는 것은 실제 상담사 주도·협력적 상담 과정과 다를 수 있음. 실내 시뮬레이션과 인간 상담의 불일치가 남는다.

## 5. 우리 연구와의 중복/기회
- **중복:** 시뮬레이터에서 상담사 데이터를 수집하고 RM/RL로 상담 LLM의 성능을 올리는 파이프라인은 이미 존재.
- **후보:** scripted 능력 유발(coverage)과 **intervention-contingent client dynamics**의 차이가 downstream skill generalization에 무엇을 기여하는가? 이미 지정한 질문을 유발하는 것과 실제로 결과에 반응하는 내담자는 다른 학습 환경인가?
- **필수 통제:** profile/topic 수, 상담사의 시작 정책, 생성 데이터량/토큰/행동 다양성, 학습 베이스/예산, OOD evaluator.

## 6. 추가 조사
CARE-Bench, Ψ-Arena, ClientCAST, trajectory based RL 전이, hold-out leakage 및 LLM judge bias.
