# Track B / B4 — Utterance-Grounded Episodic Intent Memory

> 2026-10-30 · **Working Idea v0.3 — Gap 미확정, 2026 전문 검토 16편과 비교**

## 1. 사용자가 제안한 핵심 문제 [User-proposed]

사람은 대화할 때 자기가 전에 **무슨 말을 했는지**만 아는 것이 아니라, **그 말을 했을 때 왜 그렇게 말했는지**도 기억할 수 있다. 내담자 시뮬레이터는 발화 히스토리를 제공하더라도, 과거 특정 발화 순간에 형성했던 *비공개 해석·의도·망설임*을 다음 턴에 계속 사용할 수 있는지 분명하지 않다.

### 최소 예시
- n=8 Public: "네, 한번 해볼게요."
- Private-A: 실제로 시도할 의사가 있음
- Private-B: 상담사가 실망할까 봐 거절하지 못해 형식적으로 동의함
- n=12 Counselor: "그때 해보신다고 했는데 어떻게 됐어요?"

**공개 히스토리와 현재의 단순 trust score가 동일해도** 과거 motive에 따라 답변은 다를 수 있어야 하는가? *차이가 존재*하는 것 자체가 아니라, **맥락상 유효한 차이·장기 일관성**이 관건.

## 2. 다른 기억 의미와 구분해야 하는 대상

| 구분 | 보존 대상 | 관련 선행 |
|---|---|---|
| Public dialogue history | 무엇을 말했는가 | 거의 모든 상담 simulator |
| Current dynamic state | 지금 얼마나 신뢰/저항하는가 | PUPPET, PATIENTACT, CALM-IT, AVP |
| Previous goal/emotion | 직전 감정·목표/상담사에 대한 평가 | **CARS** `{PLANNING}`, CALM-IT goal/state |
| Persona facts + insights | 관찰 사실에 대해 인물 관점에서 어떤 의미를 구성하는가 | **DualMem**, 강한 직접 선행 |
| General state–thought policy | 같은 상황에서 어떤 생각이 유용한가 | **SAMem** |
| Raw reasoning/action episodic trace | 과거 생각과 행동 기록을 나중에 복구 | **SAM** |
| Static persona thinking pattern | 어떤 환경에서 통상 어떤 사고 패턴을 보이는가 | **MRPrompt** |
| **B4 candidate** | **내담자가 이전 발화 `u_t`를 선택하던 시점에 확정한 비공개 목적 `z_t`**, 과거 발화와 timestamp 링크 | 정확한 과거 motive–later behavior fidelity의 종합 검증은 현재 16편에서 미확인 |

**관건:** 생성 당시 내면 기록과 관찰 발화로부터 **나중에 추론해 생성한 해석**은 서로 다르다. 둘 다 '기억'이라 부를 수 있으나 시간적 출처와 정보 누출이 다름.

## 3. 형식적 개념 (확정 메서드가 아닌 실험용 구분)

상담 내담자 측 비공개 episodic record의 후보:
`E_t = (turn_id=t, public_utterance=u_t, private_appraisal=a_t, motive=m_t, concealed_content=c_t, affect=e_t, expected_response=o_t, provenance=simulated_at_t)`.

- 실제 simulator가 **발화 생성과 같은 턴에 확정·커밋**한 내용만 당시 motive로 기록한다. 나중에 public text로부터 되짚어 생성한 가설은 `retrospective_inference`로 구별하고 uncertainty를 추가한다.
- `E_t`의 역사적 기록은 당시의 관점이므로 추후 변경해서 '원래 그런 생각이었다'로 쓰지 않는다. 반면 **현재의 신념/의도**는 상담사 개입·새 경험으로 갱신돼야 한다. *remembering a belief*와 *still believing it* 구분.
- 이 시스템은 임상의 실제 무의식·마음읽기 ground truth를 측정한 것이 아니라 **시뮬레이터에서 명시적으로 설정한 내적 의도 이야기**를 제어하는 도구다. 실제 사람의 inner narrative는 검증 불가/편향 가능.

