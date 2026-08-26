# 범용 코드 자산 (마스터 원본)

> 최종 업데이트: 2026-08-26 (자산별 폴더 구조로 개편 · 자산 3종 추가 · 자산 문서 동반 복사)

어떤 유니티 프로젝트에도 그대로 복사해 쓰는 범용 C# 코드의 단일 진실.
`/unity-project-setup` 이 프로젝트 유형에 맞는 위치로 복사한다.

---

## 설치 위치

이 폴더의 `Common/` **내부 구조를 그대로** 스크립트 루트 아래로 복사한다.
스크립트 루트는 개발 형태에 따라 앞부분만 갈린다.

| 개발 형태 | 스크립트 루트 | 설치 결과 |
|------|------|------|
| 1인 개발 | `Assets/Scripts` | `Assets/Scripts/Common/…` |
| 협업 (클라이언트 담당) | `Assets/Scripts_Client` | `Assets/Scripts_Client/Common/…` |

`Common/` 아래는 두 경우가 동일하므로, 마스터는 `Common/` 안쪽만 보관한다.

## 표준 구조 — 자산 하나 = 폴더 하나

> 2026-08-26 개편. 이전에는 `Attribute/`·`Editor/`·`Service/`·`Extensions/`라는
> **런타임 종류 축**으로 나눴는데, 그러면 자산 하나(CenterHeader)가 두 폴더로 갈라지고
> 자산이 늘수록 한 폴더 안에서 남남인 스크립트들이 섞였다.

```
<스크립트 루트>/Common/
├── service-locator/    Services.cs · MonoService.cs
├── mono-extensions/    MonoBehaviourExtensions.cs
├── center-header/      CenterHeaderAttribute.cs
│   └── Editor/         CenterHeaderDrawer.cs
├── hierarchy-styler/
│   └── Editor/         HierarchyPalette.cs · HierarchyStyler.cs
├── ugui-layout/        FlexibleGridLayoutGroup.cs · SquareLayoutElement.cs
│   └── Editor/         FlexibleGridLayoutGroupEditor.cs
├── editor-shared/
│   └── Editor/         EditorGit.cs · EditorIcons.cs · ProjectPreferences.cs
└── memory-meter/
    └── Editor/         EditorMemoryMeter.cs · EditorMemoryToolbarButton.cs
```

- 자산 폴더명은 **영문 소문자 kebab-case**. `Editor`만 Unity 예약어라 PascalCase 그대로다.
- ⚠️ **`Editor/` 중첩은 선택이 아니라 필수다.** Unity는 **정확히 `Editor`라는 이름의 폴더만**
  런타임 빌드에서 제외한다. `center-header/CenterHeaderDrawer.cs`처럼 평평하게 두면
  드로어가 빌드에 섞여 **컴파일이 깨진다.**
- **`ScriptableObject`라도 에디터 도구의 설정이면 `Editor/`에 둔다**(예: `HierarchyPalette`).
  런타임이 읽지 않는 데이터를 밖에 두면 빌드에 딸려 들어간다.
  다만 **그 에셋 파일(`.asset`) 자체는 마스터에 보관하지 않는다** — `.asset`은 스크립트의
  GUID를 참조하는데 마스터는 `.meta`를 두지 않으므로, 복사하면 새 프로젝트에서
  **Missing script로 깨진다.** 코드만 옮기고 설치 후 메뉴에서 새로 만든다.
- **빈 폴더를 미리 복사하지 않는다** — 자산이 생길 때 만든다.

## 자산 문서(`<자산> 규칙.md`)는 코드와 함께 복사된다

> 2026-08-26 변경. 이전에는 `Common/`에 md를 두지 않아서, 설명을 전부 코드 주석에
> 접어 넣어야 했다. 이제 자산 폴더의 `<폴더명> 규칙.md`도 함께 따라간다.

- 파일명은 **`<폴더명> 규칙.md`** 공식을 지킨다(`memory-meter 규칙.md`).
- **그래도 코드 주석은 줄이지 않는다.** 문서는 "이 자산 전체가 무엇이고 왜 이런가"를 담고,
  주석은 "이 줄이 왜 이런가"를 담는다 — 층이 다르다.
- 자산 문서에서 **프로젝트 문서를 상대 경로로 가리키지 않는다.** 프로젝트마다 깊이가 달라
  조용히 깨진다. 스킬을 가리킬 때는 `.claude/skills/…`를 저장소 루트 기준으로 적는다.

