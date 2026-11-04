# Magical Imagination

**Magical Imagination**은 Unreal Engine 5로 제작한 **VR 프로젝트**입니다.

VR 컨트롤러를 이용한 이동과 오브젝트 상호작용을 기반으로 하며, 튜토리얼과 여러 스테이지·스토리 맵으로 구성되어 있습니다.

본 프로젝트는 **2명이 함께 진행한 팀 프로젝트**입니다.

---

## 프로젝트 소개

플레이어가 VR 환경에서 직접 움직이고 상호작용할 수 있도록 제작한 프로젝트입니다.

프로젝트에는 다음과 같은 콘텐츠가 포함되어 있습니다.

- VR 컨트롤러를 이용한 이동 및 상호작용
- 손으로 오브젝트를 잡는 Grab 기능
- VR 텔레포트 이동
- 스테이지 간 이동
- VR 메뉴
- 튜토리얼 및 여러 스테이지 / 스토리 맵
- VR 화면을 별도로 조작할 수 있는 Spectator 기능
- 마법 지팡이 및 저주·이펙트 관련 콘텐츠
- 게임 진행 정보를 저장하기 위한 SaveGame 데이터 구조

또한 MiVRy 플러그인과 Gesture Database가 프로젝트에 포함되어 있습니다.

다만 Gesture Database가 실제 게임에서 어떤 동작과 연결되는지는 저장소만으로 전체 동작을 확인하기 어려워, README에서는 구체적인 제스처 기능을 임의로 설명하지 않습니다.

---

## 주요 기능

### VR 이동

VR 컨트롤러를 이용한 이동 입력이 구성되어 있습니다.

Oculus Touch, Valve Index, Vive, Mixed Reality Controller 등의 입력 설정이 프로젝트에 포함되어 있습니다.

---

### Teleport

VR에서 원하는 위치를 지정하여 이동할 수 있는 텔레포트 기능이 구현되어 있습니다.

텔레포트 가능한 위치를 확인하고, 이동 위치를 표시한 뒤 플레이어를 해당 위치로 이동시키는 방식으로 구성되어 있습니다.

---

### Object Grab

VR 컨트롤러를 이용해 주변의 상호작용 가능한 오브젝트를 잡을 수 있습니다.

왼손과 오른손 각각에 대한 Grab 입력이 설정되어 있습니다.

---

### Level 이동

맵 내부에서 특정 위치로 이동하는 기능과 다른 레벨로 이동하는 기능이 구현되어 있습니다.

프로젝트에는 별도의 Teleport 관련 Blueprint가 구성되어 있습니다.

---

### VR Menu

VR 컨트롤러를 이용한 메뉴 입력이 설정되어 있습니다.

왼손 / 오른손 컨트롤러에 대한 메뉴 입력이 각각 구성되어 있습니다.

---

### Spectator

VR 플레이 화면을 모니터에서 별도로 확인하고 조작할 수 있는 Spectator 관련 기능이 포함되어 있습니다.

키보드와 마우스를 이용한 이동, 화면 회전, FOV 조절 등의 입력이 설정되어 있습니다.

---

## 맵 구성

프로젝트에는 튜토리얼부터 스테이지, 스토리, 엔딩까지 여러 맵이 포함되어 있습니다.

대표적인 맵 구성은 다음과 같습니다.

```text
00_GameStart
01_Tutorial
02_Stage1
03_Story2
04_Stage2
05_Story3
06_Stage3
07_Story4
08_Stage4
09_Ending
```

이외에도 테스트 및 개발 과정에서 사용된 추가 맵들이 포함되어 있습니다.

프로젝트 기본 실행 맵은 다음과 같이 설정되어 있습니다.

```text
/Game/Map/Tutorial
```

---

## 개발 환경

| 항목 | 버전 / 설정 |
| --- | --- |
| Engine | Unreal Engine 5.0 |
| Project Type | Blueprint 기반 Unreal Project |
| VR | OpenXR / OculusVR / SteamVR |
| Audio | Resonance Audio |
| Gesture Plugin | MiVRy |
| 기본 실행 맵 | Tutorial |
| 기본 브랜치 | master |

별도의 C++ Source 디렉터리는 없으며, 프로젝트의 주요 로직은 Unreal Engine Blueprint와 에셋을 중심으로 구성되어 있습니다.

---

## 사용된 플러그인

프로젝트 설정에서 다음 플러그인이 활성화되어 있습니다.

```text
OpenXR
OculusVR
SteamVR
ResonanceAudio
MiVRy
Text3D
```

