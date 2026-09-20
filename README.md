# Arca_Unity_Toolkit

> 최종 업데이트: 2026-09-20 (`unity-handoff` → `unity-editor-ops` 개편 · `github-issue-writer` 반영 · `unity-cli` 추적 제외)

Claude Code로 **유니티 프로젝트의 표준 환경을 세팅하고 키워 나가는 개인 툴킷**.

`~/.claude/skills/`에 놓여, 새 프로젝트에 나의 표준 `CLAUDE.md`·스킬·범용 C# 코드를 한 번에 심고,

이후 프로젝트에서 다듬은 규칙과 코드를 다시 이 마스터로 되돌려 **하나씩 축적**한다.

---

## 담고 있는 것

단순 스킬 모음이 아니라, 유니티 개발에 필요한 네 종류의 자산을 함께 보관한다.

| 경로 | 종류 | 내용 |
|------|------|------|
| `unity-project-setup/` | 슬래시 스킬 | `/unity-project-setup` — 새 프로젝트에 `CLAUDE.md` 생성 + 스킬·코드 자산 설치 |
| `unity-project-setup/templates/` | **마스터 원본** | 아래 세 자산의 단일 진실 (프로젝트로 복사해서 사용) |
| `unity-project-setup/templates/CLAUDE.md.template` | 문서 템플릿 | 프로젝트 유형(1인/협업·서버 유무)에 맞춰 채우는 `CLAUDE.md` 골격 |
| `unity-project-setup/templates/skills/` | 스킬 문서 | 재사용 작업 규칙 — `client/`·`common/`·`server/` 그룹 ([관리 규칙](unity-project-setup/templates/skills/README.md)) |
| `unity-project-setup/templates/code/` | 범용 C# 코드 | `Common/` — **자산 하나 = 폴더 하나** ([설치 규칙](unity-project-setup/templates/code/README.md)) |
| `unity-skill-sync/` | 슬래시 스킬 | `/unity-skill-sync` — 프로젝트 ↔ 마스터 diff 후 선택 반영 |

### 범용 코드 자산 7종

```
Common/
├── service-locator/    Services · MonoService — 하드 싱글톤 대체
├── mono-extensions/    RequireRef — 필수 인스펙터 참조 fail-fast 검증
├── center-header/      인스펙터 섹션 가운데 정렬 헤더
├── hierarchy-styler/   이름 접두 문자(!·@·#)로 하이어라키 줄 색칠
├── ugui-layout/        FlexibleGridLayoutGroup · SquareLayoutElement
├── editor-shared/      EditorGit · EditorIcons · ProjectPreferences
└── memory-meter/       상단 툴바 메모리 표시 + 정리
```

에디터 전용 파일은 각 자산 안 `Editor/`에 있다 — ⚠️ **펴면 빌드가 깨진다.**

코드 자산은 프로젝트 유형에 따라 **스크립트 루트**가 갈린다 — 1인 개발 `Assets/Scripts/`,

협업(클라이언트 담당) `Assets/Scripts_Client/`. 그 아래 `Common/` 구조는 두 경우가 동일하다.

### 스킬 12종

| 그룹 | 스킬 |
|------|------|
| `common/` | `commit-convention` · `agent-log-writer` · `agent-log-reader` · `task-writer` · `task-reader` · `github-issue-writer` |
| `client/` | `clean-code-style` · `feature-design` · `ugui-mvp` · `ugui-layout` · `optimization` · `unity-editor-ops` |
| `server/` | (없음) |

---

## 추적하지 않는 것 — Claude Code 자동 생성물

이 저장소는 `~/.claude/skills` **디렉터리 자체다.** 그래서 Claude Code가 자기 작업 폴더를
저장소 안에 만드는 일이 생긴다. 이런 것은 **툴킷의 자산이 아니다** — 내가 만들지도, 고치지도,
프로젝트에 심지도 않는다. 지워도 Claude Code가 다시 만든다.

