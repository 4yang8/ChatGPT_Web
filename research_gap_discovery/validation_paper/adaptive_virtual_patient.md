# Adaptive Virtual Patient (AVP) — Empirically Grounded Behavioral Adaptivity (2026)

- **원본**: [arXiv:2606.10051v2](https://arxiv.org/pdf/2606.10051v2) · v2 2026-10-22 · 22쪽 · PDF: 2606.10051v2.pdf
- **상태**: PDF 전면, Appendix D.1 5-arm replay, weight ablation, human study 검토 (2026-10-29).
- **대상**: 상담사 실기(empathy/exploration)에 조건부로 진행하는 **자기개방(disclosure)** 한 차원의 누적 궤적.

## 1. 문제/핵심 공헌
고정 스크립트는 실제 훈련생의 행동을 반영하기 어렵고, 프롬프트만 사용하는 LLM은 변화가 통제되지 않는다. AVP는 **실제 상담 기록 약 2,000시간으로 추정한 구조방정식(SEM)**의 관계를 참고해, 상담사 기술 점수가 쌓일 때 자기개방이 **서서히 진행되도록 하는 명시적 transition**을 구현.

네 가지 평가 속성: P1 **Responsiveness**, P2 **Gradual Build-up**, P3 **Faithful Realization**(내면 상태↔발화 일치), P4 **Skill Sensitivity**.

## 2. Method (§3, Fig.1, Appendix A/B)
- LLM detector(gpt-4o-mini): 상담사 Interpretation, Emotional Reaction, Reflection, Exploration 각각 0~2점.
- **증분식**: Δ_t = c + w_i i_t + w_er er_t + w_r r_t + w_ex x_t + ε_t. 실제 운영 가중치는 **(1,1,1,3)**, c=0.15, noise σ=0.10, global multiplier m=0.20. 누적 D_t = m ΣΔ_t. **누적형·기댓값 단조 상승**, 감소/역방향 관계 파괴를 별도 모델링하지 않음.
- Threshold: D <4.5 = Guarded, 4.5–<10 = Medium, ≥10 = High. 상태에 해당하는 **자기개방 가능한 기억만** prompt/retrieval에 노출.
- 환자 프로필 2개: Sam(guarded), Alex(expressive). **서로 다른 m/계수의 성격 전이 모델을 학습하지 않고** 같은 기본 업데이트를 사용; persona 차이로 관찰된 속도 차이는 있으나 독립적인 속도 기제와 구분해야 함.
- 데이터로 직접 학습한 인과 계수가 아니라 SEM 추정 비율을 맞춘 **정수 근사 가중치**. 사건별 효과·개인차/negative rupture의 causal validity는 아직 미검증.

## 3. 실험
- **N=20**: Licensed clinician 13, clinical students 7. 각 참여자가 두 캐릭터의 Static/Adaptive 네 조건을 모두 경험: **80 sessions, 1,033 turns**.
- Main baseline: same gpt-4o-mini + same persona의 static prompt only. Static은 심층 내용을 시작부터 노출하므로 **원래 비교는 static에 불리할 수 있음**.
- **Appendix D.1 더 강한 baseline replay**: 동일 logged trainee turns를 5 arms로 오프라인 재생: AVP / SVP-MATCHED / SVP-GRADUAL / STATE-IN-CONTEXT / skill-blind **SCHEDULE**. **Hard schedule vs adaptive comparison 자체가 이미 존재**. 다만 재생은 feedback이 달라졌을 때 훈련생의 다음 선택이 변하는 interactive 비교가 아님.

## 4. 정량 결과 및 정확한 해석
- **Table 2 P1**, AVP turn-level Δdisclosure regression: Exploration **β=+0.281, p<.001**, Empathy total **β=+0.004, p=.871**(유의하지 않음). R² 0.041로 설명력은 낮음; 실제 관찰된 효과가 empathy까지 확인됐다고 주장하면 안 됨.
- **P2**, Appendix Table 5: Full session G/M/H evaluator-coded mean **1.03→1.75 AVP** vs **1.72→1.68 SVP**. 0–80%에서는 AVP **1.03→2.00** vs SVP 1.72→2.28, fitted R² 각각 .94 vs .15. 높은 설명력은 **시간에 따른 평균 증가의 선형성**이지 인간 심리 법칙 검증이 아님.
- **P3**, Fig.3: internal disclosure target과 자동 evaluator-coded text agreement **0.651 AVP vs 0.311 SVP**. 여기서 'ground truth'는 **독립 LLM detector가 붙인 label**이고 실제 환자 내면 상태 아님. H level precision **0.326**으로 고단계 정밀도는 낮음.
- **P4**, high/low trainee skill 그룹: session-level change **0.85 vs 0.60**, 상호작용은 **p=.27 (비유의)**. 따라서 진짜 사용자 수준 skill 분화는 탐색적.
- 가중치 재계산 ablation: deployed agreement **0.651**, equal **0.546**, empathy-only **0.493**, exploration-only **0.579**. **실제 프롬프트/대화 모두 재생성한 full interactive 실험이 아니라 기록된 점수로 state rule만 다시 계산**한 counterfactual.
- 사람 평가: expressive Alex에서는 일부 realism/adaptivity 이점, guarded Sam은 **perceived adaptivity 2.70 (AVP) vs 3.20(static)**, p=.077. 장기 진행이 느리면 타당한 저항조차 '적응하지 않는다'고 보일 수 있음을 시사.

## 5. 범위/한계
**Observed:** 하나의 자기개방 상태에서 explicit, cumulative skill-conditioned transition과 content gating이 prompt only보다 점진적 진행/발화 목표 일치를 높임; 실제 인간 훈련생의 선택을 일부 반영.

**Not established:** 개인차마다 다르게 적합한 transition function; 계획된 **비단조적 하락→회복**의 조건부 타당성; 다차원 trust/affect/readiness 동시 변화; 실제 인간에게서 발생하는 후퇴/관계 손상을 모사; 합성 데이터로 훈련된 **상담 LLM downstream 성능**.

**강한 비교 관련 주의:** Appendix D.1은 5-arm replay를 이미 수행했으므로 '우리는 처음으로 hard vs soft vs free를 비교한다'는 신규성 주장 불가. 재생의 후속 user action 미반영, 사용자 수준 무작위 개입의 결여 등은 남는 방법론적 검증 기회.

## 6. 직접 Research Gap 위협
- **B1 강한 중복:** 목표 상태의 단계적 구조 + 상담사 skill 따라 progression 속도 변경 + state-conditioned generation + 스케줄 baseline. 성격별 속도는 별도의 learned transition function이 아님.
- **B3 강한 중복:** state–utterance faithful realization을 평가하고 개선. 0.651이라 개선 여지는 있으나 단순 목표 레이블 맞추기 novelty 없음.
- **Track A 경계:** '개입 조건부 변화가 훈련생에게 학습 피드백이 된다'라는 동기와 human training usefulness 평가가 있음. 하지만 **상담 LLM을 실제 학습시켜 독립 성능 개선을 비교한 실험은 수행하지 않음**.

## 7. 남는 질문
상담사 관계 손상 등 부정적 개입 시 비단조 rollback·복구를 설계하면, 단순 지속 개방 시스템보다 **유효한 개입에만 변화**하고 잘못된 개입에 강제 진전하지 않는가? 여러 내담자 기질에서 업데이트 계수/관성이 타당한가? 동일 최종 분포지만 intervention→state association을 바꾼 downstream 데이터의 학습효과가 다른가?
