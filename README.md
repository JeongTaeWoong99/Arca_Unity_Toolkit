# Arca_Unity_Toolkit

> 최종 업데이트: 2026-07-18

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
| `unity-project-setup/templates/skills/` | 스킬 문서 | 재사용 작업 규칙 — `client/`·`common/`·`server/` 그룹 |
| `unity-project-setup/templates/code/` | 범용 C# 코드 | `Common/{Attribute, Editor, Service, Extensions}` ([설치 규칙](unity-project-setup/templates/code/README.md)) |
| `unity-skill-sync/` | 슬래시 스킬 | `/unity-skill-sync` — 프로젝트 ↔ 마스터 diff 후 선택 반영 |

코드 자산은 프로젝트 유형에 따라 **스크립트 루트**가 갈린다 — 1인 개발 `Assets/Scripts/`,

협업(클라이언트 담당) `Assets/Scripts_Client/`. 그 아래 `Common/` 구조는 두 경우가 동일하다.

---

## 워크플로우

1. **심기** — 새 프로젝트에서 `/unity-project-setup` → 마스터 최신본이 프로젝트에 복사됨
2. **키우기** — 작업 중 규칙·코드를 다듬음
3. **되돌리기** — `/unity-skill-sync` → 프로젝트에서 개선한 **범용 변경만** 마스터에 반영해 축적
4. 모든 md는 상단 `> 최종 업데이트: YYYY-MM-DD` 라인으로 수정 이력을 추적

---

## 규칙

- 마스터 스킬·코드 수정은 **이 저장소에서만** 한다 (프로젝트 사본에서 고쳤으면 sync로 되돌린다).
- md 수정 시 `> 최종 업데이트:` 날짜를 반드시 갱신한다.
- `templates/code/`의 `.cs`에는 `.meta`를 두지 않는다 — 프로젝트마다 GUID가 겹치므로, 복사 후 유니티에서 생성한다.
