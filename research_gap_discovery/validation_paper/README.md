# Validation papers

Research Gap 후보를 확인·반박하기 위해 검토하는 논문 원문과 분석 기록. **논문들의 저자 주장을 우리 결론으로 혼동하지 않는다.**

## 초기 4편 (2026-10-29)

| Paper | 성격 | 상세 분석 |
|---|---|---|
| PUPPET (ACL 2026 Main) | 개입→심리상태→발화의 neuro-symbolic 규칙 연계 | [puppet.md](puppet.md) |
| PsychePass (Findings ACL 2026) | Scripted probing + pairwise tournament + reward/RL | [psychepass.md](psychepass.md) |
| DyCoP (AAAI 2026) | 작은 모델에 Memory/Retriever/Planner/Generator | [dycop.md](dycop.md) |
| PsyCLIENT (arXiv v3, 2026-10-22) | 프로필과 미리 정한 행동/참고 발화 궤적 결합 | [psyclient.md](psyclient.md) |

- [comparison_matrix.md](comparison_matrix.md): 근거 기반 직접 비교
- [PDF_SOURCES.md](PDF_SOURCES.md): 원본 문서 출처·해시·버전·원문 배치 상태

> PDF 업로드와 별개로 공식 원문 링크·로컬에 보관한 원본 SHA-256을 기록한다. PDF가 이 폴더에 **실제로 존재하는지** GitHub에서 확인되지 않았다면 업로드되었다고 주장하지 않는다.

## 논문별 검토 템플릿
1. 서지/문헌 버전
2. Problem/Gap(저자 주장)
3. Method(백본, 상태, 기억, 계획, 도구)
4. Experimental setup/controls/baselines
5. Main results(숫자, 표/페이지)
6. 무엇을 입증했는가 vs 입증하지 않았는가
7. 우리 Gap과의 중복·반박·추가 연구 질문
8. 검증 필요 사항 및 참고 논문


## 2026 논문 추가 분석 (2차, 누적 8편)

