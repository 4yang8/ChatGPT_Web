# Graph2Counsel — Clinically Grounded Synthetic Counseling Dialogue Generation from Client Psychological Graphs (2026)

- **원문:** [arXiv:2604.20382v2](https://arxiv.org/pdf/2604.20382)
- **검토 PDF:** `2604.20382v2.pdf`, 60쪽; 2026-08-30 v2
- **검토 상태:** Full-text initial review (2026-10-29)

## 1. Problem / 저자 주장
자연어 프로필/설문 기반 합성 상담 대화는 **생각–감정–행동 관계**를 구조적으로 표현하지 못해 심리적 모순이 발생할 수 있다. 실제 상담에서 추출한 *Client Psychological Graph (CPG)*로 상담 데이터 생성의 grounding을 강화(§1).

## 2. Method
- CPG 노드는 심리 과정, 방향 간선은 **excites/inhibits** 관계. 실제 6명의 환자 76개 세션에서 추출한 **76 CPG** 사용; CPG당 인구통계 등이 다른 프로필 10개 생성해 **760 대화**(약 40.12 turns)를 만듦.
- 입력 조건: CPG only / CPG-derived profile only / CPG+profile; 생성 조건: Guided Counseling(GC), GC+CoT, GC+Multi-Agent Feedback(최대 3회 수정). 상담 전략은 원문에서 Llama3.1-70B로 추출하고 GPT-4o로 대화를 생성.
- 데이터에서 Llama3-8B-Instruct를 **QLoRA SFT**; CounselBench, CounselingBench, 신규 multi-turn **Therapy-Eval**로 평가.
- **주의:** CPG는 세션 중 매 턴 갱신되는 상태 변수가 아니라 **시작할 때 주는 정적 관계 스냅샷**이다. §7/Future Work에서 temporal CPG를 차기 연구로 제시.

## 3. 주요 결과
- **전문가 4명, 유사한 presenting issue를 매칭한 100개 사례**; Table 2 (평균 rank **낮을수록 좋음**):
  - Graph2Counsel specificity **1.79**, competence **1.67**, authenticity **1.43**, flow **1.48**, unsafe session **0.5%**.
  - MAGneT **3.97 / 3.98 / 3.99 / 3.99 / unsafe 1.0%**.
  - Krippendorff α **0.70**.
- **CounselBench-Eval, Table 3:** Llama3-G2C **overall 4.29 / empathy 4.26 / specificity 4.30**; Llama3-MAG **4.12 / 4.09 / 4.12**. 모든 안전/사실성 지표에서 단독 우위는 아니다.
- **Therapy-Eval, Table 4:** G2C overall **3.81**, questionnaire 기반 SQP **3.80**(차이 미미); G2C authenticity **3.72**, SQP **3.77**(SQP 우위). G2C 평균 turn **30.57**, SQP **22.56**.
- **추가 추론 비용:** GC+MA 피드백 반복은 비용 >2배 증가해도 유의미한 품질 개선이 부족; GC+CoT도 거의 2배 비용 및 대화 길이 축소(약 30 vs 40턴). 저자들은 **GC + CPG+profile** 선택 (§6 p.8).
- 입력 충실도: CPG 중 적어도 한 번 언급된 심리 과정 비율 **0.91**, 프로필과 모순 없는 대화 비율 **99%**. 이는 **간선의 인과적 진실성이나 시간에 따른 상태 업데이트의 정확도**와 동일한 지표가 아니다.

## 4. 중요한 실험 해석
**실증:** 실제 임상에서 추상화한 구조적 정보를 넣어 합성 세션 생성과 SFT 결과를 개선할 수 있음. 같은 연구에서 GC와 CoT/MA의 추가 추론 비용을 정면으로 비교함.

**미분리:** 구조적 CPG 입력에서 비롯된 효과, 상담 전략/프롬프트/데이터 양 차이를 Downstream에서 완전히 factor-matched 하지 않음. Graph2Counsel **760개**, MAGneT **442개**, CACTUS **31,577개**로 훈련 세트 규모가 다르고, CACTUS는 이미 학습된 CAMEL 체크포인트 이용. 전문가는 유사 주제를 매칭했지만 Downstream 데이터량·출력 분포까지 통제한 것은 아님.
**비교 한계:** 76개 CPG가 환자 6명에서 유래; 문화·기관 편향 및 같은 graph blueprint 반복 노출 과적합 가능. 논문 자체가 인지 상태의 종단 변화는 아직 모델링하지 않는다고 인정 (§7).
**중요한 부정 결과:** 무조건적인 CoT/MA은 불필요할 수 있음 → 우리 Q3a vs Q3b의 원인을 비분리 상태로 해석하지 말 것.

## 5. 우리 gap과 겹치는 부분
- **G1:** '심리적 구조로 생성한 데이터가 상담 모델 학습에 유익'이라는 범용 주장은 이미 수행.
- **G2:** 이 논문의 CPG는 정적이므로 *계획/관계가 상담 개입마다 업데이트되는 것*은 차별점일 수 있지만 **PUPPET/CARS가 이미 동적 시뮬레이션**을 다룸.
- **G3/G4:** 추론 비용·계획 효율 논리의 선행 중요 증거. 같은 품질에서 더 적은 tool calls를 실증할 때만 의미 있음.

## 6. 후속 질문
- **static vs dynamic CPG**를 데이터량/다양성 매칭 후 상담사 훈련 효과로 검증한 2026 논문 탐색.
- 실제 인간 내담자 반응 분포와 CPG 기반 반응의 일치 정도 비교; 6명이라는 seed 한계.
- 최적 CoT/MA 조건 선정시 사용한 judge 지표와 인간 선호의 차이.
