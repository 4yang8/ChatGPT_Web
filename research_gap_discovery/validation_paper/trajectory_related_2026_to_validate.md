# Soft Trajectory 관련 2026년 추가 논문 — 검증 큐

> 2026-10-29 · **PDF 전문 검토 전 / 선행 연구 중복 여부 미확정**
>
> 목적: [Track B Soft Trajectory](../track_b_soft_trajectory.md) 가 실제 Methodological Gap인지 반증을 우선 탐색한다. 현재 분석이 완료된 8편과 아래의 **아직 미검토 논문**을 혼동하지 않는다.
>
> 사용자의 검색 조건: **2026년 처음 공개된 논문을 먼저 확인**. 일부 학회판의 최초 프리프린트 날짜는 별도로 검증해야 한다.

## 우선 확보할 원문

| 우선 | 논문 | 2026 PDF/링크 | 무엇을 집중 검증할까? | 상태 |
|---|---|---|---|---|
| 1 | PatientAct | [arXiv:2608.12750](https://arxiv.org/abs/2608.12750) | 성격/애착 유형에 따른 신뢰 변화 속도, resistance, 자기개방 임계값, 목표 궤적 강제 여부 | **전문 업로드 필요** |
| 2 | Empirically Grounded Behavioral Adaptivity for LLM-Based Virtual Patients (AVP) | [Findings EMNLP 2026](https://aclanthology.org/2026.findings-emnlp.1349.pdf) | 상담사의 공감/탐색 등이 자기개방에 미치는 경험적 전이, 개인차와 시간 경과, 속도 제어 여부 | **전문 업로드 필요** |
| 3 | Evoke-Sim | [2026 LuhMe](https://aclanthology.org/2026.luhme-1.23.pdf) | 상담 단계·정보 공개 제어와 실제 개입 반응의 동시 보장 여부 | **전문 업로드 필요** |
| 4 | RESISTCLIENT — Beyond Compliance | [arXiv:2604.10507](https://arxiv.org/abs/2604.10507) | 저항에 관한 근거·동기 기반 추론, 성격/개입 반응과의 중복 | **전문 업로드 필요** |
| 5 | Psycheval | [arXiv:2601.01802](https://arxiv.org/abs/2601.01802) | 다중 세션에서 신뢰 및 관계적 변화, trajectory controllability 평가 범위 | **전문 업로드 필요** |

**주의:** 위 표는 앞선 문헌 탐색에서 확보한 제목/링크/잠정 설명에 기반한 **다운로드 목록**이다. 논문의 방법·실험이 실제로 무엇을 구현하고 검증했는지는 PDF를 읽기 전까지 확정하지 않는다.

## PDF를 받을 때마다 확인할 비교축

1. 사전 궤적 유무와 강제 수준: **목표 시간표 / 단계별 가이드 / 기대 범위 / 전혀 없음**
2. 내담자별 차이: 애착·성격·신뢰 성향과 업데이트 기울기/관성/반응의 관계
3. 상담사 개입 반응: 같은 초기 상태에 다른 개입을 주는 통제 실험이 있는가?
4. 상태 변화의 동역학: 회복을 강제하는지, 악화·정체·상반된 반응이 가능한지
5. 실험 결과: 전문가 라벨/실제 데이터 참조/단순 LLM judge 및 임상 검증 범위
6. 일반화: 처음 보는 개입, 기존 규칙 밖 시나리오, 다중 세션
7. downstream 전이: 데이터의 양·다양성을 통제한 내담자 모델 요인별 학습 비교 유무
8. cost/implementation: 시스템이 복잡하면 그 이유가 연구 가설에서 도출됐는가?

## 기존 8편에서 본 핵심 관련 메커니즘

- [PsyCLIENT](psyclient.md): 사전 행동 궤적 + 충돌 시 맥락 우선
- [PsyChain](psychain.md): 단계·성격 + 감정의 점진적 긍정 변화 지시
- [PUPPET](puppet.md): 규칙·확률 상태 머신
- [CARS/STREAMS](cars_streams.md): 상담 개입·CCD 조건부 저항
- [Graph2Counsel](graph2counsel.md): 고정 CPG, 동적 갱신 미구현

다음 행동: PDF 전문 업로드 → 각 논문별 MD 심층 분석 → [Gap Register](../gap_register.md)의 B1 유사성·차별성·실증 조건 재판정.


---

## 검토 현황 갱신: 2026-10-29 Iteration 4

- **PATIENTACT**: [전문 분석 완료](patientact.md). 애착 유형별 신뢰 증감 폭/속도, trust-gated memory, 다양한 저항 행동을 **이미 구현**. '성격별 속도' 자체 신규성 기각.
- **Adaptive Virtual Patient (AVP)**: [전문 분석 완료](adaptive_virtual_patient.md). 개입 skill과 누적 개방 단계, schedule 및 gradual prompt 등 5-way replay **이미 비교**. 다만 단조 진행·두 persona·오프라인 재생 한계.
- **MASC**: [전문 분석 완료](masc.md). fixed Schedule-ST baseline, receptivity-aware action prior, counselor-conditioned stage transition과 judge/retry 이미 제공.
- **CALM-IT**: [전문 분석 완료](calm_it.md). 인물 배경·관계 과거에 따른 Δrapport, 상위 session goal+현재 goal, 상담사 개입에 따른 다차원 상태 갱신, OLMo-3.1-32B SFT 보고.

앞서 표의 PATIENTACT/AVP의 '전문 업로드 필요'는 **작성 당시의 기록**이며 현재는 분석 완료. 아직 전문 검토가 필요한 2026년 논문은 Evoke-Sim, RESISTCLIENT, Psycheval, 추가로 AVP가 참조한 [Empirical Modeling of Therapist-Client Dynamics](https://arxiv.org/abs/2602.12450) 및 [PatientHub](https://arxiv.org/abs/2602.11684) 등이다. 이 링크는 이번에 읽은 PDF References에서 추출했으며 **추가 논문의 내용은 아직 검토하지 않았음**.
