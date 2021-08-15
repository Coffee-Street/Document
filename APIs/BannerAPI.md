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
        
    example
    ```
    [
      {
        "code":"banner1",
        "title":"example",
        "imageUrl":"http://example.com",
        "message":"example message"
      },
        {
        "code":"banner2",
        "title":"example2",
        "imageUrl":"http://example2.com",
        "message":"example message2"
      }
      
    ]
    ```
