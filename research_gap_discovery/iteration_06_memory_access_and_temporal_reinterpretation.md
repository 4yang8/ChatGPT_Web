# Iteration 6 — Memory Access Policy, Historical Preservation, and Temporal Reinterpretation

> **2026-10-30** · 기존 2026 전문 검토 16편에서 특히 DualMem, SAM, SAMem, MRPrompt 및 상담 분야 CARS/CALM-IT/PATIENTACT를 비교. **확인된 구현**과 **사용자의 아이디어·우리의 후속 가설**을 분리한다.

## 1. 이번 대화의 연구자 질문

과거 n턴에 내담자가 어떤 발화를 하면서 생각했던 것(비공개 해석, 목적, 망설임 등)을 **기록**한다는 것은, 이후 k턴 생성에서 **그 기억을 필요에 따라 참고할 가능성**을 남긴다는 뜻이다.

이때 다음은 모두 서로 다른 설계·평가 질문이다.

- **무엇을 저장하나? (Content/Provenance)** — 해당 발화가 나온 당시의 숨은 목적/해석, 아니면 사후에 사실을 보고 재추론한 Insight인가?
- **언제/어떻게 조회하나? (Access/Injection)** — 항상 입력에 넣는가, 매 query Top-K인가, 에이전트가 필요할 때 Tool로 조회하는가, 상태와 Q-score로 자동 선택하는가?
- **조회한 정보를 어떻게 쓰나? (Use/Interpretation)** — 당시 의도를 역사적으로 보존하는가, 현재 질문을 위해 관련 정보를 재구성하는가, 새로운 경험으로 과거의 의미를 심리적으로 새롭게 해석하는가?

**주의:** Memory content와 retrieval policy는 실험에서 분리해야 할 독립 축. Retrieval policy의 신규성을 B4의 사전 가설로 강제하지 않는다.

## 2. 네 일반 에이전트/Role-Playing 논문이 실제로 사용한 조회 정책

| Method | 기본 컨텍스트에 있는 것 | 언제 메모리를 읽는가? | 어떤 관련성을 계산하나? | 입력으로 되돌리는 것 | 근거 |
|---|---|---|---|---|---|
| **DualMem / RoleMemo** | persona, current query; fact/insight memory bank 별도 | **매 evaluation query**마다 검색 경로 `R(M,q)` 호출. optional tool gate 미제시 | Qwen3-Embedding-0.6B **Top-10**, insight hit→linked facts 동시 포함 | fact + persona-conditioned insight | §4.1–4.2, §5.1, pp.4–5 |
| **MRPrompt / Memory-Driven RP** | 구조화된 **전체 persona LTM**과 최근 dialogue STM | **매 응답**의 Magic-If 과정에서 LTM 내부의 facet을 선택 | 현재 dialogue cue→scene facet 및 `thinking_pattern` 매칭; 별도 외부 도구 X | 선택한 facet을 반영한 **같은 호출의 응답** | §3.1–3.3, pp.3–5, Appendix O |
| **SAM (State-Adaptive Memory)** | 최근 live context + 압축 memory cues, 원본 raw pages 외부 | **Agent가 추가 정보가 필요하다고 판단할 때** Memory Tool 호출 | Agent가 **recall intent 및 cue/page를 직접 지정**. Memory Model이 선택된 pages를 현재 intent로 재구성 | `ρ_t` focused recall을 live context에 주입 | §2.2, pp.3–5, Appendix B.1 p.15 |
| **SAMem (State-Aware Memory)** | 현재 `s_t`, state–thought Q-table 외부 | **시험 시 매 decision step**. Train은 ε-greedy로 memory vs 새로운 thought 선택 | state embedding cluster match(≈0.85), `argmax_{thought}Q(s,thought)` | 선택한 reusable high-value reasoning thought를 행동 판단에 제공 | §3.2–3.3, pp.3–4; Appendix A |

### 조회 여부와 저장 시점은 별개 — 특히 SAM
- SAM에서 64K active context 초과 시 memory consolidation 수행(원본 raw page 최대 32K, cue는 남김). **64K는 Tool을 호출해야 한다는 '조회 기준'이 아니다.**
- Recall은 Agent가 **필요하다고 판단했을 때**, 목표 `q_t`를 써서 tool call. Memory Model은 원래 pages를 다시 읽고 요약하며 **조회 결과를 모델 활성 컨텍스트에 삽입**.
- DualMem에서 embedding top-10은 **항상 검색하되 어떤 항목을 고르는지**의 제어다. MRPrompt에서 facet selecting은 **프롬프트 내부 attention/reasoning**, SAMem에서 Q-max는 **상태 기반 정책 메모리 선택**이다. 네 방식은 아직 고정할 필요가 없다.

