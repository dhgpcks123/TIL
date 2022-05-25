# OAuth2

## - 과정
OAuth -> Open Auth : 오픈 된 인증처리 ( 인증 처리를 대신 해준다 )

    OAuth2.0 인증 흐름도, SPA+Restful API, 구글
    (클라이언트)                                        인증 서버         자원 서버
    SPA -------------------------------------------- Google API----- Google 자원 서버
        1. 로그인 버튼 클릭 시, 구글에게 요청 전송                                          : get 요청 /oauth2/authorization/google
        ------------------------------------------>>>
        2. 미 로그인 시 로그인 창
        <<<------------------------------------------
        3. 로그인 / 권한요청
        ------------------------------------------>>>
        4. 권한 허가-일회용 Authorization 코드전달
        <<<------------------------------------------
        5. 일회용 Authorization 코드 전송 - 인증하기 완료! : )

======================================================================================

    SPA --------------->>> REST API                   Google API      Google 자원 서버
        6. 받은 코드로 access_token, id_token 주세요
                                    --------------->>>                            token-uri : https://nid.naver.com/oauth2.0/token
        7. access_token, id_token 발급(정보 접근 권한 부여)
                                    <<<---------------                            redirectUrl: 우리집 주소
        8. 발급받은 *access_token* 으로 정보 조회 요청
                                    --------------------------------->>>          user-info-uri: https://openapi.naver.com/v1/nid/me
        9. 정보 반환
                                    <<<---------------------------------          redirectUrl 우리집으로, scope: profile, email
        10. DB 확인, JWT 토큰과 함께 정보 반환
    SPA <<<--------------- REST API                   Google API      Google 자원 서버


스프링 공식 OAuth 제공 Facebook, github, google 등..
네이버, 카카오는 직접 구현해주도록 한다


1.로그인 요청 주소  
https://kauth.kakao.com/oauth/authorize?client_id=xxxx&redirect_uri=http://localhost:8080/auth/kakao/xxxxx&response_type=code
로그인 요청을 client_id와 redirect_uri를 적어서 요청한다.  
이 과정은 클라이언트가 인증서버에 요청하는 주소이며,  
 여기서 client_id와 redirect_uri를 통해 인증 된 코드가 적절한 곳에서 사용되는지 확인할 수 있다.


4.응답받은 코드  
localhost:8080/auth/kakao/callback?code=xxxxxxx코드 값을 받았다.  
 일회용 Authoriztion 인증 코드를 받은 것!

@GetMapping(/auth/kakao/callback)
public @ResponseBody String kakaoCallback(String code){
    return code; //응답받은 코드의 코드 값을 받게 된다.
}

6-7.받은 코드로 인증 토큰을 받는다  
access_token, id_token, POST방식으로 요청  
/oauth/token HTTP/1.1  
Host: kauth.kakao.com  
    => https://kauth.kakao.com/oauth/token  
Content-Type application/x-wwww-form-urlencoded;charset=utf-8  
body에 담아서 전달해야하는 데이터   
- grant_type(authorztion_code로 고정)
- client_id 
- redirect_uri
- code
- client_secret

```java
@GetMapping(/auth/kakao/callback)
public @ResponseBody String kakaoCallback(String code){
    // HttpURLConnection(과거 방식)
    RestTemplate restTemplate = new RestTemplate();
    //Header생성
    HttpHeaders headers = new HttpHeaders();
    headers.add("Content-type", "application/x-www-form-urlencoded;charset=utf-8");
    
    // body 생성
    MultiValueMap<String, String> params = new LinkedMultiValueMap();
    params.add("grand_type", "authorization_code");
    params.add("client_id", "xxx");
    params.add("redirect_uri","xxx");
    params.add("code","xxx")

    // 셋팅
    HttpEntity<MultiValueMap<String,String >> kakaoTokenRequest =
        new HttpEntity<>(params,headers);

    // Http 요청하기
    ResponseEntity<String> response = rt.exchange(
            "https://kauth.kakao.com/oauth/token",
            HttpMethod.POST,
            kakaoTokenRequest,
            String.class
        )

    response.getBody();
    // ==> response에 토큰 응답이 온다.
    //    (토큰 타입, 액세스 토큰, 토큰 만료시간, 리프레시 토큰, 리프레시 토큰 만료시간, 스코프 등)
    // access_token으로 카카오 로그인 요청한 회원정보 요청할 수 있다.
}
```

### OAuth_token 정보  
- String access_token;
- String token_type;
- String refresh_token;
- Integer expired_in;
- String scope;
- Integer refresh_token_expires_in;

Gson, Json, ObjectMapper등을 이용해 VO에 저장

```java
//OAuthToken VO
OAuthToken oauthToken = objectMapper.readValue(response.getBody(), OAuthToken.class);
```

8.AccessToken사용해서 정보 요청  
GET/POST /v2/user/me HTTP/1.1  
Host: kapi.kakao.com  
Authorization: Bearer {access.token}  
Content-type: application/x-www-form-urlencoded;charset=utf-8  

Header: AccessToken사용  
Authorization, Authorization: Bearer $(access_token)  

```java
    RestTemplate rt = new RestTemplate();

    //Header생성
    HttpHeaders headers = new HttpHeaders();
    headers.add("Authorization", "Bearer "+oauthToken.getAccess_token())
    headers.add("Content-type", "application/x-www-form-urlencoded;charset=utf-8"); 

        // 셋팅
    HttpEntity<MultiValueMap<String,String >> kakaoProfileRequest =
        new HttpEntity<>(headers);

    ResponseEntity<Strring> response = rt.exchange(
            "https://kapi.kakao.com/v2/user/me",
            HttpMethod.POST,
            kakaoTokenRequest,
            String.class
        )

    response.getBody();

```