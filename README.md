# Magical Imagination

Unreal Engine 5 기반의 VR 프로젝트입니다.  
이 저장소는 **2명이 함께 진행한 팀 프로젝트**이며, VR 입력/상호작용, 텔레포트, 레벨 전환, VR 관전자 입력 설정과 여러 스테이지/스토리 맵 및 콘텐츠 에셋으로 구성되어 있습니다.

> 이 README는 저장소의 `.uproject`, `Config` 파일과 Blueprint `.uasset`에서 확인 가능한 심볼/참조를 기준으로 작성했습니다.  
> 파일명만 보고 기능을 추정하지 않았으며, 실제 실행 흐름까지 확인할 수 없는 항목은 별도로 표시합니다.

## 개발 환경 / 버전

| 항목 | 저장소에서 확인된 값 |
| --- | --- |
| Unreal Engine | **5.0** |
| 프로젝트 형식 | Blueprint 중심 프로젝트 |
| C++ Source | 별도 `Source/`, `.cpp`, `.h`, `.Build.cs`, `.Target.cs` 없음 |
| 기본 브랜치 | `master` |
| 게임 기본 맵 | `/Game/Map/Tutorial` |
| 기본 GameMode | `/Game/VRTemplate/Blueprints/VRGameMode` |
| Start in VR | `True` |
| ProjectVersion | `GeneralProjectSettings`에 별도 값 없음 |
| Android VersionDisplayName | `1.0` |

프로젝트의 `Magical_Imagination.uproject`에는 다음 플러그인이 활성화되어 있습니다.

- ResonanceAudio
- OpenXR
- OculusVR
- MiVRy
- Text3D
- SteamVR

## 확인된 기능

### VR Grab

`VRPawn` Blueprint에서 다음 로직과 입력이 확인됩니다.

- `GrabLeft`, `GrabRight`
- `TryGrab`
- `GetGrabComponentNearMotionController`
- Motion Controller 주변의 Grab Component 검색
- 좌/우 컨트롤러 Grab 입력 처리

입력 설정은 Oculus Touch, Valve Index, Mixed Reality, Vive 계열에 대해 `GrabLeft` / `GrabRight` 매핑을 포함합니다.

### VR Teleport

`VRPawn` Blueprint 내부에서 다음 텔레포트 관련 로직을 확인할 수 있습니다.

- `StartTeleportTrace`
- `EndTeleportTrace`
- `IsValidTeleportLocation`
- `TryTeleport`
- `K2_TeleportTo`
- `VRTeleportVisualizer`
- Niagara 기반 `NS_TeleportTrace` 참조

즉, VR Pawn 자체에 텔레포트 위치 탐색, 유효 위치 확인, 텔레포트 시각화, 실제 이동 처리에 필요한 Blueprint 로직이 포함되어 있습니다.

### 레벨 내부 이동 / 레벨 전환

`Content/Teleport`에는 다음 Blueprint가 있습니다.

- `TeleportInLevel.uasset`
- `TeleportPoint.uasset`
- `ChangeLevel.uasset`

`TeleportInLevel`에서는 Overlap 이벤트와 `K2_TeleportTo` 호출이 확인되며, 별도의 Teleport Point 위치/회전을 사용합니다.

`ChangeLevel`에서는 Overlap 이벤트와 `OpenLevelBySoftObjectPtr` 호출이 확인됩니다. 따라서 충돌/Overlap을 기준으로 다른 레벨을 여는 Blueprint 로직이 실제로 존재합니다.

### VR Menu

`VRPawn`에 다음 메뉴 관련 심볼이 확인됩니다.

- `ToggleMenu`
- `CloseMenu`
- `MenuToggleLeft`
- `MenuToggleRight`
- `MenuReference`

Oculus Touch, Valve Index, Mixed Reality, Vive에 대한 좌/우 메뉴 입력 매핑도 Config에 등록되어 있습니다.

### VR Controller 입력

`DefaultInput.ini`에 다음 계열의 입력이 설정되어 있습니다.

- Oculus Touch
- Valve Index
- Mixed Reality Controller
- Vive Controller

확인되는 주요 Action / Axis는 다음과 같습니다.

- Grab Left / Right
- Trigger Left / Right
- Movement Axis Left / Right
- Menu Toggle Left / Right
- Trigger Axis Left / Right

이는 **입력 매핑이 설정되어 있다는 의미이며, 각 장치에서 실제 플레이 테스트가 완료되었다는 의미는 아닙니다.**

### VR Spectator 입력

`VRSpectator` 관련 에셋이 존재하며, `DefaultInput.ini`에는 관전자용 입력이 등록되어 있습니다.

