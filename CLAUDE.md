# CLAUDE.md — 시작실 작업 현황판 세션 컨텍스트

> 이 파일은 Claude와의 개발 세션을 재개할 때 가장 먼저 첨부해야 하는 컨텍스트 문서입니다.
> README.md(일반 사용자용), sharepoint_schema.md(DB 구조)와 역할을 분리하여 관리합니다.

---

## 프로젝트 개요

- **앱 이름**: 연구소 시작실 작업의뢰 현황판
- **플랫폼**: Microsoft Power Apps Canvas App
- **배포**: Microsoft Teams 탭 앱
- **데이터 소스**: SharePoint List (무료 플랜, 프리미엄 커넥터 없음)
- **이메일 알림**: `Office365Outlook.SendEmailV2` (Power Automate 미사용)
- **GitHub**: https://github.com/siilver94/analysis-request-system

---

## 데이터 소스 구조

| 변수명 | SharePoint List명 | 역할 |
|--------|------------------|------|
| `das` | WorkRequests_DB | 작업 의뢰 메인 데이터 |
| `WorkRequestComments_DB` | WorkRequestComments_DB | 단계별 이력 로그 |
| `UserInfo_DB` | UserInfo_DB | 사용자 정보 (팀, 직급, 이메일) |
| `Project_DB` | Project_DB | 프로젝트 마스터 |

---

## 워크플로우

```
의뢰 등록
  └─ Stage="Start", ApprovalState="PendingLeader"
       ├─ 팀장 승인 → ApprovalState="PendingWorker"
       │    └─ 작업자 수락 → Stage="InProgress", ApprovalState="AcceptedByWorker"
       │         ├─ 작업 완료 → Stage="Completed", CloseResult="WorkDone"
       │         └─ 작업자 반려 → Stage="Completed", CloseResult="Cancelled"
       ├─ 팀장 반려 → Stage="Completed", CloseResult="Cancelled"
       └─ 대리 승인(의뢰자 본인) → ApprovalState="PendingWorker", IsMySelfApproved=true
```

---

## 주요 변수 목록

| 변수명 | 타입 | 설명 |
|--------|------|------|
| `varMyEmail` | Text | 현재 로그인 사용자 이메일 (Lower 처리) |
| `varMyName` | Text | 현재 사용자 이름 (UserInfo_DB 우선, FullName 폴백) |
| `varMyTeam` | Text | 현재 사용자 팀명 |
| `varMyDept` | Text | 현재 사용자 부문 |
| `varMyJobGrade` | Text | 현재 사용자 직급 |
| `varWorkerEmail` | Text | 작업자 이메일 (하드코딩 1인 고정: `jung.kim@tym.world`) |
| `varIsWorker` | Boolean | 현재 사용자 = 작업자 여부 |
| `varIsTeamLeader` | Boolean | 직급2 = "팀장" 여부 |
| `varIsViewer` | Boolean | 전체 데이터 조회 권한자 여부 (이메일 하드코딩) |
| `varSelectedRequest` | Record | 현재 선택된 작업 의뢰 레코드 |
| `varRequestMode` | Text | 팝업 모드: "New" / "View" / "Edit" |
| `varShowRequestPopup` | Boolean | 상세/등록 팝업 표시 여부 |
| `varShowSelfApproveConfirmPopup` | Boolean | 대리승인 확인 팝업 |
| `varShowDeleteConfirmPopup` | Boolean | 삭제 확인 팝업 |
| `varAppUrl` | Text | 앱 바로가기 URL (이메일 알림에 사용) |
| `varDateFrom` | DateTime | 기간 필터 시작일 |
| `varIncludeCompleted` | Boolean | 완료 건 포함 토글 |
| `varIncludeRejected` | Boolean | 반려 건 포함 토글 |
| `varShowToast` | Boolean | 토스트 알림 표시 여부 |
| `varToastMessage` | Text | 토스트 알림 메시지 |

---

## 역할별 갤러리 조회 범위

### galStart (시작전)

