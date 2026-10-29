# Track B / B1 — Personality- and Intervention-Conditioned Soft Trajectory

> 생성: 2026-10-29 · 버전: **Idea v0.1 / Research Gap 미확정**
>
> 출처 구분: **[User proposal]** = 연구자가 직접 설명한 설계 의도. **[Hypothesis]** = 이 의도로부터 도출한 검증 가설. **[Optional design]** = 아직 선택하지 않은 방법 아이디어.

## 1. 문제 인식 [User proposal]

1. 내담자의 심리 궤적을 **미리 고정**하면, 내담자가 상담사의 실제 발화에 적응하기보다 사전 궤적을 따라가려는 경향이 강해질 수 있다.
2. 반대로 사전 궤적을 **완전히 제거**하면 대화의 전체 방향, 예상 진행, 실험 설계의 통제성을 잃을 수 있다.
3. 그러므로 사전 정의를 없애지 않고 **상담 과정의 큰 방향만 안내하는 느슨한 궤적(soft trajectory)**을 제시하고자 한다.
4. 내담자의 **성격·신뢰 형성 성향**에 따라 같은 큰 궤적에 도달하는 **속도**가 달라질 수 있으며, 실제 상담사 개입이 그 과정을 변화시킨다.

### 연구자가 든 예시 [User proposal]

큰 틀: **초반 신뢰 하락 → 후반 신뢰 회복**. 같은 큰 틀을 부여해도 상담사에게 마음을 여는 성향이 다른 내담자는 신뢰 변화의 **기울기·시점·소요 턴 수**가 다르게 나타날 수 있다.

이는 **"정해진 턴에 반드시 정해진 신뢰값에 도달"**과 다르다.

## 2. 추가로 제안한 확장 [Hypothesis / Optional design]

**회복이 반드시 일어날 필요는 없는가?** 상담사가 지속적으로 부적절한 개입을 하면 후반에 신뢰가 회복되지 않거나 오히려 후퇴할 수 있도록 하는 것이 심리적 반응성을 지키는 데 중요할 수 있다. 이 설계는 **연구자가 이미 확정한 요구사항이 아니라, 사전 궤적의 강제성을 피하기 위한 논의 후보**이다.

**'성격 차이'를 무엇으로 표현하는가?** 단순한 외향/내향 또는 특정 성격 유형의 단정 대신, 긍정적 개입에 대한 민감도, 부정적 개입에 대한 민감도, 기존 불신의 관성, 관계 손상 이후 회복 속도 등의 **개인차 파라미터**를 검토할 수 있다. 특정 유형별 파라미터는 임상·실증 근거가 없으면 임의의 '사실'로 주장하지 않는다.

**목표 궤적의 역할:** 매 턴 정답 상태가 아닌 (a) 단계별 변화 가능성, (b) 장기적 방향, (c) 허용 가능한 속도 범위 등 중 무엇인지 아직 미정이다. 궤적은 반응 해석을 강제하거나 고품질 상담의 성공을 미리 보장해서는 안 된다.

## 3. 방법론 후보 간 비교 (가설 수준)

| 접근 | 목표 궤적 사전 정의 | 실제 개입 반응성 | 잠재적인 실패 |
|---|---|---|---|
| **Hard Trajectory** | 특정 턴/단계에 상태·행동 고정 | 종속되거나 완화 규칙만 있음 | 프롬프트에 의한 예정된 회복/자기개방 |
| **Free Dynamics** | 사전 목표 거의 없음 | 개입마다 자유롭게 상태 갱신 | 장기 방향 불안정·통제 불가·무근거 상태 변동 가능 |
| **Soft / Adaptive Trajectory** | 큰 범위·방향만 참고 | 개인차·실제 개입이 속도/후퇴/회복을 조건화 | 목표와 반응이 충돌할 때 중재 규칙 필요 |

이 표는 실제 선행연구를 세 범주 중 하나에 일률적으로 배정한 결과가 아니라 **비교 실험을 구상하기 위한 추상적 접근 분류**다.

## 4. 연구 질문 초안

> **How can a client simulator maintain high-level trajectory controllability while allowing personality- and intervention-dependent variations in the pace and direction of psychological change?**

- **RQ-B1a:** 유연한 궤적이 강제 궤적보다 상담사의 개입에 더 조건부로 반응하는가?
- **RQ-B1b:** 자유로운 상태 전이보다 장기적 진행·정체성·상태 일관성이 좋은가?
- **RQ-B1c:** 동일한 큰 궤적에서 내담자의 개인차가 **타당한 변화 속도** 차이로 나타나는가?
- **RQ-B1d (보조):** 개선된 진행·반응성이 실제 상담사 학습에 추가 가치를 제공하는가? 이는 Track A와 접점이나 B1의 필수 정의는 아님.