## 자산 목록

| 자산 | 폴더 | 내용 |
|------|------|------|
| **ServiceLocator** | `service-locator/` | 역할↔구현 등록·조회. 하드 싱글톤(`X.Inst`) 대체. ⚠️ **조회는 반드시 `Start`** — 사용 규칙은 `skills/client/feature-design` |
| **MonoBehaviourExtensions** | `mono-extensions/` | `RequireRef` — 필수 인스펙터 참조 검증(fail-fast). **사용 규칙은 `skills/client/clean-code-style` 9장** |
| **CenterHeader** | `center-header/` | 인스펙터 섹션을 가운데 정렬 헤더로 구분. 문구에 `< >`를 넣지 않는다 |
| **HierarchyStyler** | `hierarchy-styler/` | 하이어라키에서 이름 앞 접두 문자(`!`·`@`·`#`)로 줄을 색칠. 설치 후 `Create > Arca > Hierarchy Palette`로 팔레트를 하나 만든다 |
| **uGUI Layout** | `ugui-layout/` | `FlexibleGridLayoutGroup`(폭에 맞춰 셀 역산) · `SquareLayoutElement`("높이만큼 정사각형"). 함정 전반은 `skills/client/ugui-layout` |
| **EditorShared** | `editor-shared/` | 에디터 툴 공용 — git 실행(`EditorGit`) · 내장 아이콘 캐시(`EditorIcons`) · 환경 설정 뿌리(`ProjectPreferences`) |
| **MemoryMeter** | `memory-meter/` | 에디터 메모리 사용량을 상단 툴바에 1초마다 표시 + 클릭 시 정리. 용어·함정은 폴더의 규칙 문서에 |

**계약(인터페이스)은 마스터에 넣지 않는다.** `Services`는 *메커니즘*이고, `IOpponent` 같은 계약은
프로젝트마다 다른 *도메인*이다. 계약은 각 프로젝트에서 **그것을 정의한 기능 폴더 안 `Contracts/`** 에
두고 마스터로 올리지 않는다 (배치 규칙은 `skills/client/feature-design/SKILL.md` 3-1 절 참조).

## 규칙

- **이 코드는 nullable 참조 형식이 켜져 있다고 전제한다.** `MonoBehaviourExtensions`의
  `Object? reference`처럼 `?` 표기를 쓰므로, `Assets/csc.rsp`에 `-nullable:enable`이 없으면
  **CS8632 경고**가 뜬다. `/unity-project-setup` 3단계가 이 파일을 만든다.
- **`.meta`는 마스터에 보관하지 않는다.** 모든 프로젝트가 같은 GUID를 갖게 되어 프로젝트 간
  에셋 교환 시 충돌 원인이 된다. 복사 후 Unity 에디터를 켜서 생성시킨다.
- **네임스페이스를 두지 않는다.** 이 코드는 전부 글로벌 네임스페이스다 — 프로젝트마다 다른
  네임스페이스로 감싸면 복사할 때마다 손봐야 하고, 프로젝트 코드가 자기 네임스페이스 안에서
  글로벌 타입을 부르는 데는 아무 문제가 없다.
- **어트리뷰트에는 프로젝트 이름을 박을 수 없다는 점에 주의한다.** `[MainToolbarElement]` 같은
  어트리뷰트는 **컴파일 타임 상수만** 받으므로 `Application.productName`을 쓸 수 없다.
  그런 자리의 접두사는 툴킷 브랜드 **`Arca/`**로 통일한다(`Create > Arca > …`, 툴바 요소 경로).
  상수가 아니어도 되는 자리(설정 그룹 이름 등)는 `Application.productName`을 쓴다.
- **범용 코드에 특정 프로젝트의 클래스 이름을 남기지 않는다.** 주석 예시는 `XxxManager`처럼
  자리 표시자를 쓴다 — 다른 프로젝트에서 읽을 때 존재하지 않는 이름을 가리키게 된다.
- 프로젝트가 이 표준을 따르지 않는 경우(기존 폴더 구조가 이미 자리잡은 저장소 등)
  실제 설치 경로를 해당 프로젝트 `CLAUDE.md` 폴더 구조 표에 명시한다.
- 자산을 추가하면 위 **자산 목록** 표에 행을 더한다.
- 프로젝트에서 범용 코드를 개선했으면 `/unity-skill-sync`로 마스터에 반영한다.
