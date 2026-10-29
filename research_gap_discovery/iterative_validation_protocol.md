# Iterative Research Gap Discovery & Validation — 작업 규약

## 목적
ACL 논문을 위한 **중요하고 새로우며 실험 가능한 Research Gap**을 찾는다. 하나의 Gap을 먼저 고정하지 않고, 문헌을 읽을수록 **유지 / 좁힘 / 재정의 / 폐기**를 반복한다.

## 반복 절차
1. **선행연구 분석:** Problem, 사용자/내담자 정의, method, backbone, 데이터·설정, baselines, metrics, 수치, 통계, 한계, 저자의 claim을 근거와 분리해 저장한다.
2. **Gap 후보 등록:** '누가 무엇을 어디까지 했는지', '정확히 무엇이 미검증인지', '왜 중요한지'를 1~2문장으로 명시한다.
3. **반증 문헌 탐색:** 이미 해결한 논문, 실험적 반례, 동일 실험을 선행한 논문을 **적극적으로 찾는다**. 특히 각 논문의 related works와 최신 인용 논문을 역추적한다.
4. **Gap 판정:** 
   - **Keep:** 중요성·새로움·검증 가능성을 유지
   - **Narrow:** 선행연구가 이미 넓은 주장을 수행 → 범위를 좁힘
   - **Revise:** 가정·문제 정의를 수정
   - **Reject:** 이미 입증됨 / 중요성 부족 / 분리 검증 불가능
   - **Unknown:** 증거 부족
5. **최소 결정적 실험:** Counterfactual, 동일 기반 모델, paired control, 데이터 양·다양성 매칭, 독립 평가셋, 비용과 실패율 측정 등으로 핵심 가설을 반증 가능하게 설계한다.
6. **반복:** 새 문헌 결과를 [gap_register.md](gap_register.md)에 갱신하고 2~5로 돌아간다.

## 각 주장에 증거 등급
- **Observed in paper:** 논문의 실제 실험으로 확인 (수치와 세팅 필수)
- **Author interpretation:** 저자가 주장·해석 (그 이상의 임상 타당성 함의 금지)
- **Our inference:** 비교로부터 도출한 새 가설 (검증 필요)
- **Open question:** 자료가 없어 판정 유보

## 충분성 체크
- [ ] 선행연구의 직접적 중복을 탐색했는가?
- [ ] 평가할 구성개념이 조작적으로 정의됐는가?
- [ ] 연구의 중요성이 당연한 품질 향상을 넘어서는가?
- [ ] 가설과 무관한 요인(모델·토큰·지연·훈련셋 분포)을 통제할 수 있는가?
- [ ] 긍정/부정 결과 모두에서 배울 수 있는가?
- [ ] 인간 평가·안전성 검토·데이터 출처 문제를 다뤘는가?

## 문헌 관리
`validation_paper/` 아래 논문마다 분석 Markdown을 두고, [comparison_matrix.md](validation_paper/comparison_matrix.md)에서 논문 간 중복을 비교한다. 원문 출처 및 PDF 무결성은 [PDF_SOURCES.md](validation_paper/PDF_SOURCES.md)에 둔다.