MiVRy는 Unreal Marketplace 외부 플러그인이므로, 해당 플러그인이 설치되어 있지 않은 환경에서는 프로젝트를 열 때 플러그인 관련 오류가 발생할 수 있습니다.

---

## 실행 방법

### 1. 저장소 Clone

```bash
git clone https://github.com/nyang404/Magical-Imagination.git
cd Magical-Imagination
```

프로젝트에 Unreal Engine 에셋과 여러 리소스가 포함되어 있어 저장소 용량이 큰 편입니다.

---

### 2. Unreal Engine 설치

이 프로젝트는 다음 버전을 기준으로 제작되었습니다.

```text
Unreal Engine 5.0
```

가능하면 동일한 버전의 Unreal Engine을 사용하는 것을 권장합니다.

---

### 3. 프로젝트 열기

다음 파일을 Unreal Engine으로 실행합니다.

```text
Magical_Imagination/Magical_Imagination.uproject
```

---

### 4. 필요한 플러그인 확인

프로젝트를 실행하기 전에 다음 플러그인이 현재 Unreal Engine 환경에서 사용 가능한지 확인합니다.

```text
OpenXR
OculusVR
SteamVR
ResonanceAudio
MiVRy
Text3D
```

특히 `MiVRy`는 별도로 설치가 필요할 수 있습니다.

---

### 5. 게임 실행

프로젝트의 게임 기본 맵은 `Tutorial`로 설정되어 있습니다.

Unreal Editor에서 VR 장치를 연결한 뒤 VR Preview 또는 실행 환경을 통해 테스트할 수 있습니다.

프로젝트 설정에는 다음 값이 적용되어 있습니다.

```text
Start in VR = True
```

---

## VR 기기 관련 설정

프로젝트에는 다음 컨트롤러 입력 설정이 포함되어 있습니다.

- Oculus Touch
- Valve Index
- HTC Vive
- Windows Mixed Reality Controller

다만 각 장치에서 실제 플레이 테스트가 완료되었는지는 저장소만으로 확인할 수 없으므로, 위 항목은 **프로젝트에 입력 설정이 존재하는 장치**를 의미합니다.

---

## 프로젝트 구조

```text
Magical-Imagination/
├─ README.md
├─ VRPawn.uasset
└─ Magical_Imagination/
   ├─ Magical_Imagination.uproject
   ├─ Config/
   │  ├─ DefaultEngine.ini         # 엔진, 맵, VR/XR 관련 설정
   │  ├─ DefaultGame.ini           # 프로젝트 및 패키징 설정
   │  ├─ DefaultInput.ini          # VR Controller / Spectator 입력 설정
   │  └─ SteamVRBindings/          # SteamVR 장치별 입력 바인딩
   │
   └─ Content/
      ├─ Map/                      # 게임, 튜토리얼, 스테이지, 스토리, 엔딩 맵
      ├─ VRTemplate/               # VRPawn, VRGameMode, Grab, Teleport 관련 에셋
      ├─ Teleport/                 # 맵 내부 이동 및 레벨 전환 Blueprint
      ├─ UI/                       # 시작 메뉴 및 Text / Typewriter UI
      ├─ VRSpectator/              # VR Spectator 관련 에셋
      ├─ SaveData/                 # SaveGame 기반 데이터 에셋
      ├─ GestureDatabase/          # MiVRy Gesture Database
      ├─ Staff/                    # 마법 지팡이 모델 및 애니메이션
      ├─ Curses/                   # 저주 및 Smoke 관련 에셋
      ├─ CursedObjects/            # 스테이지 / 스토리용 오브젝트
      ├─ sy_aiTest/                # AI Controller 관련 테스트 에셋
      ├─ sy_monster_attack/        # 몬스터 / 동물 관련 Blueprint 및 에셋
      └─ ...                       # 환경, 사운드, VFX 및 외부 에셋
```

---

## Build 관련

프로젝트에는 Windows 및 Android 관련 설정이 포함되어 있습니다.

Android 설정에서는 Quest / Quest 2 관련 설정과 ARM64, Vulkan 설정을 확인할 수 있습니다.

다만 현재 저장소에는 아래와 같은 완성된 실행 파일은 포함되어 있지 않습니다.

```text
.exe
.apk
.aab
```

따라서 프로젝트를 실행하려면 Unreal Engine에서 직접 프로젝트를 열어 실행하거나 빌드해야 합니다.

---

> **README 작성 안내**  
> 본 프로젝트의 코드 및 에셋 개발에는 ChatGPT를 사용하지 않았으며, **이 `README.md` 문서의 작성 및 정리에만 ChatGPT를 사용했습니다.**
