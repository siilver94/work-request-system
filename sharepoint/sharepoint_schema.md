## WorkRequests_DB (작업 의뢰)

| 컬럼명                           | 타입       | 설명                                                                                                     | 비고                               |
| ----------------------------- | -------- | ------------------------------------------------------------------------------------------------------ | -------------------------------- |
| **Title**                     | 텍스트      | 의뢰 제목                                                                                                  | 기존 필드                            |
| **RequestDetail**             | 여러 줄 텍스트 | 의뢰 상세 내용 (품번/내용)                                                                                       | `PartDescription`을 대체            |
| **RequestNo**                 | 텍스트      | 작업의뢰 번호 (예: `WR-YYYY-NNNN`)                                                                            | 요청 시 자동 생성                       |
| **RequesterName**             | 텍스트      | 요청자 이름                                                                                                 | UserInfo_DB에서 스냅샷                |
| **RequesterEmail**            | 텍스트      | 요청자 이메일                                                                                                | UserInfo_DB에서 스냅샷                |
| **RequesterDeptSnapshot**     | 텍스트      | 요청자 부문                                                                                                 | UserInfo_DB에서 스냅샷                |
| **RequesterTeamSnapshot**     | 텍스트      | 요청자 팀                                                                                                  | UserInfo_DB에서 스냅샷                |
| **RequesterJobGradeSnapshot** | 텍스트      | 요청자 직급                                                                                                 | UserInfo_DB에서 스냅샷                |
| **Stage**                     | 텍스트      | `Start`, `InProgress`, `Completed` 중 하나                                                                | 업무 진행 단계를 표시                     |
| **ApprovalState**             | 텍스트      | 요청 승인 상태: `PendingLeader`, `PendingWorker`, `AcceptedByWorker`, `RejectedByLeader`, `RejectedByWorker` | 기존 `Pending/Approved`에서 확장       |
| **CloseResult**               | 텍스트      | 종료 결과: `WorkDone` (완료), `Cancelled` (반려)                                                               |                                  |
| **ApproverEmail**             | 텍스트      | 팀장(승인자) 이메일                                                                                            | UserInfo_DB에서 조회                 |
| **ApprovedByName**            | 텍스트      | 실제 승인자 이름                                                                                              | 팀장 또는 요청자(대리 승인)                 |
| **ApprovedByEmail**           | 텍스트      | 실제 승인자 이메일                                                                                             |                                  |
| **IsMySelfApproved**          | 예/아니오    | 의뢰자가 직접(대리) 승인했는지 여부                                                                                   | 대리 승인 기능 구현에 사용                  |
| **WorkerName**                | 텍스트      | 작업자 이름                                                                                                 | 작업자 수락 시 저장                      |
| **WorkerEmail**               | 텍스트      | 작업자 이메일                                                                                                | 작업자 수락 시 저장                      |
| **SubmittedAt**               | 날짜시간     | 의뢰 등록 시간                                                                                               |                                  |
| **ApprovedAt**                | 날짜시간     | 승인 완료 시간                                                                                               | 팀장 승인 또는 대리 승인 시 저장              |
| **StartedAt**                 | 날짜시간     | 작업 시작 시간                                                                                               | 작업자가 수락하여 `InProgress`로 변경될 때 저장 |
| **CompletedAt**               | 날짜시간     | 작업 완료 시간                                                                                               | 작업 완료 시 저장                       |
| **RejectedAt**                | 날짜시간     | 반려 시각                                                                                                  |                                  |
| **RejectedByName**            | 텍스트      | 반려자 이름                                                                                                 | 팀장 또는 작업자                        |
| **RejectedByEmail**           | 텍스트      | 반려자 이메일                                                                                                |                                  |
| **RejectReason**              | 여러 줄 텍스트 | 반려 사유                                                                                                  | 승인/수락 단계에서 입력                    |
| **ProjectIDRef**              | 숫자       | 프로젝트 마스터의 ID 참조                                                                                        | `Project_DB` 연결                  |
| **ProjectName**               | 텍스트      | 프로젝트명                                                                                                  | `Project_DB`에서 선택                |
| **RelatedModel**              | 텍스트      | 관련기종(관리번호)                                                                                             | 사용자 입력                           |
| **RequestQty**                | 텍스트      | 수량 (숫자 또는 텍스트)                                                                                         | 사용자 입력                           |
| **DesiredDueDate**            | 날짜       | 희망완료일                                                                                                  | 사용자 입력                           |
| **HasAttachment**             | 예/아니오    | 첨부파일 여부                                                                                                | 첨부 컨트롤 갯수 기반                     |
| **LastActionByName**          | 텍스트      | 마지막 처리자 이름                                                                                             | 승인/반려/수락 등                       |
| **LastActionByEmail**         | 텍스트      | 마지막 처리자 이메일                                                                                            |                                  |
| **LastActionAt**              | 날짜시간     | 마지막 처리 시간                                                                                              |                                  |

## WorkRequestComments_DB (댓글/이력)

| 컬럼명                 | 타입       | 설명                                                                                                                        | 비고                  |
| ------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| **Title**           | 텍스트      | 기본값 `Comment`                                                                                                             |                     |
| **RequestID**       | 숫자       | 연관된 작업의뢰 ID                                                                                                               | WorkRequests_DB의 ID |
| **RequestTitle**    | 텍스트      | 작업의뢰 제목                                                                                                                   |                     |
| **RequestNo**       | 텍스트      | 작업의뢰 번호                                                                                                                   |                     |
| **CommentText**     | 여러 줄 텍스트 | 댓글 내용                                                                                                                     | 시스템 로그도 포함          |
| **CommentType**     | 텍스트      | `Submit`, `LeaderApprove`, `SelfApprove`, `LeaderReject`, `WorkerAccept`, `WorkerReject`, `WorkerStart`, `WorkComplete` 등 | 이력 유형 식별            |
| **CommentByName**   | 텍스트      | 작성자 이름                                                                                                                    |                     |
| **CommentByEmail**  | 텍스트      | 작성자 이메일                                                                                                                   |                     |
| **CommentRole**     | 텍스트      | `Requester`, `Leader`, `Worker`, `System` 등                                                                               | 역할 구분               |
| **CommentAt**       | 날짜시간     | 작성 시각                                                                                                                     |                     |
| **IsSystemComment** | 예/아니오    | 시스템 자동 생성 여부                                                                                                              |                     |
| **IsVisible**       | 예/아니오    | 표시 여부                                                                                                                     |                     |
