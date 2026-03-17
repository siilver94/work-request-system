프로젝트 개요

이 앱은 Microsoft Power Apps를 사용하여 사내 작업 의뢰 프로세스를 자동화합니다. SharePoint 리스트를 백엔드 데이터베이스로 활용하며, 대규모 비용이 발생하는 Power Automate나 Dataverse 없이 구현되었습니다.

환경

데이터 소스: SharePoint 리스트 (WorkRequests_DB, WorkRequestComments_DB, UserInfo_DB, Project_DB).

사용자 규모: 약 100명. 성능과 위임(Delegation)을 고려한 필터 사용.

라이선스 제한: 유료 커넥터(Power Automate, Dataverse 등) 사용 불가. Email 알림은 Office 365 Outlook 커넥터를 활용.

주요 기능

Kanban 방식의 작업 목록

Start, InProgress, Completed 3단 칼럼으로 구성.

필터: 기간(30일/90일/직접 설정), 검색어(제목/요청자/요청번호), 완료/반려 포함 여부.

작업 의뢰 등록 팝업

프로젝트명은 Project_DB에서 검색 및 선택(중복 제거 및 ‘기타’ 옵션 맨 아래 정렬).

관련기종, 품번(요청상세), 수량, 희망완료일 입력 가능.

첨부파일 업로드 지원 (HasAttachment 여부 저장).

새 요청 시 RequestNo = "WR-연도-4자리 일련번호" 자동 생성.

승인/반려 흐름

팀장 승인/반려: PendingLeader 상태에서 팀장이 승인하면 PendingWorker로, 반려하면 RejectedByLeader 및 Stage = Completed로 변경.

의뢰자 대리 승인: 팀장이 부재하거나 긴급 시 의뢰자가 본인 승인 가능(IsMySelfApproved = true). 팝업을 통해 경고 후 진행.

작업자 수락/반려: PendingWorker 상태에서 작업자가 수락하면 InProgress/AcceptedByWorker로, 반려하면 RejectedByWorker/Completed로 변경.

작업 진행/완료: 작업자가 진행 상태에서 완료하면 Stage = Completed, CloseResult = WorkDone 저장.

메일 알림

Office 365 Outlook 커넥터로 승인/반려/수락/완료 시 팀장, 작업자, 의뢰자에게 메일 발송.

메일 제목/본문에 요청번호, 제목, 작성자, 반려 사유 등을 포함하고 앱 링크(varAppUrl) 제공.

댓글 및 이력 관리

모든 액션(제출, 승인, 반려, 수락, 시작, 완료)을 WorkRequestComments_DB에 기록.

UI에서 타임라인 형태로 표시할 수 있도록 설계.

권한 기반 UI 표시

varIsTeamLeader, varIsWorker 변수를 기준으로 버튼의 Visible 제어.

승인/반려/수락/완료 버튼은 해당 사용자에게만 노출.

실행 방법

해당 Power Apps 앱을 열고, App.OnStart 실행(또는 앱을 새로고침)하여 사용자 정보와 데이터 소스를 초기화합니다.

작업 등록 – “신규 등록” 버튼 클릭 후 폼에 입력 및 파일 업로드 후 저장.

팀장 승인/반려 – Start 컬럼에서 팀장 전용 버튼을 통해 처리. 의뢰자는 긴급 시 대리 승인 버튼을 사용.

작업자 수락/반려 – PendingWorker 상태에서 작업자 화면에 나타나는 버튼으로 진행.

작업 진행/완료 – InProgress 컬럼에서 작업자가 시작/완료 버튼을 클릭.
