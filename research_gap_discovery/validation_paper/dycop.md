# DyCoP — Dynamic Cognitive Planning for Cognitive-Functional Dialogue

**출처:** AAAI 2026, pp. 32069–32077. **검토:** 2026-10-29. **지위:** 분석 초안.

## 1. 저자가 제기한 문제
인지적·심리적 변화를 유도하는 상담/설득/정서지원 대화에서, 단순 표면 발화 생성만으로는 장기 목표를 향한 **전략적 지도와 상대의 변화에 대한 적응**이 어렵다고 주장.

**역할 주의:** 주된 생성 에이전트는 **내담자가 아니라 정서지원 응답 에이전트(상담 측)**이다.

## 2. 제안 방법
Memory(User Profile / Event-State Sequence / Dialogue Agenda) → Retriever(관련 기억 추출·요약) → Planner(상황 분석, local goal, Dialogue Act 기반 multi-level plan) → Response Generator. 한 턴이 지난 뒤 memory/agenda 갱신 (§Method pp. 32071–32073).

모델: **Qwen2.5-7B-Instruct**. 모델 크기가 작아도 구조 기반 응답 품질을 높이는 선행 예다. 별도의 훈련 데이터셋(331 dialogues, 1,966 turns)도 공개·구축했다.

## 3. 실험 / 핵심 비교
동일 기본 모델 사용한 ablation Table 2(총 10개 점수 합계, 3회 평균):
- Baseline Qwen **74.44**
- w/o Memory **76.22**
- w/o Retriever **75.31**
- w/o Planner **77.28**
- Full DyCoP **77.77**

Full 대비 Planner 제거 차이는 **0.49점**으로 제한적(이 수치만으로 유의성이나 필연성 판단 불가). Retrieval 제거 차이는 **2.46점**. 단순히 '모든 단계가 반드시 필요하다'고 결론 내리는 건 주의해야 함.
실험은 주로 **ESC-Role과 5턴** 상호작용, Gemini 2.5 Flash judge, 50개 sample 대조의 전문가 평가 등을 사용한다.

## 4. 입증한 것과 못한 것
**입증:** 7B에서 메모리/검색/계획을 결합하면 해당 emotional support task와 평가 지표에서 성능 개선을 얻는다.
**직접 평가 밖:** 동적 가상 내담자의 intervention-conditioned 실제 심리 변화 타당성, 학습된 상담사 정책에 대한 합성 데이터 전이 효과. 5턴 성과를 긴 다중 세션으로 일반화할 수 없음.

## 5. 우리 연구와 중복/기회
- **중복:** 작은 LLM + Memory + Retriever + Planner는 새 Architecture 그 자체로 주장하기 어렵다.
- **기회:** 도구를 반드시 호출 vs 조건부 호출, 실제 단계별 판단 변경 vs 이미 정한 결론 정당화, 계산 비용 대비 시뮬레이션 정확도.
- **실험 제안:** 동일 백본과 고정 generation 조건에서 plan only / lookup only / combined / adaptive route 및 full vs cost-matched 비교. 이때 **Q3b의 노트 작성 지침 차이**는 함께 바뀌지 않게 분리할 것.

## 6. 추가 조사
DyCoP 결과의 실제 human evaluation confidence interval 및 planner 비용, 역할 반전(내담자)에서의 재현성, 상담 상태 장기 검증 연구.