- WASD 이동
- Q / E / Space 수직 이동
- Mouse X / Y 회전
- Mouse Wheel FOV
- `F`: Fade Toggle
- `Tab`: Spectator Active Toggle
- `R`: Rotation Reset
- Middle Mouse: FOV Reset
- Gamepad 대응 입력

### VR / XR 설정

프로젝트 설정에서 다음 항목을 확인할 수 있습니다.

- `bStartInVR=True`
- OpenXR 활성화
- OculusVR 활성화
- SteamVR 활성화
- Oculus HMD `HandTrackingSupport=ControllersAndHands`
- Oculus XR API: `NativeOpenXR`
- Resonance Audio Spatialization / Reverb 설정

### 저장 데이터 구조

`Content/SaveData/SaveData.uasset`은 Unreal의 `SaveGame` 클래스를 기반으로 하며 다음 데이터가 확인됩니다.

- `GameStage`
- `SaveSlotName`
- `DataToSave`
- `SaveGameIdVars`

다만 저장소 검사만으로는 이 SaveGame 객체가 어느 시점에 저장/로드되는지 전체 호출 흐름까지 확인하지 못했습니다. 따라서 README에서는 **저장 데이터 구조가 구현되어 있음**까지만 명시합니다.

## Gesture / MiVRy 관련

`Magical_Imagination.uproject`에서 **MiVRy 플러그인이 활성화**되어 있고, `Content/GestureDatabase` 디렉터리와 여러 `.dat` 파일이 존재합니다.

또한 패키징 설정에는 다음 항목이 있습니다.

```ini
+DirectoriesToAlwaysStageAsNonUFS=(Path="GestureDatabase")
```

따라서 Gesture Database를 패키징 결과에 포함하도록 구성되어 있는 것은 확인할 수 있습니다.

다만 주요 Blueprint 로직이 바이너리 `.uasset` 형식이기 때문에, 현재 저장소 검사만으로는 **특정 제스처가 어떤 마법이나 게임 동작으로 연결되는지까지 확정할 수 없습니다.**  
따라서 제스처 인식 결과나 마법 종류를 임의로 기능 목록에 추가하지 않았습니다.

## 맵 구성

`Content/Map`에는 다음과 같은 프로젝트 맵 에셋이 포함되어 있습니다.

```text
00_GameStart.umap
01_Tutorial.umap
02_Stage1.umap
03_Story2.umap
04_Stage2.umap
05_Story3.umap
06_Stage3.umap
07_Story4.umap
08_Stage4.umap
09_Ending.umap

Tutorial.umap
Stage1.umap
Stage2.umap
Stage3.umap
StartScene.umap
VRTemplateMap2_2.umap
VRTemplateMap2_3.umap
```

맵 파일명은 GameStart → Tutorial → Stage / Story → Ending 형태로 정리되어 있습니다.  
다만 파일명의 순서만으로 실제 자동 진행 순서를 단정하지 않습니다.

## 프로젝트 구조

주요 디렉터리는 다음과 같습니다.

```text
Magical-Imagination/
├─ README.md
├─ VRPawn.uasset
└─ Magical_Imagination/
   ├─ Magical_Imagination.uproject
   ├─ Config/
   │  ├─ DefaultEngine.ini
   │  ├─ DefaultGame.ini
   │  ├─ DefaultInput.ini
   │  └─ SteamVRBindings/
   └─ Content/
      ├─ Map/                 # 게임/스테이지/스토리 맵
      ├─ VRTemplate/          # VRPawn, VRGameMode, GrabComponent 등
      ├─ Teleport/            # 레벨 내부 이동 및 레벨 전환 Blueprint
      ├─ SaveData/            # SaveGame 기반 데이터 에셋
      ├─ UI/                  # StartMenu, Text/Typewriter 관련 에셋
      ├─ VRSpectator/         # VR 관전자 관련 에셋
      ├─ GestureDatabase/     # MiVRy 관련 데이터 파일
      ├─ Staff/               # Magic staff 모델/애니메이션 에셋
      ├─ Curses/              # Smoke / Curse 관련 에셋
      ├─ CursedObjects/       # 스테이지/스토리용 오브젝트 에셋
      ├─ sy_aiTest/           # AI Controller 관련 테스트/콘텐츠 에셋
      ├─ sy_monster_attack/   # 몬스터/동물 관련 Blueprint 및 에셋
      └─ ...
```

`Content`에는 프로젝트 자체 에셋 외에도 Animal Variety Pack, Megascans, Medieval Environment, Paragon Rampage, Infinity Blade Effects 등 다수의 외부/마켓플레이스 계열 에셋 디렉터리가 포함되어 있습니다.

## 실행 방법

### 1. 저장소 Clone

```bash
git clone https://github.com/nyang404/Magical-Imagination.git
cd Magical-Imagination
```

