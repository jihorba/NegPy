<div align="center">
  <img src="media/icons/icon.svg" width="96" height="96" alt="NegPy Logo"><h1>NegPy</h1>

  [![CI](https://github.com/marcinz606/NegPy/actions/workflows/ci.yml/badge.svg)](https://github.com/marcinz606/NegPy/actions/workflows/ci.yml)
  [![Release](https://img.shields.io/github/v/release/marcinz606/NegPy)](https://github.com/marcinz606/NegPy/releases)
  [![Downloads](https://img.shields.io/github/downloads/marcinz606/NegPy/total)](https://github.com/marcinz606/NegPy/releases)
  [![License: GPL-3.0](https://img.shields.io/badge/license-GPL--3.0-blue.svg)](LICENSE)
  [![Python](https://img.shields.io/badge/python-3.13%2B-blue)](pyproject.toml)
  [![Ruff](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json)](https://github.com/astral-sh/ruff)
  [![Platform](https://img.shields.io/badge/platform-linux%20%7C%20macos%20%7C%20windows-lightgrey)](#getting-started)
  [![Contributors](https://img.shields.io/github/contributors/marcinz606/NegPy)](https://github.com/marcinz606/NegPy/graphs/contributors)
  [![Discord](https://img.shields.io/badge/discord-join-5865F2?logo=discord&logoColor=white)](https://discord.gg/JySNzUWgwy)
</div>

> 이 문서는 [marcinz606/NegPy](https://github.com/marcinz606/NegPy) 공식 저장소의 `README.md`(0.59.0 기준)를 한글로 번역한 것입니다. 원문과 내용이 다를 경우 원문이 우선합니다.

**NegPy**는 필름 네거티브를 처리하기 위한 도구입니다. 단순한 색 반전 도구를 넘어서, 필름 스캔 전용으로 만들어진 무언가가 필요해서 직접 만들었습니다. 필름과 인화지가 실제로 어떻게 작동하는지를 시뮬레이션하면서도, 랩 스캐너에서 쓰던 편의 기능들도 함께 담았습니다.

**Python**으로 만들어졌고, Linux·macOS·Windows에서 네이티브로 동작합니다.

---

![alt text](docs/media/0500.png)

---

## 사용자 가이드
**[USER_GUIDE.md 읽기](docs/USER_GUIDE.md)** — NegPy 워크플로, 기능, 컨트롤에 대한 완전한 안내서.

---

## 기능

**변환 & 필름 과학**
*   **카메라 프로파일 불필요**: 카메라 프로파일도, 가장자리 색상 추출도 필요 없습니다. 채널 감광도 분석으로 오렌지 마스크를 수학적으로 중화합니다.
*   **필름 물리학**: 밀도 공간에서 **H&D 특성 곡선**(H&D Characteristic Curve)을 모델링합니다 — 단순 선형 반전이 아니라, 독립적인 softplus toe/shoulder 무릎점과 ISO-R 인화지 등급을 가진 비대칭 toe-linear-shoulder 응답 곡선입니다.
*   **스마트 자동 변환**: 프레임별 **Auto Density**와 **Auto Grade**가 각 네거티브를 측정해 적절한 밝기/대비를 잡아줍니다 — 그대로 써도 되고, 세밀하게 조정하기도 쉽습니다.
*   **암실 인화지 프로파일**: Ilford/Kodak/Foma/Fuji 데이터시트에서 매핑한 인화지별 곡선(톤, 채널별 감마, 베이스 틴트)을 롤마다 선택할 수 있습니다.
*   **포지티브/슬라이드 지원**: 유효기간 지나거나 바랜 필름을 살리기 위한 선택적 정규화 기능이 있는 전용 **Transparency 모드**.

**촬영 & 입력**
*   **카메라 스캐닝**: 테더링된 카메라로 네거티브를 NegPy에 바로 캡처합니다 — 단일 RAW 촬영이든, RGB [Scanlight](https://github.com/jackw01/scanlight)로 구동하는 자동 적/녹/청 협대역 3연속 촬영(Trichrome Scan 병합용)이든 가능합니다. macOS/Linux, 선택적 의존성. [카메라 스캐닝 가이드](docs/CAMERA_SCANNING.md)
*   **스캐너 지원**: SANE 호환 필름 스캐너(Plustek, Nikon Coolscan 등)를 직접 제어합니다.
*   **Trichrome Scan**: 한 네거티브를 세 번의 협대역 적/녹/청 노출로 나눠 촬영한 뒤, 서브픽셀 정밀 자동 정렬로 색 번짐(fringing)을 없애며 하나의 저노이즈 컬러 스캔으로 합칩니다.
*   **플랫필드 보정**: 광원이나 스캐너의 조도 불균일/비네팅을, 빈 광원을 찍은 기준 스캔으로 보정합니다. 이름 붙여 저장 가능, 이미지별로 켜고 끌 수 있습니다.
*   **파일 지원**: 표준 RAW/TIFF는 물론, Kodak Pakon 스캐너 RAW 같은 특수 포맷도 지원합니다.

**편집**
*   **Dodge & Burn**: 프리핸드 다각형 마스크로 하는 암실 방식의 국소 밝게/어둡게 처리 — 마스크마다 개별 EV 강도와 페더(feather) 설정. GPU 가속이면서 CPU 결과와 비트 단위로 동일합니다.
*   **먼지 제거**: 그레인을 합성해 자연스러운(플라스틱처럼 보이지 않는) 자동/수동 힐링.
*   **배치 정규화**: 불러온 모든 파일의 범위를 분석해 평균을 내고, 롤 전체에 일괄 적용합니다.
*   **GPU 가속**: Vulkan/Metal을 통한 실시간 처리 및 익스포트 렌더링.

**색상 & 출력**
*   **색상 관리**: 완전한 ICC 워크플로 — 모니터 프로파일 자동 감지(Linux/macOS/Windows), 인화지/프린터 프로파일을 포함한 소프트 프루핑, 이미지별 입력/출력 프로파일.
*   **인쇄 준비**: 인쇄를 염두에 둔 익스포트 — 테두리 컨트롤, ICC 소프트 프루핑, [동적 파일명 템플릿](docs/TEMPLATING.md), **익스포트 프리셋**(저장 후 원클릭), **콘택트 시트**. 지원 포맷: JPEG, TIFF, PNG, WebP, JPEG XL.
*   **Flat / 디지털 인터미디에이트 익스포트**: 카메라의 자체 매트릭스로 RAW를 ProPhoto에 매핑한, Lightroom/Darktable/Photoshop용 플랫하고 중립적인 광색역 **16비트 TIFF** 마스터 파일.

**워크플로 & 데이터**
*   **비파괴 편집**: 원본 파일은 절대 건드리지 않고, 편집 내용은 레시피 형태로 저장됩니다.
*   **데이터베이스**: 편집 내용은 파일 해시를 키로 하는 로컬 SQLite DB에 저장됩니다 — 파일을 옮기거나 이름을 바꿔도 작업 내용이 유지됩니다.
*   **영구 실행취소/다시실행 & 히스토리**: 파일당 최대 100개 편집 단계. **히스토리 패널**에서 모든 단계를 확인하고, 어느 시점으로든 이동하거나 분기하거나 이전 버전을 익스포트할 수 있습니다. 앱을 재시작해도 유지됩니다.
*   **메타데이터 & 장비 라이브러리**: 원본 아날로그 촬영에 대한 아카이브용 메타데이터 — 카메라/렌즈/필름 재고 라이브러리를 관리하고, 프레임별로 장비 프리셋을 적용하며, 실제 카메라/렌즈/ISO EXIF(및 XMP 스캔 태그)를 익스포트 파일에 기록해 Lightroom에서 사용한 필름 장비가 표시되게 합니다. [가이드 보기](docs/USER_GUIDE.md#11-metadata-panel)
*   **단축키**: [여기서 확인](docs/KEYBOARD.md)

---

### 동작 원리

[수식과 처리 파이프라인에 대해 읽어보기](docs/PIPELINE.md)

---

## 시작하기

### 다운로드
사용 중인 OS에 맞는 최신 릴리스를 **[릴리스 페이지](https://github.com/marcinz606/NegPy/releases)**에서 받으세요.

그 이후로는 NegPy가 알아서 최신 상태를 유지합니다: 새 릴리스가 나오면 왼쪽 패널에 **⬇ 업데이트 가능** 링크가 나타나고, 클릭하면 자동으로 다운로드·설치한 뒤 새 버전으로 다시 열립니다. 수동으로 다운로드·삭제·재설치할 필요가 없습니다.

#### Linux
`.AppImage` 형태로 배포합니다. `chmod +x`로 실행 권한을 준 뒤 바로 실행하면 됩니다.

**스캐너 지원**을 쓰려면 시스템에 SANE이 설치되어 있어야 합니다:
```
sudo apt install libsane        # Debian/Ubuntu
sudo pacman -S sane             # Arch
```
또는 배포판에 맞는 동등한 패키지를 설치하세요. 스캐너를 쓸 계획이 없다면 이 단계는 건너뛰어도 앱은 정상적으로 실행됩니다.

**카메라 스캐닝 지원**(선택)은 테더링 촬영을 위해 `python-gphoto2`를 사용하며, 시스템에 `libgphoto2` 설치가 필요할 수 있습니다:
```
sudo pacman -S libgphoto2        # Arch
```
배포판에 맞는 패키지명을 찾아보세요.

#### Nix
다음 명령으로 NegPy를 바로 실행할 수 있습니다:
```bash
nix run github:marcinz606/NegPy
```
또는 자신의 flake에 input으로 추가할 수 있습니다:
```nix
{
  inputs.negpy.url = "github:marcinz606/NegPy";
  outputs = { self, nixpkgs, negpy, ... }: {
    # negpy.packages.<system>.default
  };
}
```

#### 서명되지 않은 소프트웨어 경고
무료 취미 프로젝트라 Apple이나 Microsoft에 개발자 인증서 비용을 내지 않습니다. 그래서 처음 실행할 때 무서운 경고가 뜹니다.

**macOS**:
1.  `.dmg` 파일을 더블클릭하고 앱을 `/Applications`로 드래그합니다.
2.  터미널을 열고 다음을 실행합니다: `xattr -cr /Applications/NegPy.app` (이 경고를 없애줍니다).
3.  실행합니다.

macOS에서 직접 DMG를 빌드한다면, 인텔 빌드는 `NEGPY_MACOS_ARCH=x86_64`, Apple Silicon 빌드는 `NEGPY_MACOS_ARCH=arm64`로 설정하세요.

**스캐너 지원**은 [Homebrew](https://brew.sh/)를 통한 SANE이 필요합니다:
```
brew install sane-backends
```
스캐너를 쓸 계획이 없다면 이 단계는 건너뛰어도 앱은 정상적으로 실행됩니다.

**카메라 스캐닝 지원**(선택)은 `python-gphoto2`를 사용하며, [Homebrew](https://brew.sh/)로 `libgphoto2`가 필요할 수 있습니다:
```
brew install libgphoto2
```

**Windows**:
1. 설치 프로그램을 실행합니다(경고는 무시).
2. 앱을 실행하고 경고 창들을 넘깁니다.

**스캐너 지원 (Plustek OpticFilm)** — 8200i SE와 8100 V2 — 는 선택적 `pyopticfilm` 드라이버를 사용합니다(`uv sync --group plustek` 또는 `pip install negpy[plustek]`). [Zadig](https://zadig.akeo.ie/)로 스캐너를 WinUSB에 바인딩하세요(벤더/SilverFast 드라이버 대체; 8200i SE는 `07b3:1825`, 8100 V2는 `07b3:1824`). Windows 릴리스 빌드에는 pyopticfilm, PyUSB, libusb가 포함되어 있습니다. 자세한 내용은 [docs/PLUSTEK_WINDOWS.md](docs/PLUSTEK_WINDOWS.md) 참고. 카메라 스캐닝은 Windows에서 아직 지원하지 않습니다(libgphoto2에 Windows 빌드가 없음).

---

저장소를 직접 클론해서 빌드할 수도 있습니다. 방법은 여기: [CONTRIBUTING.md](CONTRIBUTING.md)

---

## 데이터 위치
모든 데이터는 `Documents/NegPy` 폴더 안에 있습니다:
*   `edits.db`: 편집 내용.
*   `settings.db`: 마지막으로 사용한 익스포트 설정, 프리뷰 크기 같은 전역 설정.
*   `cache/`: 썸네일(삭제해도 안전).
*   `export/`: 기본 익스포트 위치.
*   `icc/`: 인화지/프린터 프로파일을 여기 넣으세요.
*   `override.toml`: 시작 시 적용되는 재정의 설정 — 아래 [문제 해결 / override.toml](#문제-해결) 참고.

---

## 문제 해결

NegPy가 시작할 때 죽거나 렌더링 문제가 있다면 `Documents/NegPy/override.toml`을 수정하세요. 처음 실행할 때 OS에 맞는 합리적인 기본값으로 자동 생성됩니다. `[performance]` 수치는 **환경설정**(`Ctrl + ,`)에도 있는데, 이 파일에 적힌 값이 대화상자 설정보다 우선합니다 — 그래서 앱이 아예 켜지지 않을 때도 이 파일로 설정을 바꿀 수 있는 겁니다.

```toml
[rendering]
# 옵션: "auto", "vulkan" (Linux/Win), "dx12" (Win), "metal" (macOS), "cpu"
backend = "vulkan"

[display]
# Qt 씬 그래프 백엔드. 옵션: "auto", "vulkan", "d3d12", "metal", "opengl", "software"
qt_rhi_backend = "auto"

# 윈도우 시스템 플러그인(Linux 전용). 옵션: "auto", "xcb", "wayland"
qt_platform = "auto"

[performance]
# GPU 텍스처 크기 상한(픽셀) — VRAM이 적은 카드에서 유용. "auto" = 제한 없음.
max_texture_size = "auto"

# HQ 프리뷰 강제 on/off. 저장된 설정을 재정의하려면 주석 해제.
# force_hq_preview = false

# 인터랙티브 프리뷰의 긴 변 길이(픽셀, 512-8192). 클수록 캔버스가 선명해지지만
# 프레임당 VRAM 사용량이 늘어남.
# preview_render_size = 1600

# 프리뷰 캐시 크기 — 최근 본 사진을 메모리에 유지해 즉시 이동 가능하게 함.
# 저사양 RAM 머신에서는 낮추세요. 기본값(~1.2GB / 사진 8장)을 재정의하려면 주석 해제.
# preview_cache_max_bytes = 1200000000
# preview_cache_max_entries = 8

[logging]
# "debug", "info", "warning", "error"
level = "info"
```

`backend = "cpu"`로 설정하면 GPU 가속을 완전히 끕니다 — 사용 중인 하드웨어에서 GPU 백엔드가 죽는 경우 유용합니다.

---

## 로드맵
앞으로 추가하고 싶은 것들: [ROADMAP.md](docs/ROADMAP.md)

## 변경 이력:

[CHANGELOG.md](docs/CHANGELOG.md)

---

### 개발자를 위한 안내

자세한 내용은 [CONTRIBUTING.md](CONTRIBUTING.md) 참고.

## 라이선스
**[GPL-3](LICENSE)** 카피레프트 라이선스.

## 후원
이 도구가 마음에 드셨다면, 테스트용 필름 한 롤 값이라도 후원해주시면 좋겠습니다 :)

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/marcinzawalski)
