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


---

## Iteration 3 — Track A/B 병렬화 및 Soft Trajectory 아이디어 (2026-10-29)

**이전 결론의 보완:** 기존 연구와 **'동적·정교한 내담자'라는 문제 자체가 겹쳐도 된다.** 기존 솔루션의 확인 가능한 한계를 더 나은 메커니즘으로 극복하는 **Track B(Methodological Gap)**를 Track A(새로운 질문/미검증 효과)와 **병렬로** 탐색한다. 방법의 차이만으로 신규성 확정은 불가하고, 기존 방법의 특정 실패를 실험적으로 입증해야 한다.

| ID | 트랙 | 문제·가설 후보 | 선행연구와 겹치는 지점 | 검증 상태 |
|---|---|---|---|---|
| A1 (기존 G1 축소) | A | 개입 조건부 내담자 반응의 fidelity가 데이터량·다양성을 통제한 상태에서 상담사 정책 학습에 **고유한** 가치를 제공하는가? | PUPPET, CARS, TherapyGym, PsychePass, PsyChain, Graph2Counsel | **Keep as hypothesis**, 추가 중복 검토 |
| B1 / BT1 | B | **목표 궤적의 통제성**을 확보하면서 **성격·상담 개입에 조건부로 실제 진행 속도·방향·회복 여부가 달라지는** 유연한 상태 전이 방법 | PsyCLIENT의 사전 행동 궤적, PsyChain의 단계/감정 개선 지시, PUPPET의 사전 규칙, CARS의 CCD 조건부 저항 | **New candidate — Full-text + 실험 확인 전** |
| B2 | B | 빠르게 변하는 감정과 느리게 변하는 신뢰·신념·관계가 다중 턴/다중 세션에 걸쳐 일관적으로 조정되는가? | AnnaAgent(미검토), CARS 단일 세션, Graph2Counsel 고정 CPG | **Unknown — 2026·기존 논문 재검증 필요** |
| B3 | B | 내부 심리 상태의 변경이 최종 발화의 행동 양상에 **실제로 반영되는지** 검증·개선할 방법 | PUPPET 상태→발화, Q3a 단계별 출력 중복 사례 | **Unknown — 단독 ACL 기여는 불확실** |

**B1 사용자 제안 (정확한 취지):** 고정된 시간표처럼 턴마다 강제되는 궤적 대신, 예컨대 '초반 신뢰 하락 후 후반 회복' 같은 **큰 틀의 참고 궤적**을 둔다. 동일한 큰 흐름을 부여해도 내담자의 성격/신뢰 성향에 따라 **진행 속도**가 달라지고, 실제 상담사의 개입이 후퇴·회복을 조절한다. **무조건 후반 신뢰 회복을 강제해서는 안 되는지**는 별도 설계 논점이며, 회복 조건을 충족하지 못하면 회복되지 않을 수 있는 설계는 논의 중인 확장안이다.

**아직 결정하지 않은 것:** 사전 궤적은 목표 경향인가, 허용 상태 범위인가, 단계별 전이 조건인가? 진행이 잘못되면 목표에서 벗어날 수 있는가? 개인차와 효과를 수치로 정의할 근거가 있는가? Soft Trajectory가 PUPPET/CARS/PatientAct/AVP보다 나은 조건이 정말 존재하는가?

### B1의 반증 중심 진단 (제안, 확정 실험 프로토콜 아님)
1. 같은 내담자 프로필·시작 상태·상담 이력을 고정하고 상담사의 개입 유형(공감/중립/성급한 해석/부적절한 압박)을 변화시킨다.
2. **Hard Trajectory / Free Dynamics / Soft Trajectory**를 같은 backbone과 가능한 한 같은 입력·추론 예산에서 비교한다.
3. 동일 개입을 **서로 다른 성격/신뢰 성향**에 제공해 전이 크기·시점·부정/긍정 민감도가 다르게 나타나는지 확인한다. 인간 행동에 관한 성격 일반화는 경험적 근거 없이는 주장하지 않는다.
4. 발화 자연스러움, 반응 타당성, 타당한 방향·속도 변화, 진행 통제성, 불필요한 강제 회복, 상태–발화 일관성, OOD 대응을 **분리 측정**한다.
5. 전문가가 판단한 반사실적 타당성은 실제 인간의 counterfactual ground truth와 다르다. 기존 방법이 해당 조건에서 실패하지 않으면 Gap을 수정/폐기한다.

### 즉시 우선 검증할 새 2026 문헌
- **PatientAct** — 성격/애착 유형별 신뢰 변화와 공개 제어가 Soft Trajectory와 얼마나 겹치는지.
- **Adaptive Virtual Patient (AVP)** — 상담사의 행동에 따라 점진적 자기개방이 달라지는지와 그 전이식/개인차.
- **Evoke-Sim** — 단계/정보 공개 제어와 상담사의 개입 유연성.
- **RESISTCLIENT / Psycheval** — 개입 조건부 저항 및 장기 평가 중복 검증.

**주의:** PatientAct·AVP·Evoke-Sim은 본문 전문 검토 전이며, 선행연구와 '정확히 겹친다'는 결론을 아직 내리지 않는다. 근거 범위와 검토 질문은 [validation_paper/trajectory_related_2026_to_validate.md](validation_paper/trajectory_related_2026_to_validate.md)에 정리했다.

