## DispatcherServlet

- dispatch 보내다
- HTTP 프로토콜로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 위임해주는 프론트 컨트롤러(Front Controller)로 정의
- 즉 controller보다 앞에 있다고 이해
- dispatcher-servlet이 어플리케이션으로 들어오는 모든 요청 핸들링 및 공통 작업 처리

### static resource 처리

- 모든 요청 ( 이미지, html, css, javascript 요청도 모두 거쳐감)
- 두 가지 방법을 고안

1. 정적 자원에 대한 요청과 애플리케이션 요청 분리

- /apps url은 dispatcher servlet
- /resources url 담당하지 않음
- url 붙여줘야해서 지저분

2. 애플리케이션 요청 탐색하고 없으면 정적 자원에 대한 요청으로 처리

- dispatcher servlet 요청 처리할 컨트롤러 먼저 찾고
- 없으면 resource 경로 탐색하여 자원 탐색