| 경로 | 무엇 | 왜 제외 |
|------|------|---------|
| `synced/` | 계정에 등록된 스킬이 내려받아지는 착지점 (`docx`·`pptx`·`xlsx`·`pdf` 등 Anthropic 제공 스킬) | Anthropic 소유 · 경로에 계정 UUID · 자동 재생성 · 4MB 대부분이 OOXML 스키마 |
| `unity-cli/` | `unity skill install claude-code`가 심는 Unity 공식 CLI 스킬 | Unity 소유 · CLI 버전에 딸림 · 같은 명령으로 언제든 재생성 |

**올릴 때도 받을 때도 제외한다.**

- **git** — `.gitignore`가 막는다. 비슷한 폴더가 새로 생기면 거기에 한 줄 추가한다.
- **`/unity-project-setup`** — 심기 대상은 `unity-project-setup/templates/` 뿐이다.
  저장소 최상위의 다른 폴더는 프로젝트로 복사하지 않는다.
- **`/unity-skill-sync`** — 비교 대상은 `templates/` 아래로 한정된다.
  자동 생성물은 diff에도, push에도, pull에도 넣지 않는다.

> 판별 기준은 하나다 — **내가 고칠 수 있고, 고친 것이 남는가?**
> 다음 동기화에 덮이거나 저절로 다시 생기는 것이면 툴킷의 자산이 아니다.

---

## 워크플로우

1. **심기** — 새 프로젝트에서 `/unity-project-setup` → 마스터 최신본이 프로젝트에 복사됨
2. **키우기** — 작업 중 규칙·코드를 다듬음
3. **되돌리기** — `/unity-skill-sync` → 프로젝트에서 개선한 **범용 변경만** 마스터에 반영해 축적
4. 모든 md는 상단 `> 최종 업데이트: YYYY-MM-DD` 라인으로 수정 이력을 추적

---

## ⚠️ 공동 소유 — 양쪽이 따로 바뀔 수 있다

여기 있는 스킬·코드는 **여러 프로젝트에 심기고, 여러 사람이 각자의 프로젝트에서 고친다.**
그래서 마스터와 사본이 **각각 따로 바뀌어 있는 상태가 정상적으로 발생한다.**

- 각 `SKILL.md` 상단의 **소유 표식**(`🔗 공동 소유`)이 그 사실을 알린다.
  ⛔ 이 표식은 **마스터에만 둔다** — 프로젝트 저장소의 스킬은 협업자와 함께 쓰는 파일이라,
  내 개인 툴킷 사정을 적으면 그 사람에게는 뜻 모를 문구가 된다.
  `/unity-project-setup`이 복사할 때 떼고, `/unity-skill-sync`는 이 블록을 차이로 치지 않는다.
- `/unity-skill-sync`는 차이를 **범용 / 프로젝트 특화 / ⛔ 충돌** 셋으로 나누고,
  **충돌은 자동 병합하지 않는다** — 양쪽 diff를 나란히 보이고 줄 단위로 확인받는다.
- **프로젝트 사본을 직접 고치는 것은 허용한다.** 막는 것은 되돌림을 건너뛰는 것뿐이다.

---

## 규칙

- 마스터 스킬·코드 수정은 **이 저장소에서만** 한다 (프로젝트 사본에서 고쳤으면 sync로 되돌린다).
- md 수정 시 `> 최종 업데이트:` 날짜를 반드시 갱신한다.
- `templates/code/`의 `.cs`에는 `.meta`를 두지 않는다 — 프로젝트마다 GUID가 겹치므로, 복사 후 유니티에서 생성한다.
  같은 이유로 `.asset`(ScriptableObject 인스턴스)도 보관하지 않는다 — 스크립트 GUID를 참조해 **복사하면 깨진다.**
- **범용 코드에 특정 프로젝트의 클래스 이름을 남기지 않는다.** 주석 예시는 `XxxManager`처럼
  자리 표시자를 쓴다 — 다른 프로젝트에서 읽을 때 존재하지 않는 이름을 가리키게 된다.
  (`MonoService`의 예시가 실제로 `SessionManager`로 굳어 있다가 그 클래스가 사라진 적이 있다)
- **이 저장소의 커밋은 사용자가 직접 한다.** 스킬은 변경만 해 두고 커밋하지 않는다.
