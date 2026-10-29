# PATIENTACT — Theory-Grounded Mental Health Client Simulation (2026)

- **원본**: [arXiv:2608.12750v2](https://arxiv.org/pdf/2608.12750v2) · v2 2026-08-24 · 24쪽 · PDF: 2608.12750v2.pdf
- **상태**: PDF 방법·본문 표·프롬프트·부록 검토 (2026-10-29).
- **대상**: theory-grounded profile + trust-gated dynamic memory + reaction/behavior/resistance.

## 1. 저자의 문제 정의
기존 가상 내담자는 지나치게 순응·조기 자기개방·빠른 문제 해결을 보인다. 프로필의 심리적 원인 관계가 얕고, 모든 기억에 동일 접근 권한을 주어 맥락에 맞는 disclosure 제한이 어렵다.

## 2. Method (§3, Fig.1)
- **Profile**: 5Ps(Presenting, Predisposing, Precipitating, Perpetuating, Protective factors), CBT 자동 사고/중간 신념, CCRT interpersonal patterns, core belief theme, attachment style(anxious/avoidant/disorganized). 원인 관계와 대인 관계 패턴.
- **Static memory vs dynamic memory**: profile을 나누고 민감한 항목마다 disclosure threshold, relevance tags, discomfort flag 부여. 현재 trust 이상일 때만 해당 내용 검색·공개.
- **Turn pipeline**: 관련/차단 기억 검색 → 상담사 개입에 따른 **반응(7종, 3 intensity)** → 행동(8종, 저항 시 3 차원 7종 패턴) 선택 → response signal → 발화 생성 → **상담사 개입에 따른 trust 갱신**.
- **Trust:** 1.0–4.0, initial **2.5**, 갱신 단위 ±0.25 또는 ±0.5. positive gain은 빈번하지만 작고 negative loss는 드물어도 크게 설정.
- **개인차 조건**: anxious는 거절 감지에 신뢰 하락, avoidant는 신뢰 상승이 느리고 압박에 취약, disorganized는 긍정적 개입 뒤에도 하락 가능. **이것이 사용자가 제안한 성격/신뢰 성향에 따른 변화 속도 차이와 직접 중복**.
- **사전 목표 궤적**: turns별 예상 신뢰값/후반 회복 목표를 미리 지정해 따라가도록 제어하지 않음; 매 턴 개입과 profile이 갱신을 결정. 그러나 *부드러운 궤적 제어라는 메커니즘의 독립적 가치*는 별도 검증 필요.

## 3. 실험·결과
- 전문가 지원으로 만든 depression/anxiety **40 clinical situations**; profile 생성 GPT-5.4. 평가용 시뮬레이션은 모두 **GPT-4o** backbone, T=0.7.
- baseline Patient-Ψ, AnnaAgent, ConsistentMI. 상담사는 4방법에 공통인 훈련용 prompt; 15턴 × 40 시나리오 × 4방법 = **160 conversations**.
- 사람 평가는 이 중 80 conversations, 심리학 배경 평가자 10명 중 대화마다 3명, blinded. 모든 160대화 GPT-5.4 judge 평가.
- **Table 1, profile**: Clinical Plausibility **4.43/5**, Internal Consistency **4.38**, Specificity **4.32**, Depth **4.28**; Krippendorff α 약 **0.41–0.45**.
- **Table 2 (사람 점수)**: Full Coherence **4.37**(Patient-Ψ 3.98), Disclosure **4.15**(Patient-Ψ 3.65), Resistance **3.82**(ConsistentMI 3.15), Emotion **4.15**(Patient-Ψ 3.83), Realism **4.15**(Patient-Ψ 3.52). 우위에 p<0.05 표기.
- **Table 4, 사람 점수 ablation**: Full Disclosure **4.15**, w/o Trust Gating **3.22**, w/o Dynamic Memory **3.40**, w/o Reaction/Behavior Pipeline **3.75**. Full Resistance **3.82**, no-gating **2.88**, no-memory **2.78**, no-pipeline **2.98**.
- **자동 judge가 실제로 다른 결론**: Full의 Coherence/Disclosure LLM 4.35/4.42 vs Patient-Ψ 4.40/4.60; 오히려 no-pipeline이 Full보다 자동 점수에서 좋기도 함. Table 3의 human–LLM Spearman ρ **0.09(Coherence), 0.29(Disclosure), 0.22(Resistance), 0.37(Emotion), 0.33(Realism)**. 임상 fidelity는 자동 judge 하나로 평가 불충분.

## 4. 비판적 범위
**Observed:** 실제로 의미 있는 내담자 프로필·내용별 공개 제어·여러 저항 형태를 결합하면 15턴 가상 상담에서 **사람 평가** 현실성이 개선됨. 큰 프로필/검색 제어가 단순 동적 반응 모듈만큼 또는 그 이상 중요한 요인이라는 실험.

**Not established:** 프로필별 신뢰 갱신 속도가 실제 인간의 **counterfactual trust dynamics**와 정량적으로 일치한다는 근거, 같은 상담사 개입만 바꾼 matched counterfactual 비교, 장기/다중 세션, 상담 LLM 학습 전이. 실제 내담자 데이터를 사용하지 않고 전문가 제작 시나리오+모델 프로필을 사용.

**Bias warning:** secure attachment는 의도적으로 sampling에서 제외(App. A). 특정 attachment category에 고정 반응 규칙을 부여하면 심리 개인차가 과도하게 단순화될 위험. 성격별 속도의 독립 효과와 민감도 ablation이 별도로 강하게 검증된 것은 아님.

## 5. 우리 연구에 대한 영향
- **B1 직접 충돌:** '성격 차이가 궤적 속도를 좌우한다' 자체는 이미 구현. **완전 신규라는 주장 불가**.
- **B2/B3 직접 충돌:** 동적 trust, memory disclosure gates, reaction→behavior→utterance가 구현됨. 단순 구현만으로 기여 불가.
- **Track A:** 시뮬레이터 개선 지표와 상담 학습 간의 전이는 이 논문의 저자도 future work로 남겨둠.
- **남는 후보:** 목표 궤적과 상호작용 조건의 **우선순위를 어떻게 조율하는가**, 장기 실패/회복, multiple personas를 통한 이질성 실제 검증 등. 단, 다른 연구(CALM-IT/MASC/AVP)와의 합산 중복 검증 필수.

## 6. 후속 실험 우선
같은 5Ps profile과 신뢰 초기조건에서 상담사 개입만 바꿔 trust/dynamic memory 변화 검증; attachment 전이를 수치·세션별로 비교; 속도·후퇴·회복 능력 평가; 단순 스케줄·상위 목표 도입이 오히려 개입 반응을 덮는지 측정.