이 저장소는 `.uasset`, `.umap`, 오디오/텍스처 등 대용량 바이너리 에셋을 많이 포함하고 있으므로 Clone 및 디스크 사용량이 클 수 있습니다.

### 2. Unreal Engine 준비

프로젝트 파일에 저장된 Engine Association은 **Unreal Engine 5.0**입니다.

가능하면 UE 5.0 환경에서 여는 것을 권장합니다.

### 3. 필요한 플러그인 확인

프로젝트에서 아래 플러그인이 활성화되어 있으므로, 프로젝트를 열기 전에 현재 Unreal Engine 설치 환경에서 사용 가능한지 확인해야 합니다.

- ResonanceAudio
- OpenXR
- OculusVR
- **MiVRy**
- Text3D
- SteamVR

특히 MiVRy는 `.uproject`에 Marketplace URL과 함께 등록된 외부 플러그인이므로, 로컬 엔진에 플러그인이 없는 경우 프로젝트 로드 시 관련 오류가 발생할 수 있습니다.

### 4. 프로젝트 열기

다음 파일을 Unreal Editor로 엽니다.

```text
Magical_Imagination/Magical_Imagination.uproject
```

### 5. 실행 맵

Config에 설정된 **게임 기본 맵**은 다음과 같습니다.

```text
/Game/Map/Tutorial
```

반면 **Editor Startup Map**은 다음 외부 에셋 데모 맵으로 설정되어 있습니다.

```text
/Game/Medieval_Environment/Medieval_Houses_Vol2/Maps/MH_02_Demo_Scene_Day
```

따라서 Unreal Editor를 열었을 때 Tutorial 맵이 바로 표시되지 않을 수 있습니다. 프로젝트 게임 흐름을 확인하려면 Content Browser에서 `Content/Map/Tutorial` 또는 필요한 프로젝트 맵을 직접 열어 테스트하는 것이 안전합니다.

### 6. VR 실행

VR HMD 및 컨트롤러를 연결하고 Unreal Editor의 VR 실행 환경에서 테스트합니다.

프로젝트 설정 자체는 `bStartInVR=True`이며 OpenXR / OculusVR / SteamVR 플러그인이 활성화되어 있습니다.

## 패키징 설정

저장소 Config에서 다음 패키징 설정을 확인할 수 있습니다.

- Shipping 기본 Build Configuration
- `ForDistribution=True`
- Pak 사용
- IoStore 사용
- 패키지 압축 사용
- 한국어(`ko`) / 영어(`en`) Culture Stage
- `GestureDatabase`를 Non-UFS 디렉터리로 Stage
- Android Target Flavor: `Android_ASTC`

Android / Oculus 관련 설정에는 다음 값이 존재합니다.

- Quest
- Quest 2
- Vulkan 활성화
- ARM64 빌드
- Min SDK 23
- Target SDK 25

이는 **저장소에 저장된 당시의 프로젝트 설정값**이며, 최신 Android / Quest 배포 요구사항과의 호환 여부를 의미하지는 않습니다.

저장소에는 `.exe`, `.apk`, `.aab` 등 완성된 패키지 실행 파일이 포함되어 있지 않습니다.

## 확인 범위

이 프로젝트는 대부분 Unreal Blueprint `.uasset`으로 구성되어 있습니다.

README 작성 시 다음을 직접 확인했습니다.

- `.uproject`의 Unreal Engine 버전 및 플러그인 설정
- `DefaultEngine.ini`의 게임 기본 맵, GameMode, XR / Oculus / Android 설정
- `DefaultGame.ini`의 VR 시작 및 패키징 설정
- `DefaultInput.ini`의 VR Controller / Spectator 입력 매핑
- `VRPawn.uasset`의 Grab / Teleport / Menu 관련 Blueprint 심볼
- `TeleportInLevel.uasset`의 `K2_TeleportTo` 호출
- `ChangeLevel.uasset`의 `OpenLevelBySoftObjectPtr` 호출
- `SaveData.uasset`의 SaveGame 기반 데이터 구조
- 저장소 전체 파일 트리의 프로젝트 구조 및 맵/에셋 존재 여부

반대로 실제 Blueprint 그래프 전체 실행 순서를 확인할 수 없는 부분은 기능으로 단정하지 않았습니다.

## 팀

**2인 팀 프로젝트**

두 명이 함께 개발 및 콘텐츠 구성을 진행한 프로젝트입니다.

## License

저장소 루트에서 별도의 `LICENSE` 파일은 확인되지 않았습니다.

또한 프로젝트에는 여러 외부/마켓플레이스 에셋 디렉터리가 포함되어 있으므로, 재배포 또는 상업적 사용 시 각 에셋의 개별 라이선스를 별도로 확인해야 합니다.
