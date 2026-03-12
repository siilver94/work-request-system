# WorkRequests_DB Schema

| Column | Type | Description |
|------|------|-------------|
RequestNo | Text | 작업의뢰 번호 |
Title | Text | 제목 |
RequestDetail | Text | 의뢰내용 |
RequesterName | Text | 요청자 이름 |
RequesterEmail | Text | 요청자 이메일 |
RequesterTeamSnapshot | Text | 요청 팀 |
Stage | Text | 진행 단계 |
ApprovalState | Text | 결재 상태 |
CloseResult | Text | 완료 결과 |
SubmittedAt | DateTime | 제출 시간 |
ApprovedAt | DateTime | 승인 시간 |
StartedAt | DateTime | 작업 시작 |
CompletedAt | DateTime | 작업 완료 |
