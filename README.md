<h1>📑 Requirement-to-TC AI Agent (for Dify)</h1>
요구사항 정의서를 기반으로 최적화된 테스트 케이스(TC)를 자동 생성하는 Dify 기반 AI 에이전트입니다.
<br><br>

<h2>📋 모델 구성 tool 및 api </h2>
- dify<br>
- cohere rerank<br>
- gemini-embbeding-001<br>
- gemini-2.5 flash ( claude sonnet 으로 변경 필요 ) 
<br><br>
<h2>📋 사용 방법 (Usage)</h2>
- '테스트케이스 생성 agent.yml' 파일을 다운받아 dify 스튜디오 메뉴에 드래그하여 워크플로우 생성 <br>   
(RAG 지식은 생성 따로 진행해야함 하단 기재)<br><br><br>


<h3> 테스트 진행 방법 </h3>
<h4>엑셀 파일로 생성할 때 (addFileYn = 'Y')</h4>
- addFileYn : 변수를 Y로 설정<br>
- attachedDoc : 요구사항 정의서.xlsx 파일을 업로드<br>
- AI가 파일 내용을 파싱하여 테스트 케이스 리스트 출력<br><br>

<h4>JSON 데이터로 생성할 때 (addFileYn = 'N')</h4>
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
<br>

<h3>📋 API 연동 방법 </h3>
<h4>엑셀 첨부파일 post</h4>
<pre>
  //파일업로드 api 
url : http://publicip:8080/v1/files/upload
header 'Authorization: Bearer {api_key}'
form 'file=@localfile;type=image/[png|jpeg|jpg|webp|gif]' 
form 'user=test_01'
</pre><br>
<pre>
  //파일업로드 수신 JSON
  //id를 복사해서 workflow api 호출필요
{
    "id": "7990da17-aa3f-42b1-adc3-5c8cf239e210",
    "name": "테스트케이스_test.csv",
    "size": 2340,
    "extension": "csv",
    "mime_type": "text/csv",
    "created_by": "78ebb34f-8b8c-48b8-ad3f-bddc19ff9bd9",
    "created_at": 1778825899,
    "preview_url": null,
    "source_url": "/files/7990da17-aa3f-42b1-adc3-5c8cf239e210/file-preview?timestamp=1778825899&nonce=f0e00db5c56d1dcf93bc64b3abe8d44a&sign=5MOHMJC6SdwzVIem9wtXvKXdQKIHAtsrggOXGg01rq4%3D",
    "original_url": null,
    "user_id": null,
    "tenant_id": "b28ed76e-1e7e-4012-adf4-d794c191814f",
    "conversation_id": null,
    "file_key": null
}
  
</pre>
<pre>
  //첨부파일 workflow api 호출
