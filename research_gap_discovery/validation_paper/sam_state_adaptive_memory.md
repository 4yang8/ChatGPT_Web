# SAM — State-Adaptive Memory for Long-Horizon Reasoning Agent

> **2026-05-23** · [arXiv:2605.24468v1](https://arxiv.org/pdf/2605.24468v1) · PDF 29pp · 상세 검토 2026-10-30 · B4의 generic memory-architecture 선행연구

## Problem (§1)
장기 web/research agent는 생각(thought), tool call, observation, partial conclusions가 섞인 긴 실행 trace에서 **나중에야 중요해진 오래된 추론 과정**을 다시 찾아야 함. 무조건 summary/삭제/최근-K는 어느 사실·생각이 현재의 목표에 관련되는지 놓칠 수 있다.

## Method (§2, Fig.1)
- RAW trajectory `[(a_1,o_1),...,(a_t,o_t)]`의 action에는 **agent internal reasoning trace, tool arguments, tool responses, partial conclusions** 등 이질적 내용이 들어갈 수 있다. 단, 실제 모델 내부의 비공개 Chain-of-Thought를 직접 추출했다는 뜻이 아니라 **시스템에 출력/저장된 텍스트형 사고·행동 기록**.
- 일정 token budget에 달한 구간을 **raw page** `p_k`와 **compact cue** `m_k=M_sum(p_k)`로 나누어 보존. 압축 cue는 pointer이고 raw trace는 삭제하지 않음.
- 호출 시 agent가 **현재 recall intent `q_t`**를 제시; cue에서 관련 page 선택, 메모리 모델이 `ρ_t=M_rec(q_t,p_k...)`로 필요한 지식·과거 결정 이유를 재구성.
- Memory module 독립 훈련 Qwen3.5-9B SFT + OAT-GRPO, reasoning backbone frozen. 추가 **Qwen3.5-27B LoRA-only SFT** 메모리 실험(27B임 자체가 새로움 아님).

## Evidence
- Table 1, four-benchmark average, GLM-4.7: no context management **49.4**, summary **54.6**, **SAM 57.0**. Qwen3.5-35B-A3B: no context management **44.5**, summary **46.1**, **SAM 48.8**. 반면 다른 pretrained agent baseline 수치는 재실행되지 않고 원 논문 표 사용; 핵심은 동일-backbone context 관리 대조군.
- Fig.2 left, GLM agent: 9B trained SAM **57.0** vs **SAM-27B 56.7**(27B는 LoRA SFT만, 9B는 SFT+RL이어서 직접 backbone-size 인과 비교 아님). 특정 모델 크기의 우수성이 핵심이 아니라 episodic recall architecture 검증.
- Fig.2 right, Qwen agent BrowseComp: no CM **36.0**, summary-only **39.0**, recency **37.0**, raw-content **38.0**, **SAM 42.2**. query-dependent retrieval이 단순 raw recall보다 낫다는 근거.
- Appendix E.2: **실패 사례**로 초기 잘못된 가설을 잘 저장/검색해 오히려 잘못된 결론을 강화하는 "memory amplifies wrong frame". 기억 내용이 충실해도 원래 해석이 틀릴 수 있음.

## B4 overlap and exact difference
**Strong generic overlap**: 과거 사고·선택 과정 기록, cue를 통해 필요 시 재구성, 자기 행위의 이유를 나중에 다시 찾는 **architecture principle**은 이미 SAM이 다룸. "내적 reasoning까지 기억하는 agent"라는 일반 novelty 주장 불가.

**Differences**:
- Task는 BrowseComp 등 정답 기반 web/research agent로, 상담 내담자의 **비공개 의도/표면적 동의의 불일치/기억 변화**가 아님.
- SAM은 저장된 전체 agent trace를 뒤에서 **필요한 결정 정보로 재구성**하는 도구. 내담자가 **자기 발화 이전에 확정한 motive를 utterance-key로 시간 보존**하고, 이후 새 경험으로 기존 motive의 해석과 현재 intent를 구별하는 임상/역할 수행 fidelity를 검증하지 않음.
- 32K page / 64K trigger가 필요한 장문 기억 관리에 특화; 짧은 10–30턴 대화에서 그대로 쓰는 것은 비용/구조 불필요할 수 있음.
- 복원 자체가 선택된 goal에 편향될 수 있음을 논문도 실패 사례로 인정.

## 연구적 시사점
Private intent ledger를 만들 경우 **기록의 출처, 언제 확정됐는지, 과거 원문 보존 vs 현재 재해석, 선택적 검색에서의 편향**을 분리해야 함. 단순 raw-scratchpad 저장 대신 **목적·발화·시점 연결과 근거/확신도**가 질문. `SAM` 일반 architecture 적용만으로 Method novelty 주장은 부족.

## 판정
**B4: HIGH implementation overlap / different application question**. 강한 memory retrieval baseline과 오류 사례를 제시.


---

## Memory read/access 상세 (Iteration 6, §2.2, Appendix B.1, pp.3–5,15)
- **Two levels**: compact memory cues는 active context에 유지, detailed raw trajectory pages는 외부 저장. **64K active-context threshold는 write-time consolidation 조건**이며 memory recall 요청 trigger가 아님. Page size 최대 32K.
- **Optional lookup**: Agent가 추가 정보가 필요하다고 판단할 때 **memory tool 호출**, 스스로 recall intent `q_t`와 관련 cues/pages를 지정. 검색 page의 선택은 고정 score threshold보다 agent의 현재 목적에 의존.
- **Tool result**: Memory Model이 선택된 raw pages를 `q_t`에 조건화해 relevant support `ρ_t`로 재구성하고 active context에 주입. 새 상태를 판단하는 건 reasoning agent.
- **주의**: 읽기 시 재구성은 **task-specific extraction/reframing**. "그때 이런 생각이었다"라는 당시 기록과 "지금 돌이켜보면 그때의 의미는 다르다"라는 **새로운 내면 해석의 역사**를 분리 저장/평가한 것은 아님.
- **현재 Q3와의 근접성**: 필요한 때 `retrieve_client_memory` Tool 사용을 선택하는 점은 유사. 그러나 Q3에 SAM의 raw page+cue 구조와 goal-conditioned reconstruction이 이미 있다고 주장하지 말 것.
- **Critical failure (§App. E.2)**: query goal 자체가 잘못된 초기 가설에 편향되면 원본을 충실하게 보존·검색하면서도 오판을 강화 가능.

[4논문 read-policy 비교](../iteration_06_memory_access_and_temporal_reinterpretation.md).
