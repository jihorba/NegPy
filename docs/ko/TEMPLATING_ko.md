# 파일명 템플릿

> [marcinz606/NegPy](https://github.com/marcinz606/NegPy) `docs/TEMPLATING.md`(0.59.0 기준)의 한글 번역입니다. 원문과 다를 경우 원문이 우선합니다.

NegPy는 **Export**와 **Scan** 사이드바 양쪽에서 동적 파일명을 만들 때 **Jinja2**를 사용합니다.

---

## Export 사이드바

### 사용 가능한 변수

| 변수 | 설명 | 출력 예시 |
| :--- | :--- | :--- |
| `{{ original_name }}` | 원본 파일의 기본 파일명(확장자 제외). | `DSC0123` |
| `{{ colorspace }}` | 익스포트 대상 색공간. | `sRGB`, `Adobe RGB` |
| `{{ format }}` | 익스포트 파일 포맷(JPEG, TIFF, …). | `JPEG`, `TIFF` |
| `{{ paper_ratio }}` | 선택한 화면비. | `3:2`, `Original` |
| `{{ size }}` | 인화 크기(cm 단위). "Original Resolution" 사용 시 비어 있음. | `30cm` |
| `{{ dpi }}` | 익스포트 DPI. "Original Resolution" 사용 시 비어 있음. | `300dpi` |
| `{{ target_px }}` | 목표 긴 변 크기(픽셀). Pixels 모드가 아니면 비어 있음. | `2048px` |
| `{{ border }}` | 테두리 폭이 0보다 크면 "border" 삽입, 아니면 빈 문자열. | `border` |
| `{{ date }}` | 오늘 날짜(YYYYMMDD 형식). | `20260125` |
| `{{ roll }}` | Scanlight 촬영 롤 이름(Metadata → Roll), 또는 `{roll}_Frame{NNN}` 형식의 파일명에서 파싱한 값. Roll Analysis 정규화 이름과는 다름. | `Roll001` |
| `{{ frame }}` | 촬영 프레임 번호(정수), 또는 파일명에서 파싱한 값. 알 수 없으면 비어 있음(`none`). 0을 채운 형태가 필요하면 `{{ frame\|pad(3) }}` 또는 `{{ frame_padded }}` 사용. `"%03d" % frame`은 frame 값이 있을 때만 동작하며, 없으면 패턴 전체가 `original_name`으로 대체됨. | `12` |
| `{{ frame_padded }}` | 0을 채운 프레임 번호(`012`), 알 수 없으면 빈 문자열. `{{ frame\|pad(3) }}`과 동일. | `012` |
| `{{ camera }}` | 카메라 제조사 + 모델. | `Mamiya 7` |
| `{{ camera_make }}` / `{{ camera_model }}` | 카메라 제조사 / 모델 각각. | `Mamiya`, `7` |
| `{{ lens }}` | 렌즈 모델(모델이 없으면 제조사). | `80mm f/4` |
| `{{ lens_make }}` / `{{ lens_model }}` | 렌즈 제조사 / 모델 각각. | |
| `{{ focal_length }}` | 렌즈 초점거리(mm). | `80` |
| `{{ film }}` | 필름 재고(stock) 이름. | `Portra 400` |
| `{{ film_iso }}` | 필름 ISO. | `400` |
| `{{ film_manufacturer }}` | 필름 제조사. | `Kodak` |
| `{{ film_color_type }}` | 필름 색상 종류. | `Color negative` |
| `{{ film_format }}` | 필름 포맷(35mm, 120, …). 익스포트용 `{{ format }}`과는 다름. | `35mm` |
| `{{ developer }}` | 현상액. | `D-76` |
| `{{ dilution }}` | 현상액 희석비. | `1+1` |
| `{{ push_pull }}` | 증감현상 정수값(−3~+3, 0 = 정상). | `1` |
| `{{ development_time }}` | 현상 시간, mm-ss 형식(콜론은 파일명에 쓸 수 없어 하이픈 사용). | `9-30` |
| `{{ development_temperature }}` | 현상 온도(°C). | `20` |
| `{{ scanning }}` | 스캔 방식 메모. | `DSLR copy-stand` |
| `{{ exposure }}` | Metadata에 입력한 노출값 텍스트. | `1/125s f/2.8` |
| `{{ capture_date }}` | 원본 촬영일(YYYYMMDD). 날짜 일부만 있으면 그 달의 1일로 채움. 미설정 시 빈 문자열. | `19980714` |
| `{{ capture_year }}` | 원본 촬영 연도. 미설정 시 빈 문자열. | `1998` |

장비·프로세스 값은 **Metadata** 패널에서 가져오거나, 배치 작업 시 각 파일에 저장된 메타데이터에서 가져옵니다. 필드가 비어 있으면 빈 문자열로 처리되어 주변 구분자도 함께 정리됩니다. NegPy는 메타데이터 값에서 경로에 쓸 수 없는 문자를 제거합니다.

### 예시

| 패턴 | 결과 |
| :--- | :--- |
| `{{ original_name }}` | `DSC0123.jpg` |
| `{{ date }}_{{ original_name }}_{{ colorspace }}` | `20260125_DSC0123_Adobe_RGB.jpg` |
| `{{ original_name }}_{{ size }}_{{ dpi }}_{{ border }}` | `DSC0123_30cm_300dpi_border.jpg` |
| `PRINT_{{ original_name }}_{{ paper_ratio }}` | `PRINT_DSC0123_3:2.jpg` |
| `{{ roll }}_Frame{{ frame\|pad(3) }}_{{ film }}_{{ film_iso }}` | `Roll001_Frame012_Portra_400_400.jpg` |
| `{{ film }}_{{ camera }}_{{ original_name }}` | `Portra_400_Mamiya_7_DSC0123.jpg` |

---

## Scan 사이드바

### 사용 가능한 변수

| 변수 | 설명 | 출력 예시 |
| :--- | :--- | :--- |
| `{{ date }}` | 오늘 날짜(YYYYMMDD 형식). | `20260125` |
| `{{ seq }}` | 순번(정수, 덮어쓰기를 피하기 위해 자동 증가). | `1`, `2`, … |

순번을 0으로 채우려면 Python의 `%` 포맷 연산자를 사용하세요: `{{ "%03d" % seq }}`.

### 예시

| 패턴 | 결과 |
| :--- | :--- |
| `{{ date }}_{{ "%03d" % seq }}` | `20260125_001.tif` |
| `roll_{{ date }}_{{ seq }}` | `roll_20260125_1.tif` |
| `plustek_{{ date }}_{{ "%04d" % seq }}` | `plustek_20260125_0001.tif` |

### 자동 증가

순번은 각 스캔 세션마다 `1`부터 시작합니다. 디스크에 아직 존재하지 않는 파일명이 될 때까지 계속 증가합니다. NegPy는 기존 파일을 **절대 덮어쓰지 않습니다**.

---

## 파일명 정리 규칙

두 사이드바 모두 렌더링된 템플릿에 동일한 구분자 정리 규칙을 적용합니다:

*   변수 사이의 공백, 대시, 밑줄은 **밑줄 하나**(`_`)로 합쳐집니다.
*   맨 앞/맨 뒤의 구분자는 제거됩니다.
*   변수가 비어 있으면(예: 테두리를 설정하지 않은 `{{ border }}`) 그 주변 구분자도 함께 정리됩니다.
*   `{{ original_name }}`(익스포트 전용)은 그대로 삽입됩니다. 원본 파일명의 대시, 공백, 밑줄은 그대로 유지됩니다.

**예시:**
패턴: `{{ original_name }} - {{ border }} - final`
*   테두리 있음: `DSC0123_border_final.jpg`
*   테두리 없음: `DSC0123_final.jpg`