## 5. 최소 진단 실험 초안 (아직 실행 안 함)

- **공통 컨텍스트:** 동일한 프로필, 초기 신뢰, 과거 경험·대화 이력과 동일한 backbone/생성 조건.
- **조작 1 — 개입 변화:** 공감적 반응 / 중립적 질문 / 성급한 해석 / 반복 압박 등으로 상담사 개입을 바꾼 matched counterfactual 분기.
- **조작 2 — 개인차:** 비교 가능한 프로필에서 신뢰 형성 성향만 다르게 설정; 성격·문화 고정관념이 결과를 규정하지 않도록 독립 전문가 검토 필요.
- **조작 3 — 궤적 제어:** hard, free, soft를 같은 대화 시작점과 가능한 한 동등한 모델·토큰 예산으로 비교. 추가 구조 변경을 최소화.
- **관찰 지표:** intervention responsiveness, trajectory controllability, personality-consistent pace, non-monotonicity, unwarranted positive convergence, state–utterance consistency, 자연스러움, 다중 턴 지속성, 추론 비용.
- **평가:** 적어도 블라인드 전문가 평가와 프로필/상태/개입 조건별 일관성 검사. 전문가 예상 반응은 인간 내담자의 실제 인과적 반사실 Ground Truth와 구별한다.
- **보류 사항:** 샘플 수, 턴 길이, 심리 변수 정의, 평가 rubric, 추론 모델, 기존 baseline 구현 확보 여부는 이후 결정.

**중요한 반증 조건:** 기존 방법들이 이미 개입 변화·개인차에 타당하게 반응하며 soft 제어가 유의미한 추가 이점을 보이지 않는다면, 이 Research Gap은 축소 또는 폐기해야 한다.

## 6. 현재 검토한 문헌과의 관계

| 논문 | PDF로 확인한 사실 | 우리에게 남는 질문 |
|---|---|---|
| [PsyCLIENT](validation_paper/psyclient.md) | 프로필과 사전 행동 레이블/참고 내용 궤적을 사용하고, 상담사 발화와 충돌하면 coherence 우선(§4.3) | 궤적 진행 순서·타이밍을 얼마나 유연하게 바꾸는가? |
| [PsyChain](validation_paper/psychain.md) | Client Speaker에 감정이 점차 좋아지고 최종적으로 합의/감사하도록 지시(Appendix A.3) | 예정된 개선을 실제 개입 효과와 분리할 수 있는가? |
| [PUPPET](validation_paper/puppet.md) | 사전 정의 확률 논리와 상태 머신으로 개입-상태 전이를 계산(§3) | 미지의 개입·개인차에서 규칙 적용 범위는? |
| [CARS/STREAMS](validation_paper/cars_streams.md) | CCD 기반 개입 조건부 저항·감정·의도 변화(§3.1) | 느슨한 상위 목표 궤적과의 통합이 별도 가치가 있는가? |
| [Graph2Counsel](validation_paper/graph2counsel.md) | 고정 CPG snapshot, temporal state updates는 future work(§7) | 심리 구조가 장기 변화하는 경우 통제와 반응을 함께 평가하려면? |

**전문 검토 전·직접 중복 위험:** PatientAct, Adaptive Virtual Patient(AVP), Evoke-Sim, RESISTCLIENT, Psycheval. 제목/초록/일부 공개 소개를 근거로 유사성을 의심하지만 **완전한 메커니즘 중복을 확정하지 않는다**. [추가 논문 검증 큐](validation_paper/trajectory_related_2026_to_validate.md).

## 7. 최종 설계 전 반드시 답해야 할 질문

1. 초반 하락·후반 상승의 '큰 틀'은 내담자가 **반드시 만족해야 하는 목적**인가, 특정 조건에서만 가능한 **잠정 경향**인가?
2. 개입이 궤적과 충돌할 때 개입을 우선하는가? 무엇을 근거로 갱신·보류하는가?
3. 성격/신뢰 성향에 따른 속도 차이를 어떻게 operationalize/validate 하는가?
4. 충분한 진전이 없는데도 생성기가 그럴듯하게 '호전'을 써 넣는 문제를 어떻게 탐지하는가?
5. 기존 방법과의 직접 비교에서 **궤적 통제성과 개입 반응성을 동시에 개선**했다는 근거를 무엇으로 측정하는가?

**연구 방향 상태:** Track B의 유망한 *검증 대상*, 아직 확정된 ACL Contribution이 아니다. 구현 도구는 가설을 확인한 뒤 필요할 때 결정한다.


---

## Iteration 4 추가 검증 — 2026 PDF 전문 4편 결과

