---
name: unity-project-setup
description: 새 유니티 프로젝트의 Claude 초기 세팅. CLAUDE.md 생성과 스킬·범용 코드 자산 복사를 수행한다. 프로젝트 초기 세팅, 클로드 세팅, 스킬 세팅 요청 시 사용.
disable-model-invocation: true
---

> 최종 업데이트: 2026-07-18

# 유니티 프로젝트 초기 세팅

새 유니티 프로젝트에 나의 표준 Claude 환경(CLAUDE.md + 스킬 + 범용 코드 자산)을 구축한다.
마스터 원본은 이 스킬 폴더의 `templates/`에 있으며, **프로젝트로 복사해서** 사용한다
(협업자도 git으로 같은 규칙을 받도록 하기 위함).

---

## 절차

### 1. 프로젝트 정보 수집

- `ProjectSettings/ProjectVersion.txt`에서 Unity 버전을 읽는다.
- 렌더 파이프라인을 확인한다: `Packages/manifest.json`에 `com.unity.render-pipelines.universal`(URP) /
  `high-definition`(HDRP)이 있으면 해당 파이프라인, 없으면 Built-in.
- 사용자에게 질문한다 (한 번에 묶어서):
  1. 프로젝트 한 줄 개요 (어떤 게임/앱인지)
  2. 1인 개발인지 협업인지
  3. 서버(네트워크) 파트가 있는지

### 2. 스킬 복사

- `templates/skills/` 전체를 프로젝트의 `.claude/skills/`로 복사한다.
  (구조: `client/`, `common/`, `server/` 그룹 유지)
- 서버 파트가 없으면 `server/` 폴더는 제외한다.
- 이미 `.claude/skills/`가 존재하면 덮어쓰기 전에 사용자에게 확인한다.

### 3. 코드 자산 설치

- **스크립트 루트**를 1단계 답변으로 결정한다 : 1인 개발 `Assets/Scripts`,
  협업(클라이언트 담당) `Assets/Scripts_Client`.
- `templates/code/Common/` 내부 구조를 그대로 `<스크립트 루트>/Common/` 아래로 복사한다.
  (표준 구조 : `Common/Attribute/` 런타임 어트리뷰트 · `Common/Editor/` 에디터 전용 ·
  `Common/Service/` 서비스 로케이터 · `Common/Extensions/` 확장 메서드. 자세한 규칙은 `templates/code/README.md` 참조)
- 프로젝트에 이미 다른 공통 폴더 구조가 자리잡았으면 **표준을 강요하지 않는다.**
  기존 위치를 쓸지 표준으로 옮길지 사용자에게 확인하고, 실제 경로를 CLAUDE.md 폴더 구조 표에 명시한다.
  (이미 `.meta`가 있는 파일을 옮기면 GUID가 바뀌어 인스펙터 연결이 끊길 수 있다)
- 복사한 `.cs`에는 `.meta`가 없다. Unity 에디터를 켜서 생성시킨 뒤 원본과 함께 커밋하라고 안내한다.

### 4. CLAUDE.md 생성

- `templates/CLAUDE.md.template`을 기반으로 프로젝트 루트에 `CLAUDE.md`를 생성한다.
- `{{DATE}}` = 오늘 날짜(YYYY-MM-DD), `{{UNITY_VERSION}}`, `{{RENDER_PIPELINE}}`,
  `{{PROJECT_OVERVIEW}}`를 채운다.
- 템플릿 내 `<!-- IF ... -->` 주석의 지시에 따라 1인/협업, 서버 유무에 맞게
  섹션을 조정하고, 처리 후 지시 주석은 제거한다.
- 폴더 구조 표는 실제 프로젝트의 `Assets/` 구성을 확인해 맞게 작성한다.
  코드 자산을 설치한 경로(스크립트 루트)도 행으로 넣는다.

### 5. 마무리

- `.gitignore`에 `.claude/settings.local.json` 항목이 없으면 추가한다.
  추가만으로 끝내지 말고 `git check-ignore` 로 실제 적용을 확인한다 — **이미 추적 중인 파일에는
  `.gitignore`가 적용되지 않으므로**, 추적 중이면 `git rm --cached` 로 인덱스에서 뺀다(파일은 유지됨).
- 생성/복사된 파일 목록과 CLAUDE.md 요약을 보고한다.
- 세팅 중 마스터 템플릿을 개선할 점을 발견했으면 `/unity-skill-sync`를 제안한다.

---

## 규칙

- 복사된 스킬·코드 내용은 임의로 수정하지 않는다 (마스터가 단일 진실).
  프로젝트에서 개선했으면 `/unity-skill-sync`로 마스터에 되돌린다.
- 모든 md 문서 상단의 `> 최종 업데이트:` 라인은 세팅 날짜로 갱신한다.
- `.claude/` 폴더는 `Assets/` 밖이므로 `.meta` 파일이 생기지 않는다 — 그대로 커밋하면 된다.
  반대로 `templates/code/`에서 복사한 `.cs`는 `Assets/` 안이므로 `.meta`가 필요하다.