자세한 사용자 아이디어와 제안 설계: [track_b_soft_trajectory.md](track_b_soft_trajectory.md). B 트랙 전체: [track_b_methodological_gaps.md](track_b_methodological_gaps.md).


---

## Iteration 4 — 신규 2026 논문 4편 전문 분석, 누적 12편 (2026-10-29)

직접 검토: [MASC](validation_paper/masc.md), [CALM-IT](validation_paper/calm_it.md), [PATIENTACT](validation_paper/patientact.md), [AVP](validation_paper/adaptive_virtual_patient.md). 요약: [iteration_04_synthesis.md](iteration_04_synthesis.md).

### 갱신 판정

| ID | 4편에서 나온 새로운 실증/중복 | 판정 | 현재 주장 가능한 범위 |
|---|---|---|---|
| **A1 / G1** | CALM-IT이 **OLMo-3.1-32B SFT→KMI 평가 9항목 중 5개 유의 개선**. AVP는 skill-conditioned 진행과 20인 인간 평가. | **NARROW — 여전히 후보** | '동적 시뮬레이터로 학습한다'는 novelty 없음. **동일 학습/데이터 규모·분포, 다른 generator 환경에서 intervention fidelity 요인만 제어**한 learning transfer의 필요조건·효과를 밝히는 질문만 가능할 여지. |
| **B1 — Soft Trajectory** | PATIENTACT의 애착별 trust 속도·손실, CALM-IT의 개인 조건부 rapport/goal update, AVP의 skill→disclosure 속도 및 5-arm schedule 비교, MASC의 counselor-driven stage+receptivity+Schedule-ST. | **NARROW STRONGLY / 강한 직접 중복** | '개인차·상담사 발화에 따른 속도 조절'과 '고정 스케줄 대비 adaptive'만으로 신규성 없다. **macro goal을 강제하지 않는 soft prior와 실제 개입 우선순위의 명시적 조율, 지연/후퇴/회복·회복 실패**의 검증이 남는지 실험 필요. |
| **B2 — 장기 일관성** | CALM-IT 다차원 상태/목표, MASC 세션 수준 성격 및 턴 수준 상태, PATIENTACT trust+memory. | **NARROW STRONGLY** | 다중 세션/다중 state에서의 **가역적 변화/경험적 검증** 등 실제 실패를 특정할 것. |
| **B3 — 상태→발화 정합성** | MASC target label voting/retry, AVP P3 text-state match(0.651 vs 0.311), PATIENTACT state-to-behavior signal. | **BASE CLAIM REJECT / 보조 문제로만 유지** | '상태를 기록하고 결과 발화를 검사한다'는 신규성 없음. target 자체의 오류를 분리해 **표현 일치 vs 전이 타당성**을 평가하는 질문은 가능. |
| **M1 — Method choice** | CALM-IT은 DeepSeek-V3.2 + OLMo-32B, MASC는 모델 조합/반복 voting, AVP는 작은 명시 상태 머신. | **UNCHANGED — Gap 아님** | 27B 및 Tool/Agentic/Memory 선택은 확정한 Gap의 **필요한 기능**에 근거. |

### 현재까지 가장 큰 학술적 위험
- **기존 B1의 중심 메커니즘이 거의 모두 이미 등장**했다. 이들 기능을 단순 조합한 새 시스템은 engineering novelty로 평가될 수 있다.
- 'MASC가 궤적을 조절하지 못한다', 'CALM-IT이 목표를 강제한다', 'PATIENTACT의 persona 반응이 부정확하다'처럼 **실험하지 않은 실패는 사실로 단정하지 않는다**.
- **AVP는 hard schedule을 포함한 5-arm offline replay까지 수행**했다. 기존에 제안한 3-way 실험 설계의 신규성은 약하다.
- **CALM-IT SFT 전이는 이미 존재**한다. Track A는 다른 synthetic generator 조건의 **matched downstream**과 독립 평가를 확보해야 한다.

### 다음 Iteration 목표
1. **B1 Failure Discovery**: 좋은 개입 / 나쁜 개입 / rupture→repair의 동일 프로필 paired sequences로 실제 약점 찾기; 개인차별 speed 민감도와 unjustified convergence를 평가.
2. **공정한 비교**: PATIENTACT와 CALM-IT의 공식 규칙/코드, MASC의 target labels, AVP의 5-arm replay를 반드시 baseline에 반영.
3. **2026 PDF 추가 검증**: [Evoke-Sim, RESISTCLIENT, Psycheval](validation_paper/trajectory_related_2026_to_validate.md), [AVP의 기반 2026 실증 연구](https://arxiv.org/abs/2602.12450), [PatientHub](https://arxiv.org/abs/2602.11684).
4. **A1 유지 여부**: simulator의 conditional state dynamics만 매칭해 바꾼 학습 실험이 이미 보고됐는지 검증.

**중요:** 위 판정은 누적 12편에 한정된다. 현재 가설의 최종 Novelty/Significance 판단을 의미하지 않는다.
