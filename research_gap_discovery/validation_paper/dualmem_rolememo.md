# DualMem / RoleMemo — From Facts to Insights: A Persona-Driven Dual Memory Framework and Dataset for Role-Playing Agents

> **2026-05-25** · [arXiv:2605.25693v1](https://arxiv.org/pdf/2605.25693v1) · 사용자 첨부 PDF 24pp · 본문 전문 검토 2026-10-30 · **Track B4 직접 경쟁**

## 우리의 검토 질문
"과거 n턴에서 내담자가 발화를 선택할 때 가진 **비공개 해석·의도·목적**을 **그 시점에 기록**하고, 이후 턴에서 그 특정 발화와 연결해 기억하는가?"

## 저자 Problem/Gap (§1–2, pp.1–3)
전형적인 메모리 프레임워크는 대화에서 **객관적 Fact**만 저장하고, 역할/인물의 고유 관점에서의 **Insight**는 보존하지 않아 장기 페르소나 일관성이 떨어진다고 주장. 길이가 길어질수록 추론시 인물의 입장에서 사실을 재해석하기 어렵다는 문제.

## Method (§3–4, pp.3–5, Fig.2)
- **RoleMemo dataset**: 2,052 personas / 1,702 topics / 20,244 queries / 최대 256K-token history. Interpretive Attribution, Contradiction Revelation, Value Judgment, Decision Guidance의 4개 질의 유형. 사실이 여러 먼 턴에 흩어지고 인물의 관점에서 이를 연결해야 답이 나온다.
- **DualMem memory bank**: *factual cognition* (대화에서 추출한 객관적 사건·사실) + *insight cognition* (인물의 가치관·목표·성격에 조건부로 그 사실을 해석한 인사이트). 각 insight와 grounding facts는 연결된 채 저장·검색한다.
- **Write mechanism**: 최근 대화 chunk `C_i`, persona `p`, 이전 메모리 `M_<i`에서 `M_i={F_i,I_i}=fθ(C_i,M_<i,p)`을 구축. 필요시 이후 질의에 맞게 재검색 `R(M,q)`.
- **Training**: Qwen3-4B memory model SFT / DAPO-style RL; 생성/평가/roleplayer 모델 별도. insight가 증거로 도출 불가능할 때 `null`을 출력하도록 negative samples 포함 (§4.3).
- **중요 범위**: 이 *Insight*는 **대화에서 관찰된 사실에 대한 persona-conditioned 해석을 기억 작성 단계에서 합성**한 것이다. 인물이 그 발화를 생성할 때 그 이전에 가졌던 실제(시뮬레이터 내에서 확정된) **사적 발화 목적**을 그대로 timestamp+utterance에 저장한 것이 아님. 또한 장문 history를 전처리하고 **후속 query 하나**로 평가하는 설정과, 매 턴 자기 마음을 갱신하는 online client simulator는 다른 문제.

## Main results
- Table 2, memory Recall@10: best baseline insight **0.41**(SimpleMem), DualMem-SFT **0.65**, DualMem-RL **0.73**. Fact: Mem0 **0.74**, DualMem-RL **0.77**. 같은 685B 기반 baseline이 항상 최고라는 뜻이 아니며 architecture+training confound 존재.
- Table 3, role-play quality(5점): NoMem **3.76**, best baseline Mirix **4.01**, DualMem-SFT **4.15**, DualMem-RL **4.16**. DualMem-RL Character Consistency **4.37**. **w/o Insight → 4.01**, **w/o Fact → 4.00**, **w/o Training → 4.02**. 유익성의 실험 근거가 단순한 구조 제안 이상으로 강함.
- §6.1: 200질의에 대한 사람-judge Pearson r **0.82/0.84** (두 사람 상호 0.88), 독립 생성기 서브셋에서 우위 재현(App. J).
- Fig.3: 256K context까지 일부 조건 성능 유지; 장기 실시간 인간 치료 세션이 아님.
- Table 6 p.8: **상담 장면**에서 "환자가 공동체를 피하는 것은 결핵 치료 자체가 아니라 낙인 우려"라는 Insight를 쓰는 예. 의료 상담의 **실제 잠재 의도 ground truth**를 관측·검증한 것이 아님.

## 우리 아이디어와의 중복/차이
**Direct overlap**: 사실만 저장하면 부족하고 **상황·인물 관점에서 해석한 기억을 별도 보존할 필요**가 있다는 일반 주장. "객관적 기억+주관적 해석" 단순 발명이라는 주장은 기각.

**Distinct candidate (needs proof)**: 특정 **내담자 자기 발화** `u_t`와, 그것을 선택하기 **직전의 비공개 의도/망설임/은폐 이유** `z_t`를 발화-시간에 기록하고, 이후 동일한 외부 발화의 숨은 의미를 안정적으로 구별하는가? DualMem은 사후/청크 처리로 **facts→insights**를 해석하며 실제 *speaker-at-generation* rationale identity와 향후 변경을 별도 평가하지 않는다. "다음 질의에서 다시 해석" vs "기록 당시 추론"을 혼동하지 말 것: DualMem도 **메모리 작성 시점**에 해석을 저장하지만 **발화 생성 당시 자기 의도의 원천**과는 다름.

**Threat**: 강한 insight memory baseline이 이미 역할 수행 평가를 개선했다. B4가 의미 있으려면 DualMem형 memory와 별도 통제 비교가 필요. 사실관계 없는 psychological inference의 hallucination/human relevance를 조심.

## 현재 판정
**B4: NARROW / 핵심 이론과 직접 중복, temporal utterance-grounded motive provenance가 남을 가능성**. 임상적 타당성/후속 대화 일관성은 향후 실험 대상. 이 문서만으로 새로움 확정 금지.


---

## Memory read/access 상세 (Iteration 6, §4.1–4.2, §5.1, pp.4–5)
- **모든 query의 기본 경로**: `q→R(M,q)→fφ(p,q,R(M,q))`. 검색 자체를 할지/말지 스스로 결정하는 agentic gate는 논문에 별도로 명시되지 않음.
- **Retrieval selection**: 모든 방법을 공정 비교하기 위해 Qwen3-Embedding-0.6B로 **top-10 relevant entries** 검색; Insight entry를 검색하면 연결된 Fact entry도 grounding을 위해 포함.
- **Write/read 타이밍**: write 단계에서 `M_i=fθ(chunk_i,prior_memory,persona)`로 **사실과 해석** 생성. read 시에는 **현재 query와 관련된 이전에 저장된 해석을 선택해 제공**하는 것이지, 과거 사건의 현재 심리적 의미를 반드시 다시 갱신하는 것은 아님.
- **B4 차이**: remembered insight = persona-grounded interpretation, not necessarily the speaker's intention committed **while producing that particular utterance**. 재해석의 출처·시점 검증이 필요.

[4논문 read-policy 비교](../iteration_06_memory_access_and_temporal_reinterpretation.md).
