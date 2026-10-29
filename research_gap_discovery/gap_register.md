# Research Gap 후보 레지스터 — 2026-10-29

> 모두 **가설 후보**이며, 이 문서의 현재 검토 대상은 2026 논문 8편이다. 8편만으로도 전체 선행연구의 공백을 단정할 수 없음.

| ID | 후보 Gap | 의미 | 충돌하는 선행연구 | 현재 상태 |
|---|---|---|---|---|
| G1 | 시뮬레이터의 어떤 fidelity 요인이 **downstream 상담사 학습 효과**를 설명하는가? | 단순 '좋은 데이터가 좋다'를 넘어 성분별 인과효과 추정 | PsychePass, PUPPET, PsyCLIENT | **Keep as hypothesis / 검증 필요** |
| G2 | **Intervention-conditioned** 상태 전이가 미리 정의하지 않은 사례에도 타당한가? | 규칙 또는 지정 행동 궤적을 넘는 일반화 | PUPPET, PsyCLIENT, ConsistentMI(추가 확인) | **Narrow / 직접 경쟁 존재** |
| G3 (퇴역) | **독립 Gap으로 다루지 않음** — 27B + Tool/Agentic은 검증할 가설이 아니라, 선택된 Gap을 해결하는 구현 선택지 | Gap과 필요한 능력을 확정한 뒤 선택 여부 검토 | DyCoP, PUPPET 등은 구현 시 참고 | **RECLASSIFIED → M1** |
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


---

## Iteration 2 — 2026-10-29: 2026 논문 4편 추가, 총 8편 재검토

검토한 신규 PDF: [CARS/STREAMS](validation_paper/cars_streams.md), [Graph2Counsel](validation_paper/graph2counsel.md), [TherapyGym](validation_paper/therapygym.md), [PsyChain](validation_paper/psychain.md).

| ID | 이번 검증에서 확인한 직접 중복/반론 | 현재 판정 | 정확하게 좁힌 형태 |
|---|---|---|---|
| G1 | CARS/STREAMS: 동적 저항 내담자와 전략 RL 연결. Graph2Counsel/PsyChain: 구조 기반 합성→8B SFT. TherapyGym: 시뮬레이터→4B GRPO. | **NARROW** | **동일 데이터량·주제·행동 다양성·학습예산·상담사/보상 정책을 통제했을 때, 개입에 대한 반응/상태 변화를 더 잘 모델링하는 시뮬레이터가 미지의 상담사·상황에서 독립적 학습 이익을 제공하는가?** |
| G2 | PUPPET: 전문가 규칙 기반 intervention→state→response. CARS: CCD·trigger 기반 resistance dynamics. | **REJECT as broad novelty** | 명시적 개입-상태 전이 *자체* 신규성 주장 폐기. 제한된 CBT/CCD 범위 밖, 개인별 변화·다중 세션에서의 **정량 OOD 반응 일반화** 가능성은 후속 검증 후보 |
| G3 → M1 | CARS/STREAMS, DyCoP, PsyChain, Graph2Counsel은 다양한 구조의 선행 사례다. | **RECLASSIFIED — Gap 아님** | 연구의 Gap·가설이 정해진 다음 그 가설의 검증에 필요한 기능을 정의하고, 그때 Tool/Memory/Agentic/Planning 사용 여부를 선택한다. 일반적인 도구 우수성이나 비용 우위는 논문의 선결 목표가 아니다. |
| G4 | DyCoP의 Planner 제거 손실 작음. Graph2Counsel은 CoT·MA가 거의 2배 비용 또는 그 이상인데 효과 제한. | **NARROW, method-side** | 특정 복잡성에서 **필요한 플래닝만 하는 구조**가 동일 심리적 응답 타당성에 필요한 호출 수를 줄이는가? 계획 자체의 기여와 노트 작성 변화 반드시 분리 |
| G5 | CARS는 고협조 시뮬레이터의 **평가 왜곡**을 이미 명시. PsyChain의 prompt는 감정 개선·최종 합의를 미리 요구. | **UNKNOWN; no broad novelty** | 시뮬레이터의 평가자가 관찰하는 **개입 민감도/비단조 심리 변화**가 scripted-progress bias 없이 타당하게 측정되는가? 전문가·실제 데이터 기반 평가 지표가 기존 CARS/PUPPET/Psycheval 대비 무엇을 더 구별하는가? |

