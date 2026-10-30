# When Clients Stop Following — CARS / STREAMS (2026)

- **원문:** [arXiv:2606.04389v1](https://arxiv.org/pdf/2606.04389)
- **업로드 PDF:** `2606.04389v1.pdf`, 30쪽, 2026-06-03 v1
- **검토 상태:** Full-text initial review (2026-10-29); 외부 재현 검증 미완료
- **연구 중심:** resistance-aware simulated client + strategic counselor RL + 평가 오염(evaluation mismatch)

## 1. Problem / Research gap: 저자 주장
기존 가상 내담자는 치료사 발화에 지나치게 협조적이어서 저항을 몇 턴 만에 철회한다(counselor-following). 공감·유창성 중심 평가가 갈등 회피를 보상하며 상담사 적응 능력을 과대평가할 수 있다(§1, Fig.1).

## 2. Method
- **CARS (simulator):** CBT Cognitive Conceptualization Diagram (CCD), 프로필, session-specific beliefs, preferred counselor style과 이전 상태를 입력으로 상담사의 발화가 부정적 믿음을 활성화/약화하는지 **구조화된 prompt 추론**. 감정 강도, 의도, 비언어, 최종 발화를 생성(§3.1, Appendix A.2, PDF pp.3, 15–16). 별도의 경험적으로 학습된 causal transition 모델로 과대해석하지 않는다.
- **STREAMS (counselor):** Thinker는 15개 이산 상담 전략 중 하나 선택; Presenter는 전략 조건부 자연어 생성. Thinker는 GPT-5.4가 생성·라벨링한 2,000대화 SFT 뒤 PPO; Presenter는 자체 SFT/GRPO 정렬(§3.2, Appendix B.3).
- **규모/비용:** Thinker Qwen3.5-0.6B, Presenter Qwen3.5-8B, 개방형 훈련 약 **106 A100 GPU-hours** (SFT20, RL86), Appendix B.3.
- **EWTS-MI:** 내담자의 예측하기 힘든(high-self-information) 반응에 대한 상담사 적응을 강조한 가중 상호정보량. 고득점만으로 임상적 적절성을 뜻하지는 않음(§3.3, §4.5).

## 3. 실제 비교·주요 결과
- 저항 조건은 **CARS**, 협조 조건은 **Patient-Ψ**로 정의. 동일 시뮬레이터의 저항 on/off 조작이 아니라 **시뮬레이터까지 달라지는 비교**이므로 경계해야 함 (Appendix C.1).
- 시뮬레이터 백본 4종, 총 120대화, CBT 전문가 4명 평가(§4.2, Table 1):
  - DeepSeek-v3.2 Fid **3.87**, Rationality **3.96**, Emotional Reasoning **4.27**, emotion-change Model-Human Consistency **81.3%**. GPT-5.4 MHC **74.2%** 등.
  - MHC는 평가자가 기대한 **감정 변화 방향과 모델 예측 일치**이며 실제 인간 환자 종단 인과효과의 ground truth가 아님.
- 19개 상담 모델 평가. 자동 PsychEval 성능이 높더라도 저항 회피를 보일 수 있다는 **평가 불일치** 발견(§4.3, Table 2).
- 임상 관련 인간평가(각 모델 30 대화, CBT 전문가 4명; §4.5, Table 4): **STREAMS-RL** 전략 적절성 **4.23**, drift **4.27**, progress **3.51** / **STREAMS-RG** **4.31 / 4.28 / 4.05**, **Graph2Counsel** **3.81 / 3.80 / 4.01**, **Soulchat2** **3.26 / 4.22 / 3.25**.
- 저항·비저항에서 정량 지표(EWTS-MI, RTF, UEC) 비교. GPT-5.4의 높은 EWTS-MI도 전략 회피와 결합되므로 단일 척도 해석은 위험(§4.5).

## 4. 무엇을 보여줬고, 무엇은 아직 보여주지 않았나?
**Observed:** 명시적 CCD 기반 저항 시뮬레이션, 작은 정책/응답 모델의 전략 RL, 자동점수와 전문가가 판단하는 전략적 유용성의 간극이 존재한다.

**Not isolated:** 다양한 훈련 시뮬레이터를 **동일 모델·데이터 양·주제·저항 노출량 조건에서 맞춘 뒤**, '개입에 따른 상태변화의 충실도'만 바꿔 상담사 학습 효과를 인과적으로 추정하는 실험. STREAMS의 SFT→RL 비교만으로 시뮬레이터 동역학의 독립효과가 입증되지는 않는다.

**한계:** CBT/고정 CCD 라이브러리, 수동 전문가 평가 표본, 단일 세션, EWTS-MI의 추정 불안정성과 RTF의 미묘한 저항 누락(§5 Limitations). 학습 보상과 환경 내부 상태를 함께 사용하므로 동일 환경 보상 최적화가 실제 인간에 대한 일반화인지 별도 평가가 필요.

## 5. 우리 Research Gap에 주는 판정
- **G2(개입 조건부 저항/상태변화 자체 신규성)**: CARS·PUPPET에 직접 중복, 넓은 형태는 **Reject**.
- **G1(어떤 simulator fidelity가 downstream에 실질적으로 기여하는가)**: CARS가 임상적으로 중요한 세부 영역을 크게 잠식하므로 **Narrow**. 단, factor-matched simulator comparisons, 미지의 상담 전략/OOD, 진짜 independent evaluation은 검증할 여지가 있을 수 있음.
- **G3(소형 모델+도구)**: STREAMS가 0.6B+8B 전략 분리를 이미 보였으므로 단순 모델 규모/분리 구조 신규성 낮음.
- **G4(단계별 계획의 의미적 기여)**: Thinker–Presenter 분리만으로 충분하지 않고 계획이 실제 의사결정을 변경하는지 측정 필요.

## 6. 다음 중복 검증
- **RESISTCLIENT** (arXiv:2604.10507, 2026): 본문에서 직접 인용; 반드시 PDF 확보.
- **Psycheval** (arXiv:2601.01802, 2026): 평가 중복 여부.
- CARS를 여러 상담사 전략에 조건부로 평가하고, 장기 관계 형성/심리 변화를 별도로 타당화한 논문 탐색.


---

## Memory read 관련 참고 (Iteration 6)
- **Appendix A.2, pp.15–16**: generation template가 `Thinking / Emotion / Intention / Response`를 생성하고, 입력에 `DIALOGUE_HISTORY`, **`PLANNING` (previous feelings and goals regarding therapist's response)**, `EMOTION_STATE`를 둠.
- **Access type**: 이전 감정/목표는 기본 입력에 제공하는 형태. 저장한 모든 **과거 발화별 private intent를 선택적으로 검색해 주입**한다는 정책이나 turn-id indexing은 PDF에서 확인되지 않음.
- **B4 비교 중요성**: client-domain에서 이미 '이전 의도/감정 → 다음 발화'가 구현됨. 그보다 세밀한 *발화 단위 동기 provenance*의 효과를 입증해야 함.

[Iteration 6 조회 비교](../iteration_06_memory_access_and_temporal_reinterpretation.md).