## 4. Counterfactual diagnostic test (실행 전 제안)

**조작:** 같은 profile·public dialogue prefix·현재 평가상태에서 단 하나의 **이전 발화 비공개 motive**만 다르게 설정(진심 수락 vs 관계 회피용 형식적 수락). 해당 발화 시점에 'motive'의 내부 상태를 조건부로 생성하고, 공개 문장은 동일하게 제약.

**비교 조건**(최소 5개):
1. **HISTORY:** public dialogue only.
2. **STATE:** public dialogue + current summary/rapport/emotion/goal.
3. **RETROSPECTIVE:** 동일 history + persona-conditioned factual-insight( DualMem-like write-stage 또는 후처리). 해당 insight 작성 시점 별도 로깅.
4. **B4 CONTEMPORANEOUS:** public+current state + utterance-linked historically committed `E_t`, 필요하면 선택 검색.
5. **SHUFFLED/MISMATCHED MOTIVE:** token·memory 양은 같게 두되 `E_t`의 motive를 다른 턴/다른 client와 교환해 인과적 메모리 신호 검증.
6. **optional FULL PRIVATE TRACE:** 각 턴의 공개가능한 구조화 내면 전부. B4의 보존·검색 이점이 단순 토큰 추가 때문인지 평가.

**평가:** 공개 대화가 같아도 motive가 다른 데 따라 **맞는 반응을 생성했는가**(그냥 발화가 달라졌나 X), delayed recall(turn-gap 2/10/30), source/utterance alignment, motive consistency vs later belief update, private-information leakage, self-contradiction, overclaim/hallucination, clinical plausibility, memory token/cost.

**생성/평가 누수 방지:** 같은 모델에서 저장한 motive를 judge가 그대로 reward하면 trivial self-consistency만 평가하게 된다. planted counterfactual은 known *simulated* ground truth일 뿐. 사람 평가자에게 disclosure/rapport/변화가 자연스러운지 separately blinding하고, 다른 모델/전략/대화 주제로 OOD 시험.

## 5. 실패를 먼저 확인할 것

- **Failure 1 — false sincere acceptance:** 과거 형식적인 동의를 현재 진심 어린 의지로 오독.
- **Failure 2 — motive drift:** 지금의 태도를 과거의 내면 의도로 소급해 덮어씀.
- **Failure 3 — spurious retrieval:** 당시 발화와 무관한 상담 에피소드의 private appraisal을 끌어와 현재 판단 왜곡.
- **Failure 4 — stale belief:** 과거 생각을 잘 기억하는 바람에 이후의 실제 변화를 부정.
- **Failure 5 — inferred-motive hallucination:** 근거 없이 추론한 '내가 왜 그랬는지'를 사실처럼 굳힘(DualMem의 insight risk와 SAM wrong frame 사례 참조).

## 6. 가장 중요한 경쟁 논문

- [DualMem](validation_paper/dualmem_rolememo.md): facts/insights를 동시에 저장하므로 개념적으로 가장 강함; **기억 시점의 인물 자기 의도** 차별점 검증.
- [SAM](validation_paper/sam_state_adaptive_memory.md): raw thought/trace 보존과 intent recall 선행; 기억 종류 자체는 새로울 수 없다.
- [SAMem](validation_paper/samem_state_aware_memory.md): state–thought pair 선행; episodic self-representation vs Q-based policy.
- [MRPrompt](validation_paper/memory_driven_role_playing.md): static thinking_pattern/scene facet cue retrieval 선행.
- [CARS](validation_paper/cars_streams.md): thinking/intention+previous planning 유지의 **가장 직접적인 client-domain** 선행.
- [CALM-IT](validation_paper/calm_it.md): 이전 goal/state와 perceived intervention; 이미 충분한 current summary가 있을 때 episodic motive가 추가 이득을 주는지 별도 검증 필요.

