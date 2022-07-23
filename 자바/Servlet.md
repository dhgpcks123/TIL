## Servlet (server + applet의 합성어)

- java 언어를 이용해 사용자 요청 받아 처리하고 결과를 다시 전송하는 class 파일을 servlet이라고 한다
- 웹에서 동적인 페이지를 java로 구현한 서버측 프로그램
- 모든 서블릿은 javax.servlet.Servlet 인터페이스 상속 받아 구현

### 특징

- 클라이언트 요청에 대해 동적으로 작동하는 웹 어플리케이션 컴포넌트
- html을 사용하여 응답
- java thread 이용 하여 동작
- MVC 패턴의 controller
- HTML 변경 시 servlet 재컴파일 필요

### 동작방식

![servlet동작방식](img/servlet동작방식.png)

1. 사용자가 URL 입력 시 HTTP Request - Servlet Container로 전송
2. 요청받은 Servlet Container는 HttpServletRequest, HttpServletResponse 객체 생성
3. 사용자가 요청한 URL이 어느 서블릿에 대한 요청인지 찾음
4. 해당 서블릿에서 service 메서드 호출 - doGet 또는 doPost 호출
5. 동적 페이지 생성 -> HttpServletResponse 객체 응답
6. HttpServletRequest, HttpServletResponse 객체 소멸
