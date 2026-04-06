📑 Requirement-to-TC AI Agent (for Dify)
요구사항 정의서를 기반으로 최적화된 테스트 케이스(TC)를 자동 생성하는 Dify 기반 AI 에이전트입니다.


📋 사용 방법 (Usage)
1. 엑셀 파일로 생성할 때 (addFileYn = 'Y')
addFileYn 변수를 Y로 설정
요구사항 정의서.xlsx 파일을 업로드
AI가 파일 내용을 파싱하여 테스트 케이스 리스트 출력

2. JSON 데이터로 생성할 때 (addFileYn = 'N')
addFileYn 변수를 N로 설정

아래와 같은 형식의 요구사항 JSON 데이터를 입력창에 붙여넣기

[
  {
    "projectId": "PRJ-INS-001",
    "title": "사고접수",
    "description": "인보험 사고접수 페이지에서 추가 청구 기능 개발필요\n추가 청구 체크박스 체크 후 사고접수 저장을 하게되면 추가 청구 되도록 구현",
    "devType": "화면",
    "isNew": "신규",
    "isDbTask": "비대상",
    "isFinancial": "대상"
  }
]








📋 지식 설정 방법 (RAG 구성) RAG_테스트자료 첨부파일 활용
<img width="1358" height="1247" alt="image" src="https://github.com/user-attachments/assets/ed44ca2f-63d1-433a-ba29-ace14f512024" />
<img width="1358" height="781" alt="image" src="https://github.com/user-attachments/assets/e36e1593-0fd1-43ac-87f7-3674ec6795c3" />
