# PDF 원본 및 출처 추적

> 2026-10-29 기준. 아래 SHA-256은 **사용자가 이 채팅에 첨부한 PDF 바이트**에서 산출한 값이다. 공식 URL에서 재다운로드한 파일의 바이트는 다를 수 있다.
>
> **중요:** 이 문서는 원본 PDF의 존재를 사칭하지 않는다. GitHub 폴더 내 PDF가 실제 업로드되었는지 여부는 개별 파일 링크가 열리는지 확인해야 한다. 직접 업로드하지 못한 경우에는 아래 공식 원문 링크로 제공한다.

| 식별자 | 논문 원본 이름 (채팅 첨부) | 바이트 수 | SHA-256 |
|---|---|---:|---|
| PUPPET | `PUPPET - Neural-Symbolic Standardized Patients for Mental Health.pdf` | 10,331,703 | `05f99ed5687c1aa208d247737695f2acbb028b3ebc3bde430f92188cfac335aa` |
| PsychePass | `2026.findings-acl.1993(1).pdf` | 3,330,814 | `9a4e5da2b796013454f02056f24743e9af8068bf8878ef2bb413e003a5ea6389` |
| DyCoP | `30328-AAAI26.LiuJ-NLP(1).pdf` | 3,719,501 | `324030cd0f413c80402409c9c8a8dcd2c64d769c61ba5ccbc7237152ebe313b5` |
| PsyCLIENT | `PsyCLIENT - Client Simulation via Conversational Trajectory Modeling for Trainee Practice and Model Evaluation in Mental Health Counseling(4).pdf` | 2,986,393 | `2d9e899254f2664899d8159480c2c2562455913c9f1faa2de73e0e1864c7b270` |

## 공식 원문 / 서지 링크
- **PUPPET:** https://aclanthology.org/2026.acl-long.121/ (공식 PDF: https://aclanthology.org/2026.acl-long.121.pdf)
- **PsychePass:** https://aclanthology.org/2026.findings-acl.1993/ (공식 PDF: https://aclanthology.org/2026.findings-acl.1993.pdf)
- **DyCoP:** https://ojs.aaai.org/index.php/AAAI/article/view/40478 (공식 PDF: https://ojs.aaai.org/index.php/AAAI/article/download/40478/44439)
- **PsyCLIENT:** https://arxiv.org/abs/2601.07312 (v3, 2026-10-22)

### 원문 업로드 상태
현재 GitHub 연결 방식은 text/JSON 기반 요청으로 PDF 바이너리 원본을 안전하게 전달할 수 있는 직접적 파일 첨부 API를 제공하지 않는다. 따라서 **원본 PDF가 GitHub에 커밋됐다는 주장은 하지 않는다**. PDF 원본은 사용자의 채팅 첨부 파일로 보존되며, 동일한 버전·바이트 그대로 수동 업로드 후 이 표의 SHA-256과 대조해야 한다.

## PDF 검증 권장
업로드 후 SHA-256을 비교해 원문 무결성을 보장한다. 논문 개정판·학회 버전과 arXiv 버전은 별도 관리하고, 분석 문서에 참조한 버전을 명시한다.


## 2026년 2차 검토 원본 4편 (채팅에 업로드, 2026-10-29)

| 논문 | PDF 첨부 파일명 | 크기(byte) | SHA-256 | 원문 |
|---|---|---:|---|---|
| CARS/STREAMS | 2606.04389v1.pdf | 1,562,185 | 66e3c7827792c9ff386679700cc57d8ad8025f5cdac1b712c58380ad7ba4a3ef | [arXiv](https://arxiv.org/pdf/2606.04389) |
| Graph2Counsel | 2604.20382v2.pdf | 1,510,210 | 66b452724042b8c2357752b6c8bce229f774b48a85f744ef4a48e14d7f02d21d | [arXiv](https://arxiv.org/pdf/2604.20382) |
| TherapyGym | 2603.18008v1.pdf | 1,563,979 | 8d90f8f8e6e4ae65dcfd5c9c6a89a7251f67c2ba8bc6bf2c721df7fc7e9686fa | [arXiv](https://arxiv.org/pdf/2603.18008) |
| PsyChain | 2026.findings-acl.1831.pdf | 2,871,677 | c87548ee90587e490f90abfb773162240ac1d0a6bbd44e78c635a149e4e54815 | [ACL Anthology](https://aclanthology.org/2026.findings-acl.1831.pdf) |

**GitHub PDF 상태:** 원본 바이너리 업로드는 이번 작업에 포함되지 않았다. 분석 Markdown만 GitHub에 직접 저장했고, PDF의 원본 바이트는 현재 대화 첨부 파일에 존재한다.
