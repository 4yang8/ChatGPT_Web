# Track B — 기존 방법론의 한계를 개선하는 연구 Gap 탐색

> 2026-10-29 · 상태: **가설 후보 / 기존 실패의 실증 확인 전**
>
> Track B는 **기존 논문과 목표·주제가 겹쳐도 되는** 연구 방식이다. 문제는 단순히 다른 코드를 쓰는 게 아니라, **기존 접근의 중요한 실패 조건을 확인하고 그 실패를 더 잘 해결하는가**이다.

## Track A와의 관계

- **Track A:** 시뮬레이터의 어떤 fidelity가 실제 상담사 학습에 독립적인 효과를 주는가? [Gap Register](gap_register.md)의 A1/G1을 이어감.
- **Track B:** '보다 타당한 동적 내담자'라는 이미 연구 중인 목표를 유지하더라도, **기존 솔루션의 한계**를 진단하고 개선하는 방법론적 접근.
- 두 트랙은 나중에 합칠 수 있다. 예: B에서 특정 실패를 해결한 시뮬레이터가 A에서 실제 상담 정책 학습에 추가 가치를 제공하는지 검증. 그러나 **A가 증명돼야만 B의 가치가 생기는 것은 아니다**.

## B1 — Soft / Adaptive Trajectory Control (현재 핵심 후보)

**사용자 아이디어:** 궤적은 사전 설정하되, 각 턴의 필수 도착점이 아니라 상담 진행의 **큰 틀**로 사용. 동일한 큰 흐름에서도 내담자 성격·신뢰 성향과 상담사 개입 때문에 **변화 속도·진행·후퇴·회복**이 달라지도록 한다.

**방법론적 문제 가설:** 고정 스크립트는 반응 민감도를 해칠 수 있고, 전적으로 자유로운 상태 전이는 일관된 방향/통제를 잃을 수 있다. **이 Trade-off가 기존 논문에서 실제로 관찰되는지 먼저 확인**해야 한다.

- 관련 검토한 2026 논문: [PsyCLIENT](validation_paper/psyclient.md) (사전 행동 궤적), [PsyChain](validation_paper/psychain.md) (단계·긍정 변화 지시), [PUPPET](validation_paper/puppet.md) (규칙 기반 전이), [CARS/STREAMS](validation_paper/cars_streams.md) (CCD 개입 조건부 저항).
- **중복 위험:** PatientAct, Adaptive Virtual Patient, Evoke-Sim은 더 직접적인 비교 대상일 수 있으나 **PDF 전문 미검토**. [2026 검증 목록](validation_paper/trajectory_related_2026_to_validate.md).
- [상세 연구 아이디어·설계 미결사항](track_b_soft_trajectory.md).

## B2 — Stability–Plasticity in Longitudinal Client States (보조 후보)

감정은 빠르게 변하되 개인의 장기 신념, 관계적 신뢰, 기억과 정체성은 일관되게 유지하거나 느리게 변해야 한다는 문제. 상담 개입이 실제로 유효하면 상태가 누적 변화하고, 부적절한 상호작용이면 후퇴할 수도 있어야 한다.

- **확인한 선행:** Graph2Counsel은 심리 그래프를 정적 snapshot으로 두고 장기 업데이트를 후속 과제로 인정; CARS는 단일 세션 범위를 한계로 언급.
- **추가 중복 검사 필요:** AnnaAgent, 2026 Psycheval 등 다중 세션 기억/상태 모델링 연구.
- **실패 진단:** 같은 내담자로 여러 세션을 진행하며 무근거 급격한 성격 변동, 새로운 근거를 무시한 상태 관성, 이전 회복/후퇴 기억 누락을 측정한다.
- 성격과 심리 변수의 변화 속도를 임의 숫자로 정해 임상 사실로 주장하지 않는다.

## B3 — State–Utterance Faithfulness (보조 후보)