| Paper | PDF | 분석 | 핵심 검증 |
|---|---|---|---|
| When Clients Stop Following (CARS/STREAMS) | [arXiv:2606.04389](https://arxiv.org/pdf/2606.04389) | [cars_streams.md](cars_streams.md) | CCD 기반 저항 시뮬레이션, strategic RL, 자동평가 mismatch |
| Graph2Counsel | [arXiv:2604.20382](https://arxiv.org/pdf/2604.20382) | [graph2counsel.md](graph2counsel.md) | CPG 기반 합성+SFT, static representation, CoT/MA 비용 |
| TherapyGym | [arXiv:2603.18008](https://arxiv.org/pdf/2603.18008) | [therapygym.md](therapygym.md) | CTRS/safety judge + multi-turn GRPO |
| PsyChain | [Findings ACL 2026](https://aclanthology.org/2026.findings-acl.1831.pdf) | [psychain.md](psychain.md) | 단계·성격·안전성 기반 multi-agent synthesis+8B SFT |

현재 논문 원본 PDF는 이 채팅에 업로드됐으며 **GitHub에는 분석 Markdown을 저장한 상태**다. PDF 자체가 GitHub에 업로드됐다는 뜻은 아니다. 원본 해시/버전: [PDF_SOURCES.md](PDF_SOURCES.md).

비교 요약: [comparison_matrix.md](comparison_matrix.md) / 후보 Gap 판정: [gap_register.md](../gap_register.md).


## Track B Soft Trajectory 관련 추가 검증 (전문 미검토)

[trajectory_related_2026_to_validate.md](trajectory_related_2026_to_validate.md)에 PatientAct, Adaptive Virtual Patient, Evoke-Sim, RESISTCLIENT, Psycheval 등의 **검증 질문과 PDF 링크**를 관리한다. 이 논문들은 현재 분석 완료 8편에 **포함되지 않는다**. 최신 초록·검색 결과의 잠정적 유사성은 해당 PDF의 방법/실험에서 확인해야 한다.


## 2026 논문 추가 분석 (3차, 누적 12편)

| Paper | PDF 원본 | 분석 문서 | 특히 중요한 검증 질문 |
|---|---|---|---|
| MASC | [arXiv:2610.08250v1](https://arxiv.org/pdf/2610.08250v1) | [masc.md](masc.md) | Schedule-ST와 조건부 단계 진행 비교; latent state–utterance matching; 과다한 multi-agent retry |
| CALM-IT | [arXiv:2601.10085v3](https://arxiv.org/pdf/2601.10085v3) | [calm_it.md](calm_it.md) | goal+rapport+emotion+state의 누적 조정, 개인차, OLMo-3.1-32B downstream SFT |
| PATIENTACT | [arXiv:2608.12750v2](https://arxiv.org/pdf/2608.12750v2) | [patientact.md](patientact.md) | 애착 유형별 trust delta, disclosure timing, resistance; 임상 평가 |
| Adaptive Virtual Patient (AVP) | [arXiv:2606.10051v2](https://arxiv.org/pdf/2606.10051v2) | [adaptive_virtual_patient.md](adaptive_virtual_patient.md) | SEM 기반 skill-conditioned disclosure pace, Schedule/Gradual Prompt/State-in-context replay |

[12편 종합 비교](comparison_matrix.md) · [Gap Iteration 4](../gap_register.md) · [Iteration 4 상세 종합](../iteration_04_synthesis.md)

참고: 새 4편의 PDF도 사용자 채팅 첨부본을 근거로 분석했다. **GitHub에는 Markdown 분석과 해시를 기록했으며 PDF 원본 바이너리를 올렸다는 의미는 아니다.**


## 2026 인접 Role-Playing / Agent Memory 연구 4편 추가 (Iteration 5; 누적 전문 검토 16편)

> 기존 **심리상담/시뮬레이션 관련 12편**과 달리 아래 네 편은 **일반 Role-Playing 또는 Agentic Decision-Making** 연구다. 상담 시뮬레이터의 임상 타당성을 입증한 논문으로 간주하지 않는다.

| 논문 | PDF | 분석 | B4와의 비교 |
|---|---|---|---|
| RoleMemo / DualMem | [arXiv:2605.25693v1](https://arxiv.org/pdf/2605.25693v1) | [dualmem_rolememo.md](dualmem_rolememo.md) | 사실 + persona-conditioned insight를 **작성 단계에서 저장**; 당시에 실제로 선택한 발화의 자기 의도와는 구별 |
| SAM | [arXiv:2605.24468v1](https://arxiv.org/pdf/2605.24468v1) | [sam_state_adaptive_memory.md](sam_state_adaptive_memory.md) | 과거 사고·행동·관찰 trace 저장 + 현재 목적별 recall; 특정 상담 발화 이유의 역사적 정합성 미검증 |
| SAMem | [Findings ACL 2026](https://aclanthology.org/2026.findings-acl.722.pdf) | [samem_state_aware_memory.md](samem_state_aware_memory.md) | state–thought Q-memory, reusable decision policy vs 개인적 당시 motive |
| Memory-Driven Role-Playing (MDRP/MRPrompt) | [arXiv:2603.19313v1](https://arxiv.org/pdf/2603.19313v1) | [memory_driven_role_playing.md](memory_driven_role_playing.md) | 정적 scene facet의 thinking_pattern, 단일턴 continuation; 세션 중 의도 기억 생성/갱신 아님 |

**새 Gap 후보:** [B4 — Utterance-Grounded Episodic Intent Memory](../track_b_utterance_grounded_intent_memory.md) · [Iteration 5 종합](../iteration_05_synthesis.md) · [Gap Register](../gap_register.md). 원본 PDF 바이너리는 이 대화에만 업로드되어 있고 GitHub에는 분석 Markdown·SHA-256을 저장함.
