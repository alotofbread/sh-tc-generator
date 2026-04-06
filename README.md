<h1>📑 Requirement-to-TC AI Agent (for Dify)</h1>
요구사항 정의서를 기반으로 최적화된 테스트 케이스(TC)를 자동 생성하는 Dify 기반 AI 에이전트입니다.
<br><br>

<h2>📋 사용 방법 (Usage)</h2>
<h3>엑셀 파일로 생성할 때 (addFileYn = 'Y')</h3>
- addFileYn : 변수를 Y로 설정<br>
- requirements_doc : 요구사항 정의서.xlsx 파일을 업로드<br>
- AI가 파일 내용을 파싱하여 테스트 케이스 리스트 출력<br><br>

<h3>JSON 데이터로 생성할 때 (addFileYn = 'N')</h3>
- addFileYn : 변수를 N로 설정<br>
- jsonData : 아래와 같은 형식의 요구사항 JSON 데이터를 붙여넣기
<br><br>
<pre>
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
</pre>
<br><br><br><br>

<h2>📋 지식 설정 방법 (RAG_테스트자료 첨부파일 활용)</h2>
<img width="1358" height="1247" alt="image" src="https://github.com/user-attachments/assets/ed44ca2f-63d1-433a-ba29-ace14f512024" />
<img width="1358" height="781" alt="image" src="https://github.com/user-attachments/assets/e36e1593-0fd1-43ac-87f7-3674ec6795c3" />
<br><br>
<h3>파일업로드 청크설정</h3>
<img width="1358" height="588" alt="image" src="https://github.com/user-attachments/assets/81060047-43ff-4d0e-bf08-8c5b56278d72" />