상태 갱신·계획 기록이 있더라도 그 내용이 최종 발화에 실제로 반영되는지 불분명할 수 있다. Q3a 로그에서 계획 단계에 이미 최종 문장과 같은 발화를 작성한 사례는 **단계 분리의 의미적 효과를 점검할 필요**를 보여준다.

- **중복 위험:** PUPPET도 state→utterance 순서를 명시적으로 사용.
- **실패 진단:** 같은 문맥에서 내면 상태만 변경할 때 적절한 발화 내용/자기개방·저항·말투가 변하는지 확인.
- 단순히 Python workflow 순서와 스키마를 강제한 것이 신규 기여라고 주장하지 않는다.

## 실험 전 연구 결정 원칙

1. 같은 주제의 선행연구가 존재한다는 이유만으로 B를 폐기하지 않는다.
2. '기존 연구에서 다루지 않음'과 '기존 방법의 명확한 실패'는 구분한다.
3. 강한 기존 방법에서도 동일한 실패가 재현되는지 확인한다.
4. 실패 원인에 대응하는 **가장 단순한 방법**을 먼저 설계한다. 27B + Tool/Memory/Agentic/Planning은 검증된 요구가 있을 때만 선택.
5. 개선 범위·통제 조건·평가자의 전문성·안전성·재현 비용을 사전 정의한다.

다음 단계: B1에 대한 **[Soft Trajectory 문제 정의·진단 실험](track_b_soft_trajectory.md)**을 우선 정교화하되, 중복 연구 PDF를 검토한 뒤에도 문제가 남는지 확인한다.


---

## Iteration 4: 신규 4편 이후 B1–B3 재판정 (2026-10-29)

**B1 — NARROW substantially / 직접 중복이 큼.** [PATIENTACT](validation_paper/patientact.md)는 애착에 따른 trust update, [CALM-IT](validation_paper/calm_it.md)는 client profile·과거 경험에 조건부로 trust/emotion/goal update, [AVP](validation_paper/adaptive_virtual_patient.md)는 상담 기술에 따라 disclosure 진행 속도를 조절, [MASC](validation_paper/masc.md)는 motivation/barrier에 조건부 stage transition과 fixed schedule 기준선을 함께 사용. 따라서 '성격·상담사 개입에 따라 궤적 속도가 달라진다'는 기여로 주장할 수 없음.

**B2 — DIRECT OVERLAP, further narrowing needed.** CALM-IT은 multi-dimensional states, MASC는 stable Big Five/turn state, PATIENTACT는 trust+memory를 함께 모델링한다. 단순 장기 일관성·계층적 상태 관리만으로는 부족. 다중 세션의 **가역적 변화와 경험적 전이 타당성**처럼 입증되지 않은 명확한 약점이 필요.

**B3 — DIRECT OVERLAP.** AVP는 State–Utterance Faithful Realization(P3), MASC는 목표 EE/PS/CA→발화→judge+retry를 명시적으로 실험. '검증기가 발화를 일치시킨다'만으로 신규성이 없음. 목표 label의 **정확성**과 target adherence를 혼동하는 실패는 새 연구 질문 후보지만 실증 요구.

**남는 B1의 아주 좁은 가능성**: 사전에 설정한 **상위·장기 방향**을 강제적 결과가 아닌 조정 가능한 **soft prior**로 다루면서, 상담사의 지속적인 부적절 개입에서는 **이유 있는 지연·역행·회복 실패**를 허용하고 개인차별 속도와 진행의 통제성을 동시에 평가할 수 있는가. 이것은 **방법론적 가설**, 아직 직접적인 선행 실패 증명 없음.

다음 작업은 새로운 Q 버전 구현보다 CALM-IT/PATIENTACT/MASC/AVP 공식 구현을 근거로 동일 profile·intervention series의 *paired failure discovery*를 수행할 수 있는지 검토하는 것. 구체 실험: [Soft Trajectory Idea](track_b_soft_trajectory.md).
