1. 승인된 JavaScript 원본 (Authorized JavaScript origins)
- 역할: 프론트엔드 애플리케이션의 주소를 등록하는 곳입니다. Google은 이 주소에서 시작된 로그인 요청만 신뢰하겠다는 의미입니다.
- 이 주소는 프론트엔드가 백엔드에 "이제 로그인 시작해줘!"라고 요청하는 **시작점(Starting Point)**입니다. 

- 입력값: http://localhost:3000
 

 
2. 승인된 리디렉션 URI (Authorized redirect URIs)
- 역할: 사용자가 Google에서 성공적으로 로그인한 후, Google이 사용자를 **다시 돌려보낼 백엔드 서버의 주소(Callback URL)**를 등록하는 곳입니다.
- 이 주소는 Spring Security가 Google로부터 인가 코드(Authorization Code)를 받기 위해 대기하고 있는 표준 엔드포인트입니다. 즉, 인증 프로세스의 **도착지(Destination)**입니다.

- 입력값: http://localhost:8080/login/oauth2/code/google