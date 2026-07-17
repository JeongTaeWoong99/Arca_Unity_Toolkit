# Claude 전역 스킬 저장소

> 최종 업데이트: 2026-07-17

`~/.claude/skills/`에 위치하는 개인 전역 스킬 모음.

유니티 프로젝트의 Claude 환경(CLAUDE.md + 스킬)을 표준화·최신화하기 위한 저장소다.

## 구성

| 경로 | 내용 |
|------|------|
| `unity-project-setup/` | `/unity-project-setup` — 새 프로젝트에 CLAUDE.md 생성 + 스킬·코드 복사 |
| `unity-project-setup/templates/` | **마스터 원본** — CLAUDE.md 템플릿·스킬·코드 자산의 단일 진실 |
| `unity-project-setup/templates/skills/` | 스킬 문서 (client / common / server) |
| `unity-project-setup/templates/code/` | 범용 C# 코드 자산 — `Common/{Attribute,Editor,Utils}` ([설치 규칙](unity-project-setup/templates/code/README.md)) |
| `unity-skill-sync/` | `/unity-skill-sync` — 프로젝트 ↔ 마스터 diff 후 선택 반영 |

코드 자산은 프로젝트 유형에 따라 **스크립트 루트**가 갈린다 — 1인 개발 `Assets/Scripts/`,
협업(클라이언트 담당) `Assets/Scripts_Client/`. 그 아래 `Common/` 구조는 두 경우가 동일하다.

## 워크플로우

1. 새 프로젝트에서 `/unity-project-setup` → 마스터 최신본이 프로젝트에 복사됨
2. 작업 중 규칙 수정 → `/unity-skill-sync` → 범용 변경만 마스터에 반영
3. 모든 md는 상단 `> 최종 업데이트: YYYY-MM-DD` 라인으로 수정 이력 추적

## 규칙

- 마스터 스킬 수정은 이 저장소에서만 한다 (프로젝트 사본 수정 시 sync로 반영).
- md 수정 시 `> 최종 업데이트:` 날짜를 반드시 갱신한다.
