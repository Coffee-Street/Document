## STRADA API 명세서

## API Specification

	STRADA 앱의 메인 화면 banner 정보를 API 서버에 요청한다.
 
## Banner API

  **Request**
  - http://{Server URL}:8080/strada/v1/banners
  
  **Header**
  - 로그인 시 받은 JWT Token
  - `Authorization: Bearer {YOUR-TOKEN}`

  **Request Parameters**
  - null
    
  **Request Body**
  - null

  **Response**
  - status code 200 success

  **Response type**
  - JSON List
  
    |Param|Type|Description|
    |------|---|---|
    |code|String|배너 이름|
    |title|String|배너 상단 타이틀|
    |imageUrl|String|배너 이미지 주소|
    |message|String|배너 하단 메시지|
    |backgoundColor|String|배너 배경 색|
    |fontColor|String|배너 글자 색|
        
  **Document**
  - title, message 개행
  상단 타이틀, 하단 메시지에 개행문자 '\n' 추가.
  클라이언트에서 개행 문자를 파싱해서 출력.
  
  - backgoundColor, fontColor
  배너의 배경 색과 문자의 font 색을 지정.
  Color의 hex 코드를 String 형식으로 저장.

  **example**

  
    [
      {
        "code":"banner1",
        "title":"example",
        "imageUrl":"http://example.com",
        "message":"example message",
        "backgroundColor":"#ffffff",
        "fontColor":"#000000"
      },
        {
        "code":"banner2",
        "title":"example2",
        "imageUrl":"http://example2.com",
        "message":"example message2",
        "backgroundColor":"#ffffff",
        "fontColor":"#000000"
        
      }
      
    ]
  
