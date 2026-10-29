# CALM-IT — Enhancing Synthetic Motivational Interviewing Dialogue Quality with Conversational Dynamics (2026)

- **원본**: [arXiv:2601.10085v3](https://arxiv.org/pdf/2601.10085v3) · v3 2026-10-13 (arXiv ID 2601로 2026년 최초 공개) · 55쪽 · PDF: 2601.10085v3.pdf
- **상태**: PDF 본문 및 Appendix D.4/D.5와 프롬프트 검토 (2026-10-29).
- **대상**: *양쪽* 에이전트의 rapport·emotion·readiness·goal 변화에 따른 장문 MI 합성 대화.

## 1. 저자의 문제 정의
서로의 심리 상태를 추정/반응하며 긴 상담을 이어야 하는데, 기존 합성 데이터가 개별 턴의 자연스러움에 치우쳐 rapport, 감정, 변화 준비도, 목표의 **공동 진화**를 추적하지 못한다. 단일한 사전 스케줄 대신 실제 대화에서 상태를 갱신한다.

## 2. 방법 (§3, Algorithms 1–2, Appendix A)
- **Client hidden state**: rapport(ρ), emotion(e), change stage(c), short-term goal(g), background & summary. 한 턴의 상담사 개입을 50%의 확률로 시스템-1 프롬프트로 평가(그렇지 않으면 neutral). 그 평가와 개인 배경/신념/과거 이력으로 상태 갱신 후 action과 발화(후보 3개→ranker 선택) 생성.
- **Rapport update**: 개입 결과에 따라 **∆ρ∈[−0.10,+0.05]**; 원 논문 Appendix A3는 신뢰 성향, 권위에 대한 태도, coping, 관계/트라우마 이력 등으로 이 **차이의 방향·크기**를 조절하도록 명시. '개인차에 따라 속도가 달라진다'는 우리 아이디어와 이미 직접 중복.
- **Goals**: overarching session goal과 current conversational goal을 구분하고 매 턴 current goal 조정. 피상적 질문이나 유효하지 않은 전개에서는 이전 stage/목표를 재조정할 수 있는 규칙.
- **Therapist side**: 내담자의 hidden state 직접 관찰 불가; public dialogue에서 내담자 상태를 독립적으로 추정. 반복·정체 감지 후 pivot, 아니면 MI-consistent strategy 선택. 양측이 직접 hidden state를 공유하지 않음.
- **중요:** '초반 하락 후 후반 상승'이라는 **전 구간 수치 목표 궤적을 따로 설정해 추적하는 방식**과 같지는 않음. 하지만 높은 수준의 session goal과 자유로운 개입 조건부 상태 갱신을 이미 결합.
- **설계상의 임의성:** Bernoulli(0.5) appraisals, rapport 하한/상한 등은 10개 pilot에서 비공식 trajectory inspection으로 고름; 실대화에서 개별 state trajectory를 적합/검증하지 않음.

## 3. 실험·표·숫자
- DeepSeek-V3.2 backbone; 추가 **OLMo-3.1-32B** 생성 재현. 686개의 deidentified Reddit-post 기반 상황을 MI profile로 생성.
- 4 frameworks(KMI, CAMI+STAR, CALM-IT, CALM-IT-no conversational dynamics) × 3 target lengths(30/50/100 speaker turns) × 686 = **8,232 대화**. 추가 closure 10 turns.
- **Table 3** (DeepSeek, automated): CALM-IT **Effectiveness 4.45** vs KMI 4.27 / CI-NC 3.67; Goal Alignment **4.73** vs 4.60 / 3.89; Client Consistency **4.92** vs CI-NC 4.79. MITI globals 최고지만 Cultivating Change Talk KMI 4.65 vs CALM-IT 4.72 차이는 **0.07**.
- **Fig. 1**: 30과 100턴 **별도 생성한 대화들의 집계 지표 변화**가 적음. 개별 대화에서 실제 state trajectory를 추적 검증한 결과가 아님.
- **Table 2**: 4명 licensed psychologists 96 blinded transcripts(각 framework-length 8), LLM-human weighted κ 0.56–0.78. 임상 상태 전이 ground truth와의 일치도는 평가하지 않음.
- **Fig.2 §6**: Redirect acceptance **64.28%** vs KMI **59.11%**, CI-NC 52.14%; strongest redirection 이후 change talk +12.40%, sustain talk −8.15%. 저자도 두 역할 agent의 **joint outcome**이지 상담사 자체 효능의 인과효과가 아니라고 명시.
- **Appendix Table D4**: Full Effectiveness **4.45**, no-rapport **4.01**, no-goal **4.12**, static **3.18**; 여러 구성 요소 제거 시 품질 저하.
- **Downstream Appendix D.5/Table D5**: OLMo-3.1-32B를 **CALM-IT 생성 코퍼스의 50%**로 SFT, 10% held-out subset 평가; 원래와 fine-tuned counselor를 **KMI framework**로 운영하여 206 paired tests. **9개 MI 측정 항목 중 5개 개선 유의**: Reflection Quality **+0.502**, Cultivating Change Talk **+0.609**, Partnership **+0.900**, Realignment **+0.180**, Effectiveness **+0.238**. Question Quality **−0.129**, Empathy **−0.087** (비유의), Softening Sustain Talk **+0.131**(비유의), Goal Alignment **+0.002**(비유의). **이미 simulator-generated data→trained counselor transfer 증거 존재**.
- Appendix E: DeepSeek generation inference 평균 CALM-IT 약 8분/dialogue vs KMI 약 2분; 총 generation 계산량 약 1,650 GPU-hours(프로젝트 전체).

## 4. 방법론적 근거의 한계
**Observed**: 명시적 양측 상태·목표 관리가 합성 MI 대화의 평가 지표를 높이고 다른 generation framework에서 학습한 상담 모델에도 일부 전달됨.

**Not established**:
- 내담자 숨은 상태의 **실제 인간 세션 대비 타당성**. 논문이 직접 여러 번 인정.
- 개인차별 신뢰/감정 변화 계수를 실제 상담 기록에서 충분히 검증했는가? 상태 prompt와 heuristic δ-bound 기반.
- 시뮬레이터의 dynamic factor *자체*가 학습 성능을 높였는지: downstream은 CALM-IT 데이터 SFT vs original 모델, **KMI 및 CI-NC 등 여러 synthetic data로 동일 조건 학습시키는 매칭 비교가 아님**.
- 상호작용 중 좋지 않은 개입이 장기 진행 목표를 뒤집었을 때 타당한 실패/후퇴를 보이는가?
- **비판적 관점:** redirect *acceptance*가 높으면 항상 좋은가? 과잉 협조 위험과 충돌할 수 있음; 독립 임상 타당성 확인 필요.

## 5. Track A/B 충돌과 실험 아이디어
- **B1:** *큰 목적/목표 + 개인차에 따른 속도 조절 + 개입에 따라 신뢰 갱신*을 이미 구현. 아이디어의 기본 조합은 강한 중복.
- **B2:** 변동 상태(ρ,e,c,g)와 상대의 불완전한 추정을 이미 분리. MI 기반 장문 시뮬레이션의 강한 baseline.
- **Track A:** 합성→32B 정책 학습까지 시연했으므로 일반 'dynamic client improves counselor learning' 주장은 매우 취약. **matched data-generator-controlled experiments**가 필요.
- **가능한 새로운 좁은 질문:** 명시적 *상위 기대 궤적*을 조건부 제어하면서 **상담 개입이 방해하면 목표를 포기/지연하고 관계를 회복**시키는 방법의 한계가 CALM-IT에서 실제 보이는가? 실험 전까지 미확정.

## 6. 후속 확인
동일 프로필·same counselor interventions에서 reward/progress 목표 vs negative intervention 역방향 효과를 정량 측정; 실제 session-level trajectory vs length robustness; stage 변화와 하위 emotion/trust coherence; 데이터 규모 및 비용 통제한 downstream baseline.
