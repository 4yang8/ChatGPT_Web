# MASC — Multi-Agent Self-Calibration with Latent Construct Alignment (2026)

- **원본**: [arXiv:2610.08250v1](https://arxiv.org/pdf/2610.08250v1) · 최초 공개 2026-10-06 · 29쪽 · PDF: 2610.08250v1.pdf
- **상태**: 업로드 PDF 본문·표·부록 검토 (2026-10-29). 독립적 재현 실험 전.
- **대상**: MI (Motivational Interviewing) 내담자 시뮬레이터 + **CRPC-Bench**.
- **관련 Gap**: B1(궤적 통제와 실제 개입), B2(상태/성격 일관성), B3(상태–발화 일치).

## 1. 저자가 주장하는 문제
정적 profile prompting은 장기 대화에서 persona drift, 지나친 순응, 심리적 상태·대화행동·감정 불일치를 발생시킨다. 턴별 일관성 실패를 탐지·수정하는 장치와 session-level/turn-level의 통합 평가가 필요 (§Introduction).

## 2. 방법의 정확한 흐름 (§Method, Fig.1, Algorithm 1)
1. **Session constraints**: profile, 동기·신념·acceptable plan, Big Five(High/Low), 초기/최종 change stage, receptivity(1~5).
2. **Latent Construct Discriminator**: 현재 대화에 근거해 기대하는 **emotion(7), psychological state(Precontemplation/Contemplation/Preparation), communicative action(12)** 세 레이블을 먼저 계산.
3. **상태 전이 규칙(Equation 3)**: Precontemplation→Contemplation은 상담사가 목표 동기를 충분히 다룰 때, Contemplation→Preparation은 믿음 장벽이 해소될 때만 진행. 그 외에는 현재 단계 유지. 제시된 규칙은 **앞으로 가거나 정체하는 형태**이며 명시적 역방향 stage transition이 없다.
4. **Receptivity-aware action prior**: 1~5 receptivity 수준에 따라 premature Acknowledge/Accept/Plan 억제. 개인차는 **행동 분포를 조정**하는 역할이며, 성격별 연속 상태 전이 속도/기울기를 직접 학습한 것은 아니다.
5. **Multi-Agent Debate**: 생성 에이전트 3개, 시도당 5단계 토론, 최대 3회 시도.
6. **Voting Calibrator + Retry Memory**: 3개의 judge가 EE/PS/CA 레이블을 투표, target과 다른 차원을 메모리에 넣어 재생성. 실패 후 최대 2개 기록 유지.
7. **CRPC-Bench**: session-level profile/Big Five/receptivity, turn-level emotion/state/action. 생성 내부 레이블을 최종 채점에 그대로 복사하지 않고 공통 evaluator로 재레이블함.

**중요한 구분**: 레이블 매칭은 *시스템이 예측한 target*에 맞춰 생성했다는 의미. target이 실제 인간의 개입 반응으로 맞는지는 별도의 검증.

## 3. 실험·표·결과
- 원천: **38개 MI 프로필**(원래 데이터에서 initial/final state, motivation, beliefs, receptivity 등); Big Five/감정 추가 주석.
- baseline 7개: Base, Example, Profile, ProAct, Pro+Dial, Consistent Client Simulation(CSF), **Schedule-ST**. Schedule-ST는 **턴 1–10, 11–20, 21–30**에 각기 Precontemplation/Contemplation/Preparation을 강제로 배정. **이미 hard vs conditioned progression을 부분 비교한 선행**.
- 모델: MASC(Homo) Qwen-plus/turbo/max; Hetero ChatGPT-5.5, Minimax-M3, DeepSeek-v4-pro. Hetero 우위에는 **모델 자체 차이도 혼재**.
- **Table 1**: Hetero Profile PE/BE/MO/BL/AP = **77.68/98.71/79.74/78.95/65.91%**. CSF의 AP **68.51%**로 Hetero **65.91%**보다 높음(완전한 전 항목 우위 아님). Hetero Avg Rec **3.34**, MR@20 **47.13%**, Avg MS **19.55** vs Real 3.27/48.39%/27.56.
- **Table 2, 낮을수록 좋음**: Hetero Action/Emotion/State KL = **0.06/0.27/0.06**; Schedule-ST **0.10/0.33/0.18**, CSF **0.07/0.56/0.13**. 이것은 **개별 상담사의 개입에 대한 올바른 조건부 반응의 검증이 아니라 실대화와의 corpus-level label distribution 차이**.
- **Table 3 (Homo 내부 ablation)**: Full profile average **77.16**, Big Five **0.74**, KL 0.06/0.29/0.07; w/o Debate profile **71.65**, w/o Voting+Memory **73.68**, w/o Memory **74.62**. Voting과 Memory 결합 제거로 **분리 인과효과 파악 불가**.
- 73.97%의 턴이 예측 EE/PS/CA 세 목표를 전부 충족; **이 중** 55.81%가 첫 시도, 31.04% 두 번째, 13.15% 세 번째 시도(전체 턴 대비 비율로 읽지 않음).
- 참고: App. E.15–E.17은 profile-only의 **조기 계획 제시** vs MASC의 점진적 변화 비교 사례.

## 4. 입증/미입증과 비판적 검토
**Observed**: 명시 target→생성→투표→수정 흐름은 프로필/정서/행동의 목표 일치, 실제 MI 코퍼스와의 주변 분포 유사성을 개선. 고정 스케줄보다 양호한 지표.

**Not established**:
- **실제 사람의 반사실적 조건부 행동 타당성**, 새로운 상담 개입 시나리오에 대한 변화 속도/후퇴/회복의 정확성.
- 외부 전문가가 평가한 동적 반응의 임상적 원인–결과 정합성(일부 주석 검증은 있지만 본 모델 전체 임상실험은 아님).
- 시뮬레이터 합성 데이터로 상담사 LLM을 학습시킨 후의 성능 전이.
- 3×5×최대3회 후보 생성/투표의 **품질 대비 비용**(구체 단가/시간 분석 미제시).
- **미리 예측된 target 자체가 잘못되면** target을 충실히 재생성하는 loop도 그 오류를 고칠 수 있는가?

## 5. 우리 연구에 대한 판정
- **Track B1:** 가장 직접적인 중복 중 하나: 초기/목표 stage + receptivity + counselor-conditioned transition + fixed Schedule-ST baseline. '고정 시점 대신 변화 조건을 반영했다'만으로 신규성이 부족.
- **Track B3:** 내면 상태–대화행동–정서 3차원 목표 일치/재시도 자체는 이미 구현·검증됨. 단순 checker/retry novelty 약함.
- **남는 후보:** 목표 상태의 정합성/타당성, 역방향 전이·회복 허용, 개인차 기반 transition timing, 실제 개입을 바꾼 counterfactual의 연쇄 반응에 대한 평가.

## 6. 후속 재현 질문
동일 시작 프로필/목표에서 상담사가 의도적으로 유효/무효 개입을 계속할 때, MASC가 '목표 stage 달성'을 강제하는가? 탐색에서 negative/rupture에 따른 stage rollback이 일어나는가? 오분류된 target label을 언제 수정하는가? 비교 지표에 conditional trajectories가 필요한가?
