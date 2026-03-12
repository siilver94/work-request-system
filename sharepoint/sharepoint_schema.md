# SharePoint Schema

## 1. WorkRequests_DB

| 컬럼명 | 타입 | 설명 |
|---|---|---|
| Title | 텍스트 | 의뢰 제목 |
| RequestDetail | 여러 줄 텍스트 | 의뢰 상세 내용 |
| RequestNo | 텍스트 | 작업의뢰 번호 |
| RequesterName | 텍스트 | 요청자 이름 |
| RequesterEmail | 텍스트 | 요청자 이메일 |
| RequesterDeptSnapshot | 텍스트 | 요청자 부문 |
| RequesterTeamSnapshot | 텍스트 | 요청자 팀 |
| RequesterJobGradeSnapshot | 텍스트 | 요청자 직급 |
| Stage | 텍스트 | Start / InProgress / Completed |
| ApprovalState | 텍스트 | Pending / Approved |
| CloseResult | 텍스트 | 완료 결과 |
| ApproverEmail | 텍스트 | 승인자 이메일 |
| ApprovedByName | 텍스트 | 실제 승인자 이름 |
| ApprovedByEmail | 텍스트 | 실제 승인자 이메일 |
| IsMySelfApproved | 예/아니오 | 본인 대리결재 여부 |
| WorkerName | 텍스트 | 작업자 이름 |
| WorkerEmail | 텍스트 | 작업자 이메일 |
| SubmittedAt | 날짜시간 | 제출 시간 |
| ApprovedAt | 날짜시간 | 승인 시간 |
| StartedAt | 날짜시간 | 시작 시간 |
| CompletedAt | 날짜시간 | 완료 시간 |
| RejectedAt | 날짜시간 | 반려 시간 |
| RejectedByName | 텍스트 | 반려자 이름 |
| RejectedByEmail | 텍스트 | 반려자 이메일 |
| RejectReason | 여러 줄 텍스트 | 반려 사유 |
| AttachmentLink | 하이퍼링크 | 첨부 링크 |
| HasAttachment | 예/아니오 | 첨부 여부 |
| LastActionByName | 텍스트 | 최종 처리자 이름 |
| LastActionByEmail | 텍스트 | 최종 처리자 이메일 |
| LastActionAt | 날짜시간 | 최종 처리 시간 |

## 2. WorkRequestComments_DB

| 컬럼명 | 타입 | 설명 |
|---|---|---|
| Title | 텍스트 | 기본값 Comment |
| RequestID | 숫자 | 요청 ID |
| RequestTitle | 텍스트 | 요청 제목 |
| RequestNo | 텍스트 | 작업의뢰 번호 |
| CommentText | 여러 줄 텍스트 | 댓글 내용 |
| CommentType | 텍스트 | General / Submit / Approve / Reject / Start / Complete |
| CommentByName | 텍스트 | 작성자 이름 |
| CommentByEmail | 텍스트 | 작성자 이메일 |
| CommentRole | 텍스트 | Requester / Approver / Worker / System |
| CommentAt | 날짜시간 | 작성 시간 |
| IsSystemComment | 예/아니오 | 시스템 댓글 여부 |
| IsVisible | 예/아니오 | 표시 여부 |

## 3. UserInfo_DB 참조 컬럼
- e-mail
- 이름
- 부문
- 팀
- 직급2
