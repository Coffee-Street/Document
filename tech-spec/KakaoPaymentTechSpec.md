## STRADA 결제 API 설계

### 1. Summary

* Strada 어플리케이션의 결제 기능을 제공하기 위해 Kakao pay의 단건 결제에 대해 study하고 간단한 웹 어플리케이션을 통해 데모 환경 구축

### 2. Background

* Strada 어플리케이션의 결제 기능을 제공하기 위해



### 3. Goals

* 결제 플랫폼(모듈)을 연동
* Kakao 단건 결제를 첫번째 target으로 결제 흐름을 파악하고 내용 정리
* 결제 준비, 결제 승인, 결제 완료, 결제 내역을 관리할 수 있도록 api 개발
* 다른 결제 플랫폼(모듈)을 추가할 수 있도록 portable한 설계

### 4. Non-goals

* CORS 문제
  1. 테스트 할때만 발생하는지, 실제로도 발생할 수 있는지?
* 실제 결제까지 진행하지는 않음

### 5. Plan

* 간단한 결제 테스트 webserver 를 react로 개발

* 결제 준비
    1. 결제 화면을 띄운 뒤 '카카오 페이로 결제' 버튼 클릭

* 결제 과정(웹)
    1. '카카오 페이로 결제' 버튼을 클릭하면 QR코드가 나오는 페이지로 이동
    2. 카카오톡 결제 페이지에서 QR 코드를 스캔한 뒤 결제 정보 확인
    3. 결제 버튼 클릭
    4. 
* 결제 과정(앱)
    1. '카카오 페이로 결제' 버튼을 클릭하면 카카오톡 결제 화면으로 이동
    2. 결제 버튼 클릭
    3. Deeplink를 통해 strada 화면(결제 승인 단계)로 이동
    4. Strada payment ready api 호출, 결제 ready에 필요한 파라미터 전달

* 결제 승인
  1. 결제를 승인 버튼 클릭
  2. Kakao api로부터 결제 승인에 대한 response를 받으면 strada서버의 결제완료 api 호출
  3. 서버에서 승인 결과 db에 저장
  

### 6. Other Considerations

* 초기에는 Kakao의 payment-ready, payment-approval api 호출을 strada 서버에서 호출하려 했으나 몇 가지 문제가 발생하여
  Kakao api와의 연동은 클라이언트에서만 진행하고 strada 서버에는 결제에 대한 결과만 저장하도록 변경

* 몇 가지 문제:  
  1. 결제 승인에 pg_token과 tid가 필요
  2. tid는 결제 ready 단계에서 response
  3. pg_token은 결제 버튼을 클릭하면 자동으로 redirection하는 url의 path variable로 전달함
  4. 서버의 payment approval api에서 pg_token과 tid를 매핑하는 방법이 없음
  (공식 페이지와 블로그 설명으로는 tid를 자동 redirection할때 쿠키로 전달하라고 함)
  5. 아니면 payment approval api를 호출하는 웹서버 페이지를 하나 더 두는 방법도 고려해볼만함
  6. 서버의 payment approval api할때 자동 redirection이기 때문에 auth 인증 헤더를 전달할 수 없음

* 해결 방법1 -- X
  1. tid와 pg_token 맵핑은 home.js와 같이 별도의 결제 웹 서버를 만든다. 예제와 같이 웹 서버에서 ready api를 호출한 뒤 tid를 브라우저의 localstorage에 저장하고 redirection url로 넘어가자
  2. 웹 페이지는 결제 준비, 승인 페이지 2개로 나눈다.(승인 페이지는 페이지 진입 후 사용자와 상호작용 없이 바로 승인 api를 호출할수도 있다.)
  3. 위 방법처럼 클라이언트에서 모든 결제를 진행하고 결과만 strada서버에 넘기면 문제 6번도 자연스럽게 해결 가능하다.

* 해결방법2 -- Deep link 이용
  
  https://velog.io/@ragnarok_code/%EB%94%A5%EB%A7%81%ED%81%ACDeeplink%EB%9E%80 참고

  1. 해결 방법1의 별도의 웹 서버 대신 deep link를 이용한다. kakao ready api의 approval_url에 deeplink uri를 포함해 전송한다.
  2. Ready api를 호출하는 동시에 결제 ready 정보를 strada api의 kakao ready api로 전송한다. (tid 포함?)
  3. Deep link를 통해 인앱으로 redirect된 후 kakao approve api를 호출.
  4. 결제가 완료되면 response된 정보를 strada api에 전송.

### 7. Milestones

* 결제 취소, 실패
  1. 결제 취소, 실패는 클라이언트에서 발생함으로 이벤트가 발생했을 때 strada 서버에 알려야 한다.
  2. Strada api의 updateStatus api를 호출하여 대상 transaction의 상태를 업데이트 한다.

* 환불 - TODO