| 역할 | 조회 범위 |
|------|----------|
| varIsViewer | 전체 |
| varIsWorker | ApprovalState = "PendingWorker" 전체 |
| varIsTeamLeader | 본인 건 + 같은 팀 PendingLeader 건 |
| 일반 사용자 | 본인 건만 |

### galInProgress (진행중)

모든 사용자에게 **전체 공개** (시작실 업무 과부하 파악 목적)

### galCompleted (완료)

| 역할 | 조회 범위 |
|------|----------|
| varIsViewer | 전체 |
| varIsWorker | 전체 |
| varIsTeamLeader | RequesterTeamSnapshot = varMyTeam |
| 일반 사용자 | RequesterEmail = varMyEmail |

---

## 화면 구성

단일 스크린(`WorkRequest_Screen`) 안에 모든 UI가 팝업 레이어 방식으로 구현되어 있습니다.

```
WorkRequest_Screen
├─ Header (앱 타이틀, 작업의뢰 버튼)
├─ con_searchFilter (좌측 필터 패널: 기간, 검색, 토글)
├─ Container7 — galStart (시작전 컬럼)
├─ Container8 — galInProgress (진행중 컬럼)
├─ Container9 — galCompleted (완료 컬럼)
├─ conRequestPopup (상세보기 / 등록 팝업)
│   ├─ Container2 (View 헤더: 제목, 상태 배지, 닫기)
│   ├─ conRequestView (View 모드: 요약정보, 상세정보, 첨부파일, 역할별 액션)
│   │   ├─ conRequesterAction (의뢰자: 대리승인, 삭제)
│   │   ├─ conLeaderAction (팀장: 승인, 반려)
│   │   └─ conWorkerAction (작업자: 수락, 반려, 완료)
│   └─ confrmRequest (New/Edit 모드: 등록 폼)
├─ conSelfApproveConfirmPopup (대리승인 확인)
├─ conDeleteConfirmPopup (삭제 확인)
└─ conNotice (토스트 알림)
```

---

## 하드코딩 항목 (환경 변경 시 수정 필요)

| 항목 | 위치 | 현재값 |
|------|------|--------|
| 작업자 이메일 | `WorkRequest_Screen.OnVisible` > `varWorkerEmail` | `jung.kim@tym.world` |
| 전체조회 권한자 | `WorkRequest_Screen.OnVisible` > `varIsViewer` | `seung.kim@tym.world`, `eunseong.kim@tym.world` |
| 앱 바로가기 URL | `WorkRequest_Screen.OnVisible` > `varAppUrl` | PowerApps 배포 URL |

---

## 개발 원칙 및 주의사항

- **SharePoint 무료 플랜**: Dataverse, 프리미엄 커넥터 사용 불가. 데이터 소스는 SharePoint List만 사용.
- **Delegation 주의**: `Find()` 함수는 delegation 불가 → 500건 초과 시 검색 누락 가능. 현재 데이터 규모에서는 문제없으나 대용량 전환 시 재설계 필요.
- **이름 파싱**: `varMyName`은 반드시 `UserInfo_DB.이름_한글` 우선 사용. `User().FullName` 파싱은 슬래시 포함 이름에서 깨질 수 있어 폴백으로만 사용.
- **이메일 비교**: 항상 `Lower()` 처리 후 비교.
- **Power Fx 수식**: 컬럼명을 따옴표로 감싸지 말 것 (예: `Lower(제목)` ✅, `Lower("제목")` ❌).
- **SharePoint 첨부파일**: 이미 저장된 항목 간 복사 불가 (URL 참조값) → 신규 등록 시 직접 업로드만 가능.
- **이메일 발송**: Power Automate 없이 `Office365Outlook.SendEmailV2` 직접 호출.

---

## 세션 재개 방법

1. 이 파일(`CLAUDE.md`)을 Claude 채팅에 첨부
2. `powerapps/WorkRequest_Screen_fullcode.yaml` 첨부 (전체 코드 확인이 필요한 경우)
3. `sharepoint/sharepoint_schema.md` 첨부 (DB 구조 변경이 필요한 경우)
4. 작업할 내용을 설명하면 바로 이어서 개발 가능
