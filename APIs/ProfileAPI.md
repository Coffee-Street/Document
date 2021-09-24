## STRADA API 명세서

## API Specification

	STRADA 앱의 메인 화면 profile 정보를 API 서버에 요청한다.
 
## Profile API

### 조회

  **Request**
  - uri: http://{Server URL}:8080/strada/v1/profiles/{id}
  - method: GET

  **Header**
  - 로그인 시 받은 JWT Token
  - `Authorization: Bearer {YOUR-TOKEN}`

  **Request Parameters**
  - null
    
  **Request Body**
  - null

  **Response**
  - status code 200 success

  - status code 401 unauthorized
  - status code 403 forbidden

  **Response type**
  - JSON List
  
    |Param|Type|Description|
    |------|---|---|
    |id|String|프로필 아이디|
    |userId|String|유저 핸드폰 번호|
    |point|String|유저 포인트|
        
    example
    ```
    [
      {
        "id":"1234",
        "userId":"010-1234-1234",
        "point":"2000"
      }
    ]
    ```

### 삽입

  **Request**
  - uri: http://{Server URL}:8080/strada/v1/profiles/
  - method: POST
  
  **Header**
  - 로그인 시 받은 JWT Token
  - `Authorization: Bearer {YOUR-TOKEN}`

  **Request Parameters**
  - null
    
  **Request Body**
  - ProfileRequest
    - userId (가능하면 서버 안에서 token으로부터 빼오고 싶음)
    - point

  **Response**
  - status code 200 success

  - status code 304 unauthorized
  - status code 400 bad request

  **Response type**
  - JSON List
  
    |Param|Type|Description|
    |------|---|---|
    |id|String|프로필 아이디|
    |userId|String|유저 핸드폰 번호|
    |point|String|유저 포인트|
        
    example
    ```
    [
      {
        "id":"1234",
        "userId":"010-1234-1234",
        "point":"2000"
      }
    ]
    ```
  
### 수정

  **Request**
  - uri: http://{Server URL}:8080/strada/v1/profiles/{id}
  - method: PUT
  
  **Header**
  - 로그인 시 받은 JWT Token
  - `Authorization: Bearer {YOUR-TOKEN}`

  **Request Parameters**
  - null
    
  **Request Body**
  - ProfileRequest
    - userId (가능하면 서버 안에서 token으로부터 빼오고 싶음)
    - point

  **Response**
  - status code 200 success

  - status code 304 unauthorized
  - status code 403 forbidden

  **Response type**
  - JSON List
    
    |Param|Type|Description|
    |------|---|---|
    |id|String|프로필 아이디|
    |userId|String|유저 핸드폰 번호|
    |point|String|유저 포인트|
        
    example
    ```
    [
      {
        "id":"1234",
        "userId":"010-1234-1234",
        "point":"2000"
      }
    ]
    ```
### 삭제

  **Request**
  - uri: http://{Server URL}:8080/strada/v1/profiles/{id}
  - method: DELETE
  
  **Header**
  - 로그인 시 받은 JWT Token
  - `Authorization: Bearer {YOUR-TOKEN}`

  **Request Parameters**
  - null
    
  **Request Body**
  - null

  **Response**
  - status code 200 success

  - status code 304 unauthorized
  - status code 403 forbidden

  **Response type**
  - null
  