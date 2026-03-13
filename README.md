# Work Request System

Power Apps 기반 연구소 작업의뢰 시스템

## System Overview

연구소 작업자가 제작 요청을 관리하기 위한 시스템

### 주요 기능

- 작업 의뢰 등록
- 팀장 승인 / 반려
- 작업 진행 관리
- 첨부파일 관리
- 작업 이력 추적
- KPI 분석

---

## Technology

- Power Apps (Canvas)
- SharePoint List
- Power Automate (알림 예정)

---

## Database

### WorkRequests_DB

주요 컬럼

- RequestNo
- Title
- RequestDetail
- RequesterName
- RequesterEmail
- RequesterTeamSnapshot
- Stage
- ApprovalState
- CloseResult
- SubmittedAt
- ApprovedAt
- StartedAt
- CompletedAt

---

### WorkRequestComments_DB

댓글 기록 저장

- RequestID
- Comment
- Author
- CreatedAt

---

## Workflow

1. 의뢰 등록
2. 팀장 승인
3. 작업 시작
4. 작업 완료
5. 기록 보관

- 전체 화면 코드: `powerapps/WorkRequest_Screen_fullcode.yaml`