## 3. 상담 논문들은 어떠한가?

| 논문 | 실제 조회/주입 설계 | 우리의 B4와 구별할 부분 |
|---|---|---|
| **CARS/STREAMS** | 이전 감정·목표 `{PLANNING}`, 현재 감정, Dialogue History를 다음 발화 생성 프롬프트에 제공 (Appendix A.2) | 직전 감정/목표 carry-over는 이미 존재. 과거 **특정 발화 ID에 묶인** 숨은 motive의 저장·장기 선택적 회상은 직접 확인되지 않음 |
| **CALM-IT** | `UpdateSummary` 및 `c,g,e,ρ,q` 상태를 매 턴 함수 인자로 전달 (Algorithm A1). 대화 발화는 history에 append | 과거 발화의 **자기 숨은 의도**를 source-linked episodic record로 조회한다고 명시하지 않음 |
| **PATIENTACT** | 매 턴 상담사 발화의 **activation tag**와 item relevance를 검사하고, **trust≥disclosure threshold**를 만족할 때 공개 가능한 dynamic memory item을 제공; 불편한 blocked items는 회피/저항 유도 (§3.3.1–3.3.4) | 내적으로 기억에 접근하는 것과 **상담사에게 공개하는 것**은 다른 단계다. 저장된 비공개 의도를 조회해도 반드시 발화로 노출하면 안 됨 |
| **MASC** | Retry Memory는 같은 턴에서 목표 레이블과 실패 발화의 차이를 다음 재생성에 주입 | cross-turn episodic private memory와 다름 |

## 4. 중요한 개념 정리: 과거 생각 보존 ≠ 현재 목적에 따른 재구성 ≠ 심리적 재해석

### A. Historical preservation — 과거 생각을 그때의 기록으로 남김
예를 들어 내담자가 8턴에 **"네, 해볼게요"**라고 공개 발화하고, *당시* 속마음으로 **"거절하기 곤란해서 동의하는 척했다"**를 생성·확정했다면 기록:

`E_8 = (utterance_8, motive_then_8, emotion_then_8, source=turn_8)`.

SAM은 과거 trace(생각·행동·관찰) 원본 보존의 일반적인 선행 모델이다. 다만 장문 research agent의 실행 trace와 **자기 발화 순간의 개인적 심리 의도**는 의미적으로 다르다.

### B. Contextual recall / task-oriented reconstruction — 현재 필요에 맞게 꺼냄
15턴 상담사 질문: **"전에 해본다고 했는데 실제로 해봤나요?"**

SAM형 query-conditioned retrieval은 과거 페이지에서 **"그때는 형식적으로 동의"**라는 사실을 찾아 현재 질문에 관련된 부분으로 압축해 전달한다. **이 단계는 과거 경험을 현재 다르게 평가했다는 뜻이 아니다.** 단지 같은 원본 기록 중 **현재 질문에 유의미한 부분**을 골랐다.

### C. Temporal reinterpretation — 새로운 경험으로 과거 사건의 현재 의미를 새롭게 형성
이후 내담자가 상담사와 관계가 좋아지면 15턴에 **"그때는 강요받는다고 느꼈는데, 지금 돌이켜보면 나를 돕고 싶어서 한 제안이었을 수도 있겠어요"**라고 할 수 있다.

- 당시 기록 `motive_then_8`: *그때는 거절 곤란 → 형식적 동의*. **회상 가능한 역사 기록**으로 남음.
- 현재 해석 `interpretation_now(8,15)`: *지금은 당시 제안을 다르게 평가* (새 근거에 따라 변경 가능).
- 현재 목표/신뢰 `state_15`: 현재 상황에서 별도 갱신.

**SAM의 read-time reconstruction은 B에 더 가까움**, **DualMem의 persona-conditioned insight는 A의 사실 기록과 해석을 *write-time*에 생성·저장하는 것**이며, **C와 같이 '같은 과거 자기 사건의 의미를 추후 상담 경험으로 재평가하는 명시적 심리 변화'가 핵심 실험은 아니다**.

기억의 원래 정보가 진짜였는가와 그것을 정확히 회수했는가도 별개의 질문(SAM Appendix E.2 wrong frame: 잘못된 사고를 충실히 회수하면 오류 강화).

