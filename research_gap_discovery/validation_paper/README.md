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
