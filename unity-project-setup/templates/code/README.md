# 범용 코드 자산 (마스터 원본)

> 최종 업데이트: 2026-07-18

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

## 표준 구조

```
<스크립트 루트>/Common/
├── Attribute/   런타임 어트리뷰트 (인스펙터 표식 등)
├── Editor/      에디터 전용 코드 (드로어·툴) — 빌드에서 제외됨
├── Service/     서비스 로케이터 — 전역 접근의 공통 창구
└── Extensions/  확장 메서드 — 기존 타입에 얹는 공통 헬퍼
```

- **에디터 전용 코드는 전부 `Common/Editor/` 한 곳에 모은다.** Unity는 `Editor` 라는 이름의 폴더를
  에디터 전용으로 처리해 빌드에서 제외한다. 런타임 코드가 이 안에 들어가면 빌드가 깨지므로,
  런타임 어트리뷰트는 반드시 `Attribute/` 에 둔다.
- 어트리뷰트와 짝꿍 드로어는 폴더가 갈린다 (예 : `Attribute/CenterHeaderAttribute.cs` ↔
  `Editor/CenterHeaderDrawer.cs`). 짝을 찾기 쉽도록 **파일명을 `~Attribute` / `~Drawer` 로 맞춘다.**
- 위 폴더들은 표준 분류이며, **자산이 생길 때 만든다** — 빈 폴더를 미리 복사하지 않는다.
- 세 분류에 안 맞는 범용 코드가 생기면 `Common/<범주>/` 로 폴더를 추가하고 이 문서에 반영한다.

## 자산 목록

| 자산 | 런타임 | 에디터 | 내용 |
|------|------|------|------|
| CenterHeader | `Common/Attribute/CenterHeaderAttribute.cs` | `Common/Editor/CenterHeaderDrawer.cs` | 인스펙터 섹션을 가운데 정렬 헤더로 구분 |
| ServiceLocator | `Common/Service/Services.cs` + `Common/Service/MonoService.cs` | — | 역할↔구현 등록/조회. 하드 싱글톤(`X.Inst`) 대체 — 사용 규칙은 `feature-design` 참조 |
| MonoBehaviourExtensions | `Common/Extensions/MonoBehaviourExtensions.cs` | — | MonoBehaviour 확장 메서드. 필수 인스펙터 참조 검증(`RequireRef`, fail-fast) 등 |

**계약(인터페이스)은 마스터에 넣지 않는다.** `Services` 는 *메커니즘*이고, `IOpponent` 같은 계약은
프로젝트마다 다른 *도메인*이다. 계약은 각 프로젝트에서 **그것을 정의한 기능 폴더 안 `Contracts/`** 에
두고 마스터로 올리지 않는다 (배치 규칙은 `skills/client/feature-design/SKILL.md` 3-1 절 참조).

## 규칙

- **`.meta` 는 마스터에 보관하지 않는다.** 모든 프로젝트가 같은 GUID를 갖게 되어 프로젝트 간
  에셋 교환 시 충돌 원인이 된다. 복사 후 Unity 에디터를 켜서 생성시킨다.
- 프로젝트가 이 표준을 따르지 않는 경우(기존 폴더 구조가 이미 자리잡은 저장소 등)
  실제 설치 경로를 해당 프로젝트 `CLAUDE.md` 폴더 구조 표에 명시한다.
- 자산을 추가하면 위 **자산 목록** 표에 행을 더하고, 사용 규칙이 필요하면
  `skills/client/clean-code-style/SKILL.md` 에 절을 추가한다.
- 프로젝트에서 범용 코드를 개선했으면 `/unity-skill-sync` 로 마스터에 반영한다.