## 7. Current decision

**Novelty claim to reject:** "내담자가 생각을 기억하는 최초의 모델", "기억을 사실/해석으로 나누는 최초의 시스템", "state+thought 메모리 최초".

**Provisional B4 research question:**
> *When public utterances conceal distinct internal motives, does preserving the simulator's contemporaneous, utterance-grounded private intent improve long-horizon client continuity and intervention-responsiveness beyond current-state memory and persona-conditioned retrospective inference?*

**검증 조건:** CARS·DualMem-like 강한 baseline을 포함한 paired experiment에서 명확한 실패/개선 근거가 필요. 2025 Role-aware memory/CoT 선행은 추후 추가 검증하며, 단지 상담 도메인에 적용했다는 이유로 ACL 신규성이 보장되지 않는다.

[16편 기준 종합 비교](iteration_05_synthesis.md) · [Gap Register](gap_register.md).


---

## 8. Iteration 6 — Memory content / access / temporal reinterpretation 구분 (2026-10-30)

### 새로 명확해진 연구자 관점
과거 발화 때의 생각을 **기록한다**는 것은 이후 발화 생성에서 이를 **참고할 가능성을 확보**하는 것이다. 모든 턴의 컨텍스트에 항상 포함할 수도 있고, 필요할 때만 검색·도구 호출·주입할 수도 있다. **메모리의 존재와 메모리 조회 정책은 별도 설계 변수**다.

### 세 기능을 서로 혼동하지 않기
1. **Historical preservation (당시 생각 보존):** 과거 `t`턴에 생성한 발화 `u_t`와 **그 시점에 설정된 비공개 생각/의도 `z_t^{then}`**를 원래의 역사적 기록으로 묶어 남긴다. **SAM**의 raw trajectory 보존 원칙이 가장 가까운 일반 선행.
2. **Contextual recall / reconstruction (현재 필요에 따른 검색·정보 재구성):** `k>t`턴의 현재 목표 `q_k`에서 과거 `(u_t,z_t^{then})`의 관련 부분을 찾아 현재 컨텍스트에 전달한다. **SAM**은 `intent→selected page→focused reconstruction` 방식을 이미 구현. 이는 그때의 의도가 **지금 달라졌다는 심리적 재해석**과 동의어가 아니다.
3. **Temporal reinterpretation (과거 경험의 현재 의미 갱신):** 새로운 상호작용으로 현재 내담자가 *당시의 의도를 어떻게 해석하는지* `r_{k←t}`를 새로 생성한다. 과거 기록 `z_t^{then}`은 **그때의 기록으로 보존**하고, 현재의 이해/평가 `r_{k←t}` 및 현재 목표·신념 `s_k`를 별도 관리한다. **DualMem**은 관찰된 사실의 persona-conditioned **write-time** 해석을 저장하는 강한 선행이지만, '같은 과거 사건에 대한 이후의 새 해석과 원래 해석을 동시에 유지'하는 **temporal revision 실험**은 명시적 주목적이 아니다.

**출처 구분:** 위 3번의 명시적 상태/해석 분리와 같은 방법은 **우리의 가설적 설계 후보**이며, SAM·DualMem이 전체 연구영역에서 이를 수행하지 못한다고 확정한 것은 아니다. 사람의 실제 생각도 완전한 불변 원본은 아닐 수 있으며 여기서 '불변'은 **시뮬레이션에서 확정한 당시 내부 상태의 감사 가능한 기록**을 의미한다.

### Memory access 선행의 네 유형