**C는 제안된 새로운 실험 질문(B4-T)**일 뿐, 위 논문들이 구현 불가능하거나 전체 연구 영역에서 미연구임을 입증한 것은 아니다. 실제 인간의 자전적 기억도 완전 고정되지 않으므로 '불변'은 **시뮬레이터가 과거 턴에 작성한 기록의 provenance 보존**이라는 실험 설정을 가리킨다.

## 5. 현재 Q3와 가장 가까운 것은 무엇인가?

**Memory access 설계 측면에서는 SAM이 가장 가깝다.** 기존 Q3 계열의 `stage_client_update`(appraisal/turn_goal/response_plan), 선택적으로 사용하는 `retrieve_client_memory`, `commit_client_utterance`, `update_client_notes`라는 흐름 때문이다.

그러나 현재 Q3가 **SAM의 cue + raw pages + recall goal + reconstruction** 구현을 갖췄다는 의미는 아니다. 또한 `update_client_notes`가 모든 턴의 **원본 private motive를 발화 ID와 연결하여 역사적으로 보존**한다는 점은 별도 검증이 필요하다.

- **기억 내용의 선행 원리**: DualMem(facts+insights).
- **선택적 조회의 선행 원리**: SAM(optional Memory Tool, raw trace retrieval).
- **과거 생각을 재사용 가능한 지침으로 만드는 원리**: SAMem(state–thought policy).
- **정적 사고 성향 선택**: MRPrompt(persona facets).
- **우리의 문제 후보**: 특정 내담자 `u_t`에 연결된 당시에 확정한 **private motive의 장기 회상과 이후 재해석의 정합성**.

## 6. 실험 변수 분리 — 구현 기술부터 정하지 않기

| 실험 | 독립 변수 | 통제 조건 | 검증 질문 |
|---|---|---|---|
| **E1. Memory content/provenance** | Public-only / Current State / Retrospective Insight(DualMem-like) / Committed Utterance-Linked Motive / Shuffled-motive | **Oracle Retrieval**, 동일 상황/모델/메모리 길이 | 과거 **당시** 자기 의도가 사후 추론보다 추가 정보를 주는가? |
| **E2. Retrieval/access policy** | Always-injected / Every-turn Top-K / Optional Agent Tool / Hybrid(활성+장기) | **동일한 기록/기억 스키마**, 가능한 동일 token budget | Retrieval 실패·누수·행동 효과·지연·비용의 trade-off는? |
| **E3. Temporal reinterpretation** | 후속 상담 개입/새로운 증거를 변경 | **같은 과거 발화와 당시 motive** 보존 | 과거 기록을 소급 수정하지 않으면서 **지금의 해석**을 타당하게 갱신하는가? |

실패 사례: 가짜 동의를 진심으로 오독; 나중의 현재 태도로 과거 의도를 소급 덮어쓰기; 과거 의도를 현재 신념으로 고착; 기억 retrieval 오류; 근거 없는 심리 해석 확신; 내부 기억을 상담사에게 불필요하게 공개.

사람 상담사의 숨은 의도에 대한 ground truth가 없으므로 최초의 planted motive는 **시뮬레이터 내부 설정의 known value**일 뿐 임상적 타당성을 보장하지 않는다. Blind human/expert evaluation은 별도로 수행해야 한다.

## 7. 판정과 다음 문헌 검증
- **확인된 기존 기술**: fact+insight write-time memory(DualMem), raw reasoning trace+optional intent recall(SAM), state–thought Q-memory(SAMem), prompt-internal static thinking facet(MRPrompt), client past feeling/goals(CARS), dialog-driven state(CALM-IT).
- **B4-C = 현재 가설 유지**: 특정 발화 시점에 확정한 과거 private motive의 출처·시간 정합성이 추가 가치 있는가?
- **B4-A = 구현/평가 선택 축**: always / top-K / optional / hybrid 중 무엇이 효과적인지는 연구 문제와 실험 결과에 종속, 구조 우수성 자체를 Contribution으로 고정하지 않는다.
- **B4-T = 신규 하위 가설**: 원래의 의도와 현재의 재해석을 구별하는 것이 상담 내담자 시뮬레이션에서 중요한 실패를 해결하는가? **Gap 검증 전**.
- **추가 선행연구**: 사용자가 정한 2026-first 기준에 따라 더 탐색. 2025의 PRIME, Pre-storage Reasoning, CATCH, Thinking in Character 등은 차기 문헌 라운드에서 검토.

[Track B4 설계 기록](track_b_utterance_grounded_intent_memory.md) · [Gap Register](gap_register.md) · [16편 관련 분석](validation_paper/README.md).