### 연구 방향에 주는 실질적인 결론
1. '내담자 시뮬레이터를 만들고 그 데이터로 상담 모델 학습'은 2026년만 보아도 여러 논문과 겹치므로 **주 Contribution으로는 부족**하다.
2. '내담자 저항이 상담 개입에 따라 달라진다'는 CARS에서 다뤘으므로 **저항 하나만으로는 신규성 부족**.
3. **Counterfactual Intervention Fidelity**(동일 내담자·히스토리에서 상담 개입만 바꿨을 때 다음 상태·자기개방·저항의 적합성)와 **Downstream Transfer**(동일 양/분포의 훈련에서 훈련된 상담 모델 성능)의 관계를 직접 연결하는 **설명적/인과적 실험**이 후보.
4. 특히 PsyChain의 **scale-matched Combined**는 단순히 데이터 *양*만으로 비판할 수 없게 만드는 대조군이다. 그러나 역할/전문성/safety/단계 지시/생성 분포 전체가 달라지므로 특정 동적 내담자 요인의 독립효과는 입증하지 않는다.
5. Graph2Counsel의 GC+CoT/GC+MA 부정 결과는 **우리 Q3a 필수 플래닝**의 합리성을 미리 긍정할 수 없다는 근거. Q3b의 선택 기준/비용/실제 판단 갱신을 별도 분석할 것.

### 결정적 실험 초안
- **Simulator baseline**: 동일 27B 프로필 prompt-only / +memory & explicit state / +intervention-conditioned state / +conditional planning; same compute budgets. Frontier single-model comparator는 보조축.
- **Counterfactual probe**: 고정 초기 상태+대화 이력; 전략이 다른 3–4개 치료사 개입(공감/성급한 해석/질문/불일치)을 입력. 독립 전문가가 state-change 방향·강도, disclosure timing, resistance appropriateness를 블라인드 평가. 참조 로그가 있으면 경험적 분포 비교도 별도 수행.
- **Downstream factorial**: 동일 counselor-policy generator, topic/seed profile, turns/token/count 및 behavior distribution를 매칭; evaluator/RM, SFT/RL 초깃값·예산도 동일. **상태 전이의 개입 종속성만** 변경한 conditional-vs-shuffled/disabled 대조군 포함.
- **Held-out**: 미지의 profile+문제+전략, 타 시뮬레이터·인간 평가자, 충분한 다중 세션 시나리오. 생성기·judge 공유로 인한 평가 누수 방지.
- **Confound tracking**: state updater, goal-conditioned agenda, counselor generator, preset progress, safety module의 개별 ablation, 총 호출 수/비용, 응답 길이, 데이터 다양성/질.

### 새로운 반증 문헌 조사(2026 최초 공개 확인 필요)
- **RESISTCLIENT**, arXiv:2604.10507 — CARS가 직접 언급한 resistance-informed motivation reasoning.
- **Psycheval**, arXiv:2601.01802 — multi-session & multi-therapy eval을 다뤄 G5와 충돌 가능.
- 2026 논문 중 simulator fidelity factor의 matched downstream 비교가 실제 있는지 집중 조사.

**주의:** 현재 위 내용은 논문 8편의 비교에 따른 **가설 상태**이며, 처음 공개일 및 실제 2026 논문 선행 결과의 범위를 추가 검증하기 전에는 "새로운 Research Gap 확정"으로 표현하지 않는다.


## Methodological Decision M1 — 27B + Tool/Agentic (G3에서 재분류, 2026-10-29)

**연구자가 명확히 한 의도:** 27B급 모델 사용은 연구 수행의 기본 방향이지만, Tool/Agentic/Memory/Planning 자체의 우수성, 중요성, 비용 절감을 입증하는 것이 연구 목표는 아니다. 이 구성 요소들은 **선행연구 검토 → Research Gap 정의 → 가설과 필요한 능력 명세**를 마친 뒤, 필요하다고 판단될 때에만 선택하는 **구현 방법론**이다.

순서: (1) Gap 확인 → (2) 그 Gap을 검증할 가설/평가 설계 → (3) 기능적 요구사항 도출 → (4) 가장 단순한 구현부터 시작하여 필요시 Tool/Agentic 도입 → (5) 연구 가설을 검증.

따라서 계획·기억·도구의 ablation은 그 기능의 선택이 논문에서 주장하는 기여에 실제로 필요한 경우에 한해 실시한다. **'27B+Tool이 프론티어 모델보다 더 낫다'는 사전 가설을 강제하지 않는다.** 비용/효율 지표도 선택한 Research Gap에 관련될 때에만 보조 증거로 제시한다.

앞선 기록의 G3 표기는 **후보에서 철회된 역사적 표기**이며 앞으로는 M1로 부른다.
