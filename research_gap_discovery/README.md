# Counseling Simulator — Research Gap Discovery & Validation

> 상태: 탐색 중 (Working Notes) · 시작: 2026-10-29 · 목표: ACL 논문을 위한 검증 가능한 연구 질문/기여 정립

이 폴더는 **27B급 오픈 모델을 활용한 동적 내담자 시뮬레이션 연구**에 관한 Research Gap 발견·검증 논의를 기록한다. Tool/Agentic 아키텍처는 미리 증명하려는 기여가 아니라, Gap과 연구 가설을 확정한 뒤 필요에 따라 선택할 구현 방식이다. 현재의 질문과 Research Gap은 **확정된 결론이 아니라 검증 대상**이다.

## 연구의 큰 목표

상담사의 개입을 내담자 프로필·경험·현재 심리 상태에 비추어 해석하고, 그에 따른 상태 변화가 이후 대화에 누적되는 **Dynamic Client Simulator**를 구현한다. 궁극적으로 시뮬레이터와 상담사 에이전트의 상호작용으로 데이터를 생성하고, 그 데이터로 상담 모델을 학습한 뒤 **독립적인 평가 환경에서의 상담 성능**을 검증하려는 방향이다.

## 지금의 세 가지 고민

1. 프론티어 모델을 사용하는 기존 연구와 비교했을 때 27B급 시스템이 어떤 지표에서 경쟁할 수 있는가? 반드시 모든 단일 지표에서 이겨야 하는가?
2. **우리가 정의한 Research Gap을 해결하려면 어떤 기능이 필요한가?** 27B급 모델로 구현할 때 Memory·Tool·Planning·Agentic이 필요할지, 단순한 방법으로 충분할지 가설과 실험 설계에 근거해 이후에 결정한다. **이 아키텍처 자체의 우수성을 연구 목표로 설정하지 않는다.**
3. 동적 내담자로 합성 데이터를 만들고 상담사 모델을 학습시키는 과정에서 무엇을 새롭게 알 수 있는가? **"덜 정교한/덜 다양한 내담자로 만든 데이터가 덜 좋은 결과를 내는 건 당연하다"**는 반론을 어떻게 분리·검증할 것인가?

세부 기록: [research_questions_and_context.md](research_questions_and_context.md)

## Research Gap 탐색의 두 트랙 (2026-10-29 추가)

- **Track A — Question/Empirical Gap:** 기존 연구가 충분히 답하지 못한 중요한 질문. 현재 선두 후보는 시뮬레이터의 개입-반응 충실도가 상담사 학습에 제공하는 독립적 가치.
- **Track B — Methodological Gap:** 기존 연구와 **같은 목표(더 정교한 내담자)**를 공유해도 됨. 기존 솔루션에서 검증 가능한 실패 조건을 찾아 새로운 메커니즘으로 개선하는 접근.
- **Track B의 구체적 아이디어:** [Soft Trajectory — 성격·개입 조건부 유연한 심리 궤적](track_b_soft_trajectory.md). 사전 궤적은 상담의 큰 방향이고, 실제 속도·후퇴·회복은 내담자의 개인차와 실제 개입에 좌우되도록 한다.
- [Track B 방법론 후보·Failure Discovery 계획](track_b_methodological_gaps.md) · [추가 2026 문헌 검증 목록](validation_paper/trajectory_related_2026_to_validate.md)

**중요:** Tool/Planning/Agentic은 독립적인 Research Gap이 아니라 **선택된 가설을 구현하기 위한 수단**이다. 궤적 메커니즘 또한 아직 확정 방법이 아니며 기존 방법 대비 실패가 검증되어야 한다.

## Iterative Research Gap Discovery & Validation

```text
선행연구 읽기·비교
      ↓
Research Gap 후보 정의 (아직 검증 전)
      ↓
해당 후보를 반박하거나 이미 해결한 논문 우선 탐색
      ↓
유지 / 축소·수정 / 폐기
      ↓
중요성·신규성·반증 가능성·실험 비용 재평가
      ↺ 반복
      ↓
검증 가능한 연구 질문·기여·실험 설계 확정
```

- [iterative_validation_protocol.md](iterative_validation_protocol.md) — 반복 검증 절차와 판정 기준
- [gap_register.md](gap_register.md) — Research Gap 후보별 증거, 반증 조건, 다음 조사
- [validation_paper/README.md](validation_paper/README.md) — 현재 전문 검토한 2026년 상담·시뮬레이션 12편 및 인접 에이전트 기억 연구 4편 (총 16편)의 비교·분석 링크
- [validation_paper/PDF_SOURCES.md](validation_paper/PDF_SOURCES.md) — PDF 원본·출처·버전 추적
- [iteration_04_synthesis.md](iteration_04_synthesis.md) — 2026 상담·시뮬레이션 12편 기준 Research Gap 검증
- [iteration_05_synthesis.md](iteration_05_synthesis.md) — RoleMemo/DualMem·SAM·SAMem·MRPrompt 4편과 B4 중복 검증
- [iteration_06_memory_access_and_temporal_reinterpretation.md](iteration_06_memory_access_and_temporal_reinterpretation.md) — **기억 조회·주입의 4가지 방식, SAM과 Q3의 유사성, 과거 생각 보존과 재해석의 차이 (Iteration 6)**
- [track_b_utterance_grounded_intent_memory.md](track_b_utterance_grounded_intent_memory.md) — **B4: 발화 당시의 비공개 의도를 다음 턴에 기억하는 설계의 신규성 후보**

## 해석 원칙

- **논문의 주장(author claim)**, **실험으로 확인된 사실(evidence)**, **우리의 해석(inference)**을 섞지 않는다.
- "해당 논문에서 다루지 않음"과 "전체 분야에서 해결되지 않음"을 구별한다.
- 새 Gap을 지지하는 문헌만 찾지 않고 **중복·반증 문헌**을 우선 확인한다.
- 시뮬레이터의 발화 자연스러움, 개입-반응 타당성, 장기 상태 일관성, 합성 데이터 다양성, Downstream 학습 효과를 별도 구성개념으로 취급한다.
- 프론티어 비교에는 성능만이 아니라 총 토큰·호출 수·지연·비용·실패율을 함께 검토한다.
- 안전성·임상적 타당성은 독립적인 평가가 필요하며, LLM 시뮬레이터의 결과를 임상 효과로 단정하지 않는다.

> **공개 주의**: 이 저장소는 public이다. 내부 연구 아이디어와 노트가 공개되므로 비공개가 필요하면 저장소 권한을 조정한다.