url : http://publicip:8080/v1/workflows/run
header 'Authorization: Bearer {api_key}' 
header 'Content-Type: application/json'
request body : '{
    "inputs": {
        "attachedDoc": {
            "type": "document",
            "transfer_method": "local_file",
            "upload_file_id": "7990da17-aa3f-42b1-adc3-5c8cf239e210"
        },
        "addFileYn": "Y"
    },
    "response_mode": "blocking",
    "user": "test_01"
}'
</pre>
<br><br>
<h4>JSON post</h4>
<pre>
url : http://publicip:8080/v1/workflows/run
header 'Authorization: Bearer {api_key}' 
header 'Content-Type: application/json'
request body '{
    "inputs": {
        "addFileYn": "N",
        "jsonData": "[{\"projectId\": \"PRJ-INS-001\", \"title\": \"사고접수\", \"description\": \"인보험 사고접수 페이지에서 추가 청구 기능 개발필요\", \"devType\": \"화면\", \"isNew\": \"신규\", \"isDbTask\": \"비대상\", \"isFinancial\": \"대상\"}]"
    },
    "response_mode": "blocking",
    "user": "developer_01"
}'
</pre><br><br>
<h4>공통 workflow 수신 JSON</h4>
<pre>
{
    "task_id": "83fadec3-2630-4e6e-a1a7-2b7c8c116602",
    "workflow_run_id": "a99d0989-97d1-4905-a893-5e31f7b6a21a",
    "data": {
        "id": "a99d0989-97d1-4905-a893-5e31f7b6a21a",
        "workflow_id": "28e02c6a-77df-4ff0-8210-95c71d2a1cf5",
        "status": "succeeded",
        "outputs": {
            "text": [
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-001",
                    "testCaseName": "인보험 사고접수 페이지 진입 시 '추가 청구' 버튼/링크 노출 확인",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "유효한 사고접수 건 (추가 청구 가능한 상태)",
                    "type": "Positive",
                    "priority": "High"
                },
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-002",
                    "testCaseName": "'추가 청구' 버튼 클릭 시 추가 청구 정보 입력 화면 로드 확인",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "유효한 사고접수 건, 추가 청구 버튼 클릭",
                    "type": "Positive",
                    "priority": "High"
                },
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-003",
                    "testCaseName": "추가 청구 필수 항목 모두 입력 후 정상 접수 확인",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "사고번호: 'S001-2023-001', 추가청구항목: '통원치료비', 청구금액: '150000', 청구사유: '추가 진료 발생'",
                    "type": "Positive",
                    "priority": "High"
                },
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-004",
                    "testCaseName": "추가 청구 완료 후 성공 메시지 표시 및 사고접수 내역에 반영 확인",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "정상적으로 추가 청구 완료된 건",
                    "type": "Positive",
                    "priority": "High"
                },
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-005",
                    "testCaseName": "필수 입력 항목 누락 시 유효성 검증 오류 메시지 표시 확인",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "사고번호: 'S001-2023-001', 추가청구항목: '', 청구금액: '100000', 청구사유: '누락 테스트'",
                    "type": "Negative",
                    "priority": "High"
                },
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-006",
                    "testCaseName": "청구금액 필드에 숫자 외 문자 입력 시 유효성 검증 오류 메시지 표시 확인",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "사고번호: 'S001-2023-001', 추가청구항목: '약제비', 청구금액: 'abc', 청구사유: '잘못된 형식'",
                    "type": "Negative",
                    "priority": "Medium"
                },
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-007",
                    "testCaseName": "이미 최종 처리된 사고에 대해 추가 청구 시도 시 불가 메시지 확인",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "사고번호: 'S001-2023-002' (처리상태: '지급완료'), 추가청구항목: '재활치료', 청구금액: '200000'",
                    "type": "Negative",
                    "priority": "High"
                },
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-008",
                    "testCaseName": "추가 청구로 인한 보험금 지급액 변경 시, 정확한 금액 반영 여부 확인",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "기존 지급액: 500000, 추가 청구액: 150000, 예상 총 지급액: 650000",
                    "type": "Positive",
                    "priority": "High"
                },
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-009",
                    "testCaseName": "추가 청구 시, 백오피스 시스템으로 추가 청구 정보가 정확히 전달되는지 확인",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "사고번호: 'S001-2023-003', 추가청구항목: '물리치료', 청구금액: '80000'",
                    "type": "Positive",
                    "priority": "High"
                },
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-010",
                    "testCaseName": "동일 사고 건에 대한 중복 추가 청구 시도 시 방지 로직 동작 확인",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "사고번호: 'S001-2023-004' (이미 추가 청구 접수됨), 동일 청구 항목으로 재시도",
                    "type": "Negative",
                    "priority": "High"
                },
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-011",
                    "testCaseName": "추가 청구 금액이 특정 한도(예: 100만원) 초과 시 승인 절차 트리거 확인",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "사고번호: 'S001-2023-005', 추가청구항목: '수술비', 청구금액: '1200000'",
                    "type": "Edge",
                    "priority": "High"
                },
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-012",
                    "testCaseName": "추가 청구 화면의 UI/UX가 사용자 친화적이고 일관된 디자인을 따르는지 확인",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "N/A",
                    "type": "UI/UX",
                    "priority": "Medium"
                },
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-013",
                    "testCaseName": "모바일/태블릿 환경에서 추가 청구 화면의 반응형 디자인 및 기능 동작 확인",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "접속 환경: 모바일 (Chrome, Safari), 태블릿 (Chrome, Safari)",
                    "type": "UI/UX",
                    "priority": "Medium"
                },
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-014",
                    "testCaseName": "추가 청구 기능 사용 시 개인 및 금융 정보의 안전한 전송/처리 여부 확인 (보안)",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "N/A",
                    "type": "Security",
                    "priority": "High"
                },
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-015",
                    "testCaseName": "비로그인 또는 권한이 없는 사용자가 추가 청구 기능에 접근 시 접근 제어 확인",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "비로그인 상태 또는 일반 사용자 계정 (추가 청구 권한 없음)",
                    "type": "Security",
                    "priority": "High"
                },
                {
                    "projectId": "PRJ-INS-001",
                    "testCaseId": "TC-INS-ADDCLAIM-016",
                    "testCaseName": "추가 청구 화면 로딩 및 데이터 전송 처리 속도 확인 (성능)",
                    "programName": "인보험 사고접수 페이지 - 추가 청구 기능",
                    "testData": "N/A",
                    "type": "Performance",
                    "priority": "Low"
                }
            ]
        },
        "error": null,
        "elapsed_time": 21.774564,
        "total_tokens": 4686,
        "total_steps": 7,
        "created_at": 1778824363,
        "finished_at": 1778824385
    }
} 
</pre>
<br><br>

<h2>📋 지식 설정 방법 (RAG_테스트자료 첨부파일 활용)</h2>
<img width="1358" height="1247" alt="image" src="https://github.com/user-attachments/assets/ed44ca2f-63d1-433a-ba29-ace14f512024" />
<img width="1358" height="781" alt="image" src="https://github.com/user-attachments/assets/e36e1593-0fd1-43ac-87f7-3674ec6795c3" />
<br><br>
<h3>파일업로드 청크설정</h3>
<img width="1358" height="588" alt="image" src="https://github.com/user-attachments/assets/81060047-43ff-4d0e-bf08-8c5b56278d72" />
