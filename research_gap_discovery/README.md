# Counseling Simulator — Research Gap Discovery & Validation

> 상태: 탐색 중 (Working Notes) · 시작: 2026-10-29 · 목표: ACL 논문을 위한 검증 가능한 연구 질문/기여 정립

이 폴더는 **27B급 오픈 모델 + Tool/Agentic 아키텍처 기반 동적 내담자 시뮬레이터**에 관한 연구 방향 논의를 기록한다. 현재의 질문과 Research Gap은 **확정된 결론이 아니라 검증 대상**이다.

## 연구의 큰 목표

상담사의 개입을 내담자 프로필·경험·현재 심리 상태에 비추어 해석하고, 그에 따른 상태 변화가 이후 대화에 누적되는 **Dynamic Client Simulator**를 구현한다. 궁극적으로 시뮬레이터와 상담사 에이전트의 상호작용으로 데이터를 생성하고, 그 데이터로 상담 모델을 학습한 뒤 **독립적인 평가 환경에서의 상담 성능**을 검증하려는 방향이다.

## 지금의 세 가지 고민

1. 프론티어 모델을 사용하는 기존 연구와 비교했을 때 27B급 시스템이 어떤 지표에서 경쟁할 수 있는가? 반드시 모든 단일 지표에서 이겨야 하는가?
2. **27B + Tool + Agentic** 설계의 학술적 필요성은 무엇인가? 단순히 구성요소를 더 붙였다는 Engineering을 넘어 어떤 일반화 가능한 원리를 입증할 수 있는가?
3. 동적 내담자로 합성 데이터를 만들고 상담사 모델을 학습시키는 과정에서 무엇을 새롭게 알 수 있는가? **"덜 정교한/덜 다양한 내담자로 만든 데이터가 덜 좋은 결과를 내는 건 당연하다"**는 반론을 어떻게 분리·검증할 것인가?

세부 기록: [research_questions_and_context.md](research_questions_and_context.md)

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
- [validation_paper/README.md](validation_paper/README.md) — 논문 4편 비교 매트릭스와 논문별 분석 링크
- [validation_paper/PDF_SOURCES.md](validation_paper/PDF_SOURCES.md) — PDF 원본·출처·버전 추적

## 해석 원칙

- **논문의 주장(author claim)**, **실험으로 확인된 사실(evidence)**, **우리의 해석(inference)**을 섞지 않는다.
- "해당 논문에서 다루지 않음"과 "전체 분야에서 해결되지 않음"을 구별한다.
- 새 Gap을 지지하는 문헌만 찾지 않고 **중복·반증 문헌**을 우선 확인한다.
- 시뮬레이터의 발화 자연스러움, 개입-반응 타당성, 장기 상태 일관성, 합성 데이터 다양성, Downstream 학습 효과를 별도 구성개념으로 취급한다.
- 프론티어 비교에는 성능만이 아니라 총 토큰·호출 수·지연·비용·실패율을 함께 검토한다.
- 안전성·임상적 타당성은 독립적인 평가가 필요하며, LLM 시뮬레이터의 결과를 임상 효과로 단정하지 않는다.

> **공개 주의**: 이 저장소는 public이다. 내부 연구 아이디어와 노트가 공개되므로 비공개가 필요하면 저장소 권한을 조정한다.
