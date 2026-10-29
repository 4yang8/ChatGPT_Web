# SAMem — State-Aware Memory as a Fine-Grained Memory for LLM Agents in Decision-Making

> **Findings ACL 2026, pp.14691–14710** · [Official PDF](https://aclanthology.org/2026.findings-acl.722.pdf) · 20pp · 상세 검토 2026-10-30

## Problem
기존 경험 메모리는 task/scene 수준 요약·가이드에 머물러 현재의 **구체적인 상태**에 맞는 판단 근거를 꺼내는 데 실패할 수 있다.

## Method (§3, Fig.2)
- 각 decision step에서 과거 관찰을 요약한 `state s_t`, 그 상태에서의 **reasoning thought th_t**, 그 생각을 통해 선택한 `action a_t`, reward/next state를 기록.
- `(s_t,th_t)`를 의미적으로 cluster하여 **Q-value**를 학습한 테이블 `Q(s,th)`. 같은 상황에서 도움이 되었던 추상적 사고 지침을 검색해 판단을 유도.
- 경험 중 가치가 낮거나 논리적으로 무효인 thoughts는 forgetting 처리.
- Appendix A: generalization을 위해 thoughts에서 **구체 개체를 제거(de-indexicalization)**. 따라서 저장한 사고는 대부분 **재사용 가능한 decision template**에 가깝고, "그때 내담자가 정확히 어떤 말에 왜 동의했는가"라는 특정 개인 사건의 역사적 기억과 다름.
- 단순 메모리 retrieval을 넘어서 Q-learning과 reward 최적화가 포함됨. 심리 상태에 대한 Q-value나 보상을 임상 타당성과 혼동하지 말 것.

## Experiments and Results
- ALFWorld, ScienceWorld, Jericho, GPT-4o / GPT-4o-mini / Qwen2.5-72B / Llama-3.1-70B.
- Table 1 GPT-4o: Baseline 평균 **32.7**, SAMem **60.5**; ALFWorld SR **62.7→98.5**, ScienceWorld SR **22.2→40.0**. 숫자는 각 환경을 조합한 paper-specific 평균 지표로 상담 모델 점수가 아님.
- Table 2 GPT-4o ablation: Full ALFWorld **98.5**, w/o Q-Learning **84.3**, w/o situation processing **82.8**, w/o decayed ε-greedy **94.0**.
- Table 3 action-level vs thought-level: 9 learning experiences/setting에서 ALFWorld success **79.9** vs **97.7**. **행동 기록보다 상황에 맞는 추론 근거가 다른 상황으로 더 잘 전이할 수 있음**을 보여 주는 강한 선행결과.
- Limitations: tabular Q가 높은 차원의 state 다양성에 취약, sparse reward에서 불안정. 상담의 심리적 fidelity를 검증할 때 Q-reward를 그대로 채택할 근거 없음.

## Our B4 novelty threat
- **Direct conceptual precedent**: `state–thought` 기억, action 발생시 추론 이유 기록, 다음 결정시 조건부 재사용은 이미 SAMem이 연구. 따라서 "상태+생각의 쌍을 기억하는 최초 방법"이라는 표현은 불가.
- **Different abstraction**: 주어진 state에서 **높은 보상을 받는 generalizable thought-policy**를 재사용, 특정 내담자가 그 발화를 할 당시의 **고유한 자기 의도**를 불변 기록으로 기억하는 것이 아님.
- **Critical distinction**: 과거의 이유를 **과거 자기 일관성의 증거**로 기록하는 것(episodic/autobiographical)과, 과거의 이유를 **현재의 최적 행동을 위한 전략 지침**으로 추상화하는 것(procedural/decision policy)은 다름.

## 판정
**B4: DIRECT state–thought technique overlap, but different memory semantics/task objective**. 일반 기억 기술 발명은 주장하기 어렵고, utterance-linked temporal intent fidelity 및 상담 counterfactual 행동 타당성이라는 실험 질문으로 좁혀야 함.
