# 마법공방의 대장장이 문서 Agent 규칙

이 저장소는 **마법공방의 대장장이(Project FORGE)**의 기획 문서를 Mintlify/MDX로 관리한다. Agent는 문서를 생성·수정할 때 아래 규칙을 반드시 따른다.

## 1. 문서 타입

가장 구체적인 `doc_type`을 선택한다.

- `prd`: 제품 목표, 사용자 가치, 타깃, 검증 목표, MVP 범위
- `gdd`: 플레이어 판타지, 핵심 재미, 게임 구조, 시스템 지도
- `system`: 특정 시스템의 규칙, 상태, 상호작용
- `content`: 반복 제작되는 게임 콘텐츠의 분류와 제작 규격
- `feature`: 구현 단위의 기능, 입출력, 상태, 예외, 구현 상태
- `wbs`: 일정, 작업, 담당, 의존성, 완료 기준
- `decision`: 충돌/선택의 배경, 선택지, 결정, 영향
- `data`: 대량 데이터나 스키마 중심 문서

## 2. Front Matter 강제

일반 기획 문서는 최소 다음 필드를 가진다.

```yaml
---
title: 문서 제목
doc_type: system
status: review
version: 0.1.0
owner: TBD
created: YYYY-MM-DD
updated: YYYY-MM-DD
scope: mvp
deprecated: false
---
```

필요 시 `tags`, `related`, `source`, `reviewers`, `supersedes`, `superseded_by`를 추가한다.

- `status`: `draft | review | approved | deprecated`
- `scope`: `poc | prototype | mvp | post-mvp | live`
- `deprecated: true`이면 `status: deprecated`여야 한다.
- 확인할 수 없는 값은 추측하지 않고 `TBD`로 둔다.
- 의미가 바뀌면 `updated`를 갱신하고 필요 시 `version`을 올린다.

## 3. 문서 수명주기

`draft → review → approved → deprecated`

- 삭제보다 이력 보존을 우선한다.
- 오래되었거나 대체된 문서는 삭제하지 말고 `deprecated` 처리한다.
- 대체 관계가 있으면 `supersedes` / `superseded_by`를 양방향으로 연결한다.
- `approved` 문서의 핵심 정책은 사용자 지시나 새 Decision 없이는 변경하지 않는다.

## 4. 충돌 처리

- 여러 원본이 충돌하면 수정일이나 구현 상태만으로 자동 승자를 정하지 않는다.
- 기획 정책과 구현 상태는 별개의 사실로 기록한다.
- 실제 코드가 다르다는 이유만으로 기획 정책을 자동 변경하지 않는다.
- 미해결 충돌은 `decision` 문서에 기록하고 사용자 결정을 받는다.
- 해결된 충돌은 시스템 문서에 최종 정책을 반영하고 Decision 문서에는 이력으로 남긴다.

## 5. 현재 확정된 프로젝트 기준

다음 항목은 2026-08-24 기준 사용자 결정으로 확정되었다.

1. 무기 제작 시 **사전 원소 보관 시스템을 사용하지 않는다**. 보유 원소 전체를 팔레트에 표시하고 실제 사용 원소 종류 수 N으로 제한한다.
2. 플레이어는 **무기 대종류만 선택**하며 세부 무기 종류는 시스템/AI가 자동 판정한다.
3. 스케치 해석에 **VLM을 사용하지 않는다**. 벡터 스트로크 → 래스터/OpenCV·수학 분석 → 구조화 JSON → 로컬 텍스트 LLM 흐름을 사용한다.
4. **태그 시스템은 MVP 핵심 시스템**이다. NPC 의뢰 태그와 완성 무기 태그를 비교하여 평가 점수에 사용한다.
5. 기존 마이그레이션 문서는 검토가 끝날 때까지 `status: review`, `owner: TBD`를 기본값으로 사용한다.

## 6. 작성 규칙

- 한 문서는 하나의 책임을 가진다.
- PRD에 기능별 입출력/클래스 구조를 넣지 않는다.
- GDD는 시스템 세부 규칙을 반복하지 않고 시스템 문서로 연결한다.
- 시스템 기획서는 게임 규칙과 정책을 우선하고 구현 상세는 기능정의서로 분리한다.
- 기능정의서는 `기획 상태`와 `구현 상태`를 반드시 분리한다.
- 대량 콘텐츠 목록은 `content` 또는 `data` 문서로 분리한다.
- 일정과 담당은 WBS에서 관리하고 시스템 문서에 중복하지 않는다.
- 수치가 미정이면 임의 값을 만들지 않고 `TBD` 또는 `미정`으로 표기한다.

## 7. 원본 출처

Google Docs/Sheets에서 이관한 문서는 가능한 경우 `source`에 원본 URL을 남긴다. Mintlify 문서는 현재 기준으로 재구성된 문서이며, 원본은 변경 이력 및 근거 확인에 사용한다.
