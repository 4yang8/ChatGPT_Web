# Memory-Driven Role-Playing (MDRP): Evaluation and Enhancement of Persona Knowledge Utilization in LLMs

> **2026-03-14** · [arXiv:2603.19313v1](https://arxiv.org/pdf/2603.19313v1) · PDF 34pp · 상세 검토 2026-10-30 · 관련 Track B4

## Problem (§1–2, pp.1–3)
역할 수행 모델은 설정 persona를 단기적으로 표현할 수 있어도, 긴/열린 대화에서 **상황 단서만으로 적절한 인물 기억을 찾아 실제 행동에 적용하는 능력**이 부족할 수 있다. 기존 통합 roleplay rating은 실패가 anchoring/selection/bounding/enactment 어디서 생겼는지 분리하지 않는다.

## Method (§3, Fig.2–3)
- **MDRP**: Long-Term Memory `M_L`=사전에 구축된 인물의 traits/scene facets; Short-Term Memory `M_S`=최근 `K`개의 공개 대화 발화. `p(y|M_L,M_S)`.
- **MRPrompt** = Narrative Schema + Magic-If protocol. 페르소나 core traits와 장면별 `situation`, `emotional_state`, `behavior_pattern`, **`thinking_pattern`**, `time_scope`, `cue_phrases` 등. 현재 대화의 cue로 해당 facet을 찾아 boundary check 후 발화.
- **MREval**: Memory Anchoring(MA), Selecting(MS), Bounding(MB), Enacting(ME)의 4단계×2 = 8개 세부 지표.
- **MRBench**: 중/영어 소설 16편에서 구성한 단일 턴 continuation task, 동일한 대화 맥락을 유지하고 persona 기억/질문 부분을 최소한으로 바꿔 controlled counterfactual 비교.
- **특별히 중요**: `thinking_pattern`은 *이미 알려진 장면/인물의 일반적 사고 패턴*을 LTM에 명시한 값이지, **실제 온라인 진행에서 직전 n턴 발화를 선택할 때 생성된 개인적 심리 의도**를 추가 기록한 것이 아님.

## Results
- Table 2 / Table 4: Qwen3-8B Base **7.95**, +MRPrompt **8.12**. GLM-4.7 Base **8.11**, Qwen3-Max Base **8.08**. 일부 소형모델+명시 기억은 대형모델+기본 입력과 경쟁하지만 **동일 구조로 대형모델을 비교했을 때 완전 우위가 아니다**(GLM-4.7 MRPrompt **8.13**).
- Table 3: Qwen3-4B Base **7.85**, +Protocol **7.93**, +Schema **7.97**, Full **8.07**. Schema와 Protocol 영향 구분.
- Fig.5: MA/MS/MB의 집계 점수와 ME 상관 **r=0.35**, 유의미하지만 대응 성능 전체를 설명하지 못함.
- **Limitations p.9**: 고정 `(M_L,M_S)`를 입력받아 **단 한 번의 다음 발화**를 생성; **within-session memory update 없음**. 장기적으로 기억을 누적·교정·재해석하는 실제 상호작용을 입증하지 않음.

## B4 overlap / difference
**Overlap**: '생각/행동 패턴을 검색하고 적절히 표현'이라는 원리는 이미 구현. 기록을 "상황 단위로 선택해서 말에 반영"하는 것도 새로움 약함.

**Difference**: static persona memory가 아닌 **생성 중 매번 새로 발생한 자신의 비공개 선택 이유**를 특정 발화에 연결해 기록하고, 몇 턴 뒤 그 의도와 당시의 현재 목표를 함께 구별하며 기억/상태를 갱신할 때의 효과.

**Methodological takeaway**: 한 점수 대신 *anchoring, selecting, bounding, enacting*을 분리하고, 동일 공개 대화와 다른 개인 내면 기록으로 controlled counterfactual 평가. 하지만 이 분리 자체는 MREval이 이미 연구.

## 현재 판정
**B4: INDIRECT overlap**. 강한 일반 roleplaying 평가 baseline이지만 persistent episodic private-intent memory는 방법 범위 밖.


---

## Memory read/access 상세 (Iteration 6, §3.1–3.3, pp.3–5)
- **LTM + STM 기본 컨텍스트 주입**: 정적 Narrative Schema(`core_traits`, `scene_facets`, `thinking_pattern` 등)와 현재 최근 dialogue `M_S`를 다음 발화의 **하나의 프롬프트에 같이 제공**.
- **Query-time selection**: Magic-If Protocol이 대화 cue를 읽고 대응 facet을 **같은 모델 추론 내부에서 선택**하며 time/knowledge boundary 체크 후 발화. 외부 벡터 DB/Memory Tool 호출 아님.
- **Execution**: 프롬프트 절차는 매 응답에 적용; 모델이 별도 memory tool 호출이 필요한지 판단하는 gate 없음.
- **한계**: 실제 평가가 fixed `(LTM,STM)`의 단일턴 continuation; 자기 발화마다 새로운 private motive를 써서 나중에 다시 조회하는 mechanism을 검증하지 않음.

[4논문 read-policy 비교](../iteration_06_memory_access_and_temporal_reinterpretation.md).
