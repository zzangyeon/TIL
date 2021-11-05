HttpSession 객체

 

- HTTP 프로토콜은 비연결형(stateless) 프로토콜(연결 -> 요청 -> 응답 -> 종료)

- 연속적인 사용자 정보가 보관되지 않음

ex ) 로그인 상태, 장바구니



해결방안

- 별도의 수단을 통해 각각의 클라이언트를 구분

- 클라이언트 별로 해당 정보를 유지

- 어떻게 클라이언트를 구분할 것인가 ? -> 쿠키와 세션으로 해결 가능





쿠키 vs 세션



쿠키 :

- 브라우저를 통해 클라이언트에 저장되는 사용자 정보

- (name, value) 쌍으로 이루어진 정보

- 초기에 웹 서버에 의해 HTTP Header에 포함되어 클라이언트에게 전송

- 이후에 접속마다 클라이언트가 웹 서버에게 재전송

- 보안적 취약성으로 인해 중요 정보를 저장하지 않아야 함



세션 :

- 사용자 정보를 서버에 저장

- 클라이언트의 최초 접속 시 새로운 세션을 생성하고 세션 ID 전송

- 이후 접속마다 클라이언트가 세션 ID 재전송

- 서버는 세션 ID에 해당하는 세션 정보를 획득

- 세션 ID 전송 수단으로 쿠키를 사용할 수 있음







세션 관리

- 대부분의 세션 관련 작업은 container가 처리

- Servlet은 request로 부터 HttpSession 객체를 제공받음

ex ) HttpSession session = request.getSession();



생성된 세션이 없었던 경우

- Container에 의해 처리

- 새로운 세션 ID 생성

- 세션 ID를 클라이언트에게 보낼 준비

- 새로운 HttpSession 객체 생성 후 Servlet에 제공



기존 세션이 있는 경우

- Request에 포함된 세션 ID에 해당하는 HttpSession 객체를 찾아서 제공





HttpSession Interface



void invalidate() : 현재 세션 종료





long getLastAccessedTime() : 마지막으로 세션에 관련된 클라이언트가 요청을 보낸 시간 제공



void setMaxInactiveInterval(int) : 초단위로 최대 유휴기간 설정



int getMaxInactiveInterval() : 초단위로 설정된 최대 유휴기간 리턴



void setAttribute(String, Obect)



Object getAttribute(String)





세션 Lifecycle



- 생성 : 최초로 request.getSession()이 호출될 때 container가 생성

- 사용 : 클라이언트가 세션 ID를 이용해 접속, request.getSession()을 통해 사용중인 HttpSession 객체 획득 후 사용

- 종료 : invalidate()가 호출되거나, 세션이 타임아웃 되었을 때 container가 소멸







Reference

https://eehoeskrap.tistory.com/5 
