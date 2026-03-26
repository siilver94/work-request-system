시작실 작업 현황판 (Work Request System)

이 저장소는 연구소 내 작업 의뢰를 등록·처리·관리하기 위해 개발된 시작실 작업 현황판 Power Apps 솔루션의 소스 코드 및 문서를 포함합니다. 주요 데이터 소스로 SharePoint 리스트를 사용하며, Power Automate/Dataverse 등 프리미엄 커넥터 없이 동작하도록 설계되었습니다.

주요 기능
작업 의뢰 등록: 사용자는 제목·프로젝트·관련 기종·수량·희망 완료일·세부 내용을 입력하여 새로운 작업 의뢰를 제출할 수 있습니다. 필수 항목이 입력되지 않으면 저장되지 않으며, 등록 시 자동으로 RequestNo(WR-연도-4자리)가 생성됩니다.
승인 및 반려 흐름: 의뢰 등록 후 Stage="Start" 와 ApprovalState="PendingLeader" 로 설정되어 팀장 승인을 기다립니다. 팀장은 승인 또는 반려를 선택할 수 있으며, 승인 시 PendingWorker 상태로 넘어가 작업자에게 알림이 전송됩니다. 반려 시 CloseResult="Cancelled" 로 기록되고 의뢰자에게 알림됩니다. 팀장 부재 시 의뢰자가 대리 승인(팝업 확인) 할 수 있습니다.
작업자 처리: 작업자는 PendingWorker 상태에서 작업 수락 또는 반려를 선택합니다. 수락 시 Stage="InProgress", ApprovalState="AcceptedByWorker" 로 변경되어 작업을 시작할 수 있으며, 반려 시 CloseResult="Cancelled" 로 저장됩니다. 작업 진행 후 btnStartWork/btnCompleteWork 버튼을 통해 시작 시각(StartedAt), 완료 시각(CompletedAt) 및 결과(CloseResult="WorkDone")를 기록합니다.
메인 현황판: 시작전/진행중/완료 세 컬럼으로 구성된 카드형 대시보드를 제공하며, 기간·검색·토글(완료/반려 포함) 필터를 통해 원하는 조건의 작업을 조회할 수 있습니다. 카드에는 상태 배지, 제목, 담당자, 요청일/마감일이 표시되고, 기한 임박 시 색상으로 강조됩니다.
역할별 화면: 의뢰자·팀장·작업자 각 역할에 따라 상세화면 하단의 처리 버튼이 달라집니다. 의뢰자는 삭제/대리승인, 팀장은 승인/반려, 작업자는 수락/반려/완료 버튼만 볼 수 있습니다.
알림 시스템: Office 365 Outlook 커넥터를 이용해 각 단계 전환 시 관련자에게 메일 알림을 전송합니다. 작업 진행 중에는 작업자, 승인/반려 시에는 팀장·의뢰자 등 역할별로 적절한 안내 메일이 발송됩니다.
관리자 전용 조회: 연구관리팀 팀장(seung.kim@tym.world)은 진행중·완료 컬럼 전체를 조회할 수 있도록 특별 권한(varIsViewer)을 부여했습니다. 시작전 컬럼은 본인 요청만 볼 수 있으며 결재 권한은 기존과 동일하게 유지됩니다.
설치 및 배포
SharePoint 리스트 준비: WorkRequests_DB와 WorkRequestComments_DB 리스트를 생성하고 sharepoint_schema.md에 정의된 컬럼을 동일하게 구성합니다. 앱에서 데이터 소스 이름은 각각 das와 WorkRequestComments_DB로 지정되어 있습니다.
Power Apps 임포트: 이 저장소의 powerapps/WorkRequest_Screen_fullcode.yaml 파일을 Power Apps 스튜디오에서 가져와 새 캔버스 앱을 생성합니다. OnStart 이벤트 실행 후 데이터 연결을 설정하면 앱이 초기화됩니다.
환경 변수 설정: 앱 실행 시 varAppUrl(앱 링크), varWorkerEmail(작업자 이메일), varMyEmail/varMyName 등의 사용자 정보를 초기화합니다. App.OnStart에 맞게 환경에 따라 값을 수정하세요.
Teams 탭 추가(선택): 앱을 Microsoft Teams의 탭으로 추가하여 조직원들이 쉽게 접근하도록 할 수 있습니다.
파일 구조
├─ powerapps
│   └─ WorkRequest_Screen_fullcode.yaml    # 전체 Power Apps 화면 정의
├─ sharepoint
│   └─ sharepoint_schema.md                # SharePoint 리스트 컬럼 설명
├─ docs
│   ├─ Manuals
│   │   └─ WorkRequest_Manual_v1.pptx      # 사용자 매뉴얼(PPT)
│   └─ images
│       └─ workflow.png                    # 작업 흐름도 등 매뉴얼 이미지
├─ README.md                               # 프로젝트 개요 및 사용법
├─ development_log.md                      # 버전별 변경 내역
└─ ... 기타 소스 및 자산
개발 노트

자세한 변경 사항과 향후 개선 계획은 development_log.md에 기록되어 있습니다. 버그 수정이나 기능 추가 시 해당 문서를 업데이트하여 이력 관리를 해주세요.