**새로운 결론:** '성격·상담사 개입이 궤적 속도에 영향을 준다'는 구성요소 및 간단한 스케줄 대비 adaptive 비교는 **이미 선행연구**다. 연구자의 본래 아이디어는 유지하되, 기본 구성의 신규성 주장은 기각하고 **상위 궤적과 실제 반응 간 우선순위·충돌 해결**이라는 더 좁은 가능성을 검토한다.

| 2026 논문 | 전문에서 확인된 중복 | 아직 열려 있는 것 |
|---|---|---|
| [PATIENTACT](validation_paper/patientact.md) | anxious/avoidant/disorganized별 trust update 속도·방향 조절, 기억 공개·저항 연계 | 별도 사전 상위 목표 궤적과 개입 충돌 시 조율, 더 긴 반복 rupture/repair |
| [CALM-IT](validation_paper/calm_it.md) | session goal과 매턴 갱신 목표, 개인사/성향에 조건부 Δrapport, 감정·변화준비도 갱신 | 실제 세션 trajectory의 외부 타당성과 soft target을 주었을 때의 trade-off |
| [MASC](validation_paper/masc.md) | 사전 initial/final stage, receptivity prior, motivation/barrier에 조건부 stage transition, 고정 스케줄 baseline | 역방향 stage transition과 잘못된 target 교정, counterfactual의 타당성 |
| [AVP](validation_paper/adaptive_virtual_patient.md) | skill-conditioned 누적 progression, 고정 스케줄/gradual prompt/state-in-context를 이미 오프라인 비교 | negative therapist actions에 따른 rollback, 성격별 transition function, 다차원 변화 |

### 설계 후보의 더 엄격한 정의 (아직 제안 수준)

**Macro-trajectory as a revisable prior, not a guaranteed endpoint.** 초기 목표가 '초반 경계→점진적 신뢰 회복'인 경우에도, 실제 상담이 불성실하거나 공격적이면 회복을 보류·실패시키고 그 판단 근거를 유지하도록 한다. 반대로 맞춤형 적절 개입은 일부 내담자에게 빠른 회복, 다른 내담자에게 느린 회복을 허용한다.

- **Controllability**: 대화 주제/허용 단계/핵심 이벤트의 진행을 어느 정도 유도할 수 있는가? 목표 신뢰값 매턴 일치율을 곧바로 좋은 지표로 삼지 않음.
- **Intervention responsiveness**: 동일한 hidden state·profile에서 개입을 바꾸었을 때 불합리한 과잉 순응 없이 적절하게 변화하는가?
- **Personality-conditioned pace**: 성격별 기울기가 무작위 고정관념이 아니라 근거 있는 개인차를 구현하는가?
- **Recoverability**: 신뢰 훼손 후 적절한 repair에 응답할 수 있고, 충분한 repair가 없으면 불필요한 호전이 없는가?
- **Failure to progress when justified**: 문제 해결이 늦어져도 시뮬레이터가 잘못된 목표를 강제하지 않는가?

### 매우 중요한 실험 중복 경고

AVP의 Appendix D.1은 **SCHEDULE / SVP-GRADUAL / STATE-IN-CONTEXT / SVP-MATCHED / AVP**의 *5-arm replay*를 이미 사용했다. MASC에도 hard schedule 기준선이 있다. 그러므로 새로운 논문의 방법/실험이 **단순 Hard-vs-Free-vs-Soft 3-way 비교로 끝나면** 기존 연구와 직접 겹치고 약하다.

### 제안되는 failure discovery (새 구현 강행 전)
1. 같은 내담자와 macro 목표를 지정한 뒤, 적절한 개입·장기간 부적절한 개입·초반 부적절→후반 repair 등 **3개 intervention sequences**를 통제한다.
2. PATIENTACT·CALM-IT·MASC·AVP에서 지원 가능한 의미에 맞춰 동일 조건을 재현; 각 논문에서 **애초에 다루지 않는 차원**은 '실패'로 기록하지 않고 평가 범위 밖으로 명시.
3. 동일 목표를 지키려다 내담자가 **불합리하게 조기 진전하는지**, 반대 방향 개입을 받고 **적절히 후퇴하는지**, 반복된 repair 이후 속도 차이를 보이는지 기록한다.
4. 기존 강한 방법들이 이미 문제를 해결하거나 이 시나리오의 임상적 타당성 기준을 만들 수 없다면 B1을 축소/폐기한다.

**다음 문헌 위험:** Evoke-Sim, RESISTCLIENT, Psycheval 및 2026년 실제 상담에서 therapist-client dynamics를 추정한 연구. 선행연구 12편만으로 '조정 가능한 macro prior'가 분야 전체에 없다고 확언하지 않는다.
