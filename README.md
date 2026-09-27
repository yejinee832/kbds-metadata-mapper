# K-BDS metadata mapper

임상 메타데이터와 K-BDS BioSample 양식을 대조하고 **검토용 초안**을 만드는 Codex 스킬입니다. CA19-9 코호트를 출발점으로 만들었으며, 실제 제출이나 포털 업로드를 수행하지 않습니다.

## 무엇을 하나요?

- 임상 Excel의 `mRNA`, `QC_Macrogen`, `AGE`, `SEX`를 확인해 BioSample 행의 일부를 채웁니다.
- QC 통과 행을 초안에 넣고, 실패하거나 중복된 행은 별도 CSV에 기록합니다.
- 입력 근거가 없는 taxonomy ID, organism, biomaterial provider 등은 빈칸으로 남깁니다. 연구자가 확인한 사실만 출처와 함께 설정 파일로 추가할 수 있습니다.
- 프로젝트 설명·연구 목적 같은 **project attributes**와 FASTQ 파일·시퀀싱 방법 같은 **KRA metadata**는 임상표 한 장으로 자동 확정하지 않습니다.

## 구성

| 파일 | 역할 |
| --- | --- |
| [SKILL.md](SKILL.md) | 전체 분류·대조·검토 절차 |
| [scripts/fill_biosample.py](scripts/fill_biosample.py) | BioSample 검토용 Excel 생성 및 행별 감사 CSV 출력 |
| [references/biosample-template.md](references/biosample-template.md) | 입력 화면의 필드와 CA19-9 매핑 한계 |
| [references/ca19-9-example.md](references/ca19-9-example.md) | CA19-9 사례의 구조와 재검증 사항 |
| [agents/openai.yaml](agents/openai.yaml) | 스킬 표시 설정 |

## 사용 준비

Python 3와 `openpyxl`이 필요합니다(예: `python -m pip install openpyxl`). 임상 원본 Excel과 **현재 사용하는 비어 있는 K-BDS BioSample Excel 양식**을 별도로 준비하세요. 화면 캡처는 완전한 업로드 양식이 아니므로 이 저장소에는 양식을 넣지 않았습니다.

확인된 사실을 `facts.json`에 적습니다. `first_data_row`는 실제 양식의 첫 데이터 행에 맞춰 바꾸세요. 나이 단위를 연구자에게 확인하기 전에는 `age_unit`을 넣지 않습니다.

```json
{
  "first_data_row": 4,
  "approved_facts": {}
}
```

예를 들어 연구 문서에서 생물종을 명시적으로 확인했다면 다음처럼 근거와 함께 추가합니다. 실제 확인 전에는 예시 값을 사용하지 마세요.

```json
{
  "first_data_row": 4,
  "age_unit": "years",
  "approved_facts": {
    "Organism": {
      "value": "Homo sapiens",
      "evidence": "확인한 연구 문서와 날짜"
    }
  }
}
```

## 실행

```bash
python scripts/fill_biosample.py \
  --source clinical.xlsx \
  --template biosample_empty.xlsx \
  --config facts.json \
  --output biosample_review.xlsx \
  --audit biosample_audit.csv
```

출력된 `biosample_review.xlsx`는 **검토용 초안이며 제출 파일이 아닙니다.** `biosample_audit.csv`에는 원본 행, 처리 상태, 채운 항목과 빈 항목이 남습니다. 양식의 필수값·허용값, 샘플과 실제 생물학적 시료의 관계, QC 제외 기준을 연구자와 확인한 뒤 포털 검증을 별도로 진행하세요.

CA19-9 예시 파일에서는 91개 임상 행 중 QC 통과 89개 행이 초안에 들어가고 2개 행이 제외되었습니다. 이 숫자는 특정 파일 버전의 시험 결과이므로 새 파일에서는 다시 계산해야 합니다.

## 데이터 관리

환자 임상표, FASTQ 원본, 생성된 BioSample 초안과 감사 CSV는 이 공개 저장소에 올리지 마세요. 공개된 참고 문서에는 사례 설명과 가명화된 샘플 ID 일부가 포함되어 있습니다.
