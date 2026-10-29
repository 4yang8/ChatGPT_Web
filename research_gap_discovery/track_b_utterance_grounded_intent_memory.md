# Track B / B4 — Utterance-Grounded Episodic Intent Memory

> 2026-10-30 · **Working Idea v0.2 — Gap 미확정, 2026 전문 검토 16편과 비교**

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
