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


## 2026 논문 추가 분석 (2차, 총 8편)

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
