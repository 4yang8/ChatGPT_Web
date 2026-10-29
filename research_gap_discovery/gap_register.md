# Research Gap 후보 레지스터 — 2026-10-29

> 모두 **가설 후보**이며, 네 편만으로 선행연구 전체의 공백을 단정할 수 없음.

| ID | 후보 Gap | 의미 | 충돌하는 선행연구 | 현재 상태 |
|---|---|---|---|---|
| G1 | 시뮬레이터의 어떤 fidelity 요인이 **downstream 상담사 학습 효과**를 설명하는가? | 단순 '좋은 데이터가 좋다'를 넘어 성분별 인과효과 추정 | PsychePass, PUPPET, PsyCLIENT | **Keep as hypothesis / 검증 필요** |
| G2 | **Intervention-conditioned** 상태 전이가 미리 정의하지 않은 사례에도 타당한가? | 규칙 또는 지정 행동 궤적을 넘는 일반화 | PUPPET, PsyCLIENT, ConsistentMI(추가 확인) | **Narrow / 직접 경쟁 존재** |
| G3 | 27B 도구·상태 기반 시뮬레이터가 프론티어 모델에 비해 **품질–비용 Pareto 개선**이 있는가? | 모델 규모보다 통제·효율이 중요한지 판단 | DyCoP, PUPPET | **Unknown / 강한 비교 요구** |
| G4 | 매 턴 강제 계획 vs 선택 계획이 **실제 판단 갱신**에 영향을 주는가? | 형식적 단계 수행과 실질적인 판단 개선 분리 | DyCoP; Q3a/Q3b는 노트 조건도 다름 | **Narrow / ablation 필요** |

## G1의 핵심 가설 예시
자연스러움과 분포적 다양성이 유사한 두 시뮬레이터라도, 상담 개입에 **조건부로 적절하게** 반응하는 쪽이 생성한 대화는 특정 상담 능력(예: rupture repair, disclosure pacing, memory-grounded responsiveness) 학습에 추가 가치를 제공할 수 있다.

**검증해야 할 반론**
1. 데이터의 양·다양성 차이 때문이지, 개입 조건부 심리 반응 때문이 아닐 수 있다.
2. 프론티어 모델이 작은 모델+도구보다 단순하고 성능이 높을 수 있다.
3. 시뮬레이터가 강화한 가상 규칙을 상담사 모델이 과적합하여 독립 평가에서 저하될 수 있다.
4. 시뮬레이터와 평가기가 동일한 규칙을 공유하면 자기평가·평가 누수가 발생할 수 있다.

**최소 실험 방향(초안)**
- 동일 프로필·초기 상태·대화 이력에서 상담 개입만 변형한 counterfactual 쌍
- 프론티어 단일 프롬프트, 27B 단일 프롬프트, 27B 상태+기억, 27B 선택적 계획, 스크립트/룰 기반의 대조군
- 시뮬레이터 평가: state–utterance consistency, trajectory response, novelty, disclosure/resistance appropriateness, cost
- 상담사 학습: 동량/매칭 데이터, 동일 학습 모델/예산, held-out profiles/strategies, 독립 평가자
- 사전 정한 기준으로 평가; 점수 하나로 임상 치료 효과를 주장하지 않음

## 다음 우선 검증 논문
AnnaAgent, Consistent Client Simulation for MI, Patient-Ψ, Roleplay-doh, CARE-Bench, ClientCAST, Ψ-Arena, ESC-Eval, 관련 synthetic-data quality vs downstream transfer 연구. **읽기 전에는 어느 논문도 Gap을 해결하지 않았다고 주장하지 않는다.**
