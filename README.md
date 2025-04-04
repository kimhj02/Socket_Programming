# Socket Programming (Computer Network Class Practice)

## 📌 Purpose 1
TCP 소켓을 이용하여 클라이언트와 서버 간의 신뢰성 있는 데이터 전송 구조를 이해하고 직접 구현한다.

## 📌 Purpose 2
사용자 입력, 메시지 처리 옵션(일반, 대문자, 소문자), 종료 등 다양한 기능을 포함한 **에코 채팅 시스템**을 JSON 기반 프로토콜로 설계하고 구현한다.

## 📌 Purpose 3
서버와 클라이언트 간의 데이터 패킷을 **JSON 형식**으로 주고받음으로써, 통신의 구조화와 명확한 메시지 처리 방식을 실습한다. 또한, 입력 오류에 대한 예외 처리를 구현하여 **네트워크 프로그래밍의 안정성**을 확보한다.

---

## ✅ Result

- TCP 소켓을 활용한 에코 채팅 시스템 구현 완료
- 클라이언트는 사용자 이름 입력, 옵션 선택, 메시지 입력, 종료 기능을 수행
- 서버는 수신된 JSON 패킷을 파싱 후 옵션에 따라 메시지를 처리 및 응답
- 모든 통신은 **JSON 포맷**으로 구조화되어 있어 명확하게 처리 가능
- 옵션 입력 오류 발생 시, 상태 코드 및 오류 메시지를 포함한 **오류 패킷**을 클라이언트로 전송
- Jupyter Notebook 환경에서도 실행 가능하도록 서버를 **스레드 기반 비동기 처리**로 구현

> 예시 응답:  
> `[현진]: HELLO`  
> `에러 401: option input error: integer less than 1`

---

## 💻 실행 환경

- Python 3.8+
- Jupyter Notebook or Python Script (.py)
- 로컬 테스트용 `127.0.0.1` (loopback)