| 선행 | 조회 실행 정책 | 선택 로직·전달 | 상담 B4와의 적용 시 주의 |
|---|---|---|---|
| **DualMem** | 평가 query마다 자동 검색(별도 '검색할지' 정책 없음) | Qwen3-Embedding-0.6B **top-10**, insight가 검색되면 연결된 facts도 동반 | 저장된 insight는 persona-conditioned 해석이지 당시 자기 발화 의도가 아님 |
| **MRPrompt** | 매 응답마다 LTM+STM을 컨텍스트에 제공 | Magic-If 프롬프트가 현재 cue와 맞는 scene facet을 **내부에서 선택**, 외부 retrieval Tool 없음 | static schema; 대화 중 새 episodic private intent의 누적 테스트가 아님 |
| **SAM** | **Agent가 필요할 때 memory tool 호출** | 항상 보이는 compressed cues → agent가 page/recall goal 지정 → memory model이 원본 page에서 관련 내용을 재구성하여 주입 | 과거 사고 기록 보존·조건부 조회 구현과 직접 비슷하지만 상담 발화 당시 motive의 심리적 진실성 보장은 아님 |
| **SAMem** | test 시 **매 decision-step 조회** | 현재 state를 embedding cluster에 매칭, 높은 Q-value `thought` 선택 | 과거 자기 사건기억이 아니라 **재사용할 좋은 판단 방식** 선택. 학습 시 ε-greedy를 사용한다는 것은 test 시 선택적 호출과 다름 |

**대화용 비교 선행**: CARS의 `{PLANNING}` 이전 감정·목표는 기본 입력으로 제공되고, CALM-IT은 요약/현재 goal/emotion/rapport/stage 등을 각 턴 갱신·전달한다. PATIENTACT는 activation tags + trust threshold로 retrieval/disclosure가 제한되며 **읽은 기억을 실제 발화로 공개해야 한다는 뜻은 아니다.**

### Q3와 SAM의 유사성을 어디까지 주장할 수 있나?
- **상대적으로 가장 가까운 것 = SAM의 'optional recall tool / 상황별 과거 정보 이용'이라는 Access 측면.** Q3는 `stage_client_update`, 선택적 `retrieve_client_memory`, `commit_client_utterance`, `update_client_notes`의 흐름을 갖는다.
- Q3 노트가 **모든 발화의 당시 appraisal/motive를 turn_id와 함께 원본으로 보존하고, 이후 정확히 연결해 재조회할 수 있는지**는 아직 확인/검증된 메커니즘이 아니다.
- **기억 내용(Content)**은 DualMem의 fact–insight 관점, **기억 접근(Access)**은 SAM의 selective recall 관점을 참고할 수 있으나, 두 기술을 결합했다는 사실만으로 Method novelty를 주장하지 않는다.

### 실험의 두 축을 분리
- **Experiment 1 — Content / Provenance:** access를 **oracle retrieval**로 고정하고 public history / 현재상태 / 사후 insight / 그 시점 발화별 private motive를 비교(토큰량, 모델, 이벤트 동일).
- **Experiment 2 — Access policy:** content를 동일하게 유지하고 **always-injected / every-turn top-K / optional tool / hybrid recent-active+long-term episodic** 비교. 검색 누락률·오검색·효과·호출/지연/토큰 비용을 분리.
- **Experiment 3 (추가 후보) — Temporal reinterpretation:** 같은 과거 `z_t^{then}`에 대해 후속 상담 개입의 증거만 바꾸고, `r_{k←t}`와 `s_k`가 합리적으로 변화하는지 시험. 과거 기록 자체를 덮어쓰거나(과거 왜곡) 과거 생각을 현재 신념으로 고정하는(경직) 양쪽 실패를 분리 평가.

### 새롭게 제안하는 질문
> **Can a simulated client preserve its originally committed private motive for an earlier utterance while updating its present interpretation of that episode as counseling interactions unfold, without retroactively rewriting the remembered motive?**

이 질문은 B4의 **추가 하위 가설(B4-T)**이며, **확인된 Research Gap이 아니다**. 더 자세한 표와 논문별 근거: [Iteration 6](iteration_06_memory_access_and_temporal_reinterpretation.md).
