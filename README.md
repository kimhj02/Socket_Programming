# 프로토콜 설계

## 클라이언트 → 서버 (JSON 형식)

```json
{
	"user" = "클라이언트 이름"
	"echooption" = 옵션 숫자(1~4 선택)
	"message" = "보낼 메시지"option input error: non-integer
}
```

- 클라이언트가 입력한 이름, 메시지, 옵션 값을 기반으로 JSON 형식의 요청 패킷을 구성하여 서버에 전송
- JSON 구성
    - “user” : 메시지를 보낸 사용자 이름
    - “echooption” : 서버에서 메시지를 처리할 방식 선택(1~4)
    - “message” : 클라이언트가 보낼 실제 메시지 내용

## 서버 → 클라이언트 (정상 경우에는 패킷을 전송하지 않음)

## 서버 → 클라이언트 (오류 응답 패킷을 전송)

```json
{
  "status": 오류코드,
  "error": "오류 설명"
}
```

- 클라이언트의 잘못된 요청이나 서버 처리 중 예외가 발생한 경우, 적절한 오류 코드를 포함한 JSON 응답을 보냄
- 예시

```json
{
  "status": 403, //오류 코드 403
  "error": "option input error: non-integer"
}
```

- 생길 수 있는 예외

| 구분 | 오류 내용 | 오류 코드 | 오류 메시지 |
| --- | --- | --- | --- |
| 클라이언트 | 0 이하의 정수 입력 오류 | 401 | option input error: integer less than 1 |
| 클라이언트 | 5 이상의 정수 입력 오류 | 402 | option input error: greater than 4 |
| 클라이언트 | 정수가 아닌 실수로 입력 오류 | 403 | option input error: non-integer |
| 클라이언트 | 문자 입력 오류 | 405 | option input error: non-numeric input |

---

## 패킷의 송신 수신

1. 클라이언트는 사용자로부터 입력을 받아 이 정보를 바탕으로 JSON 형태로 패킷을 생성하고 서버로 전송
2. 서버는 JSON 패킷을 수신 후 옵션에 따라 메시지를 처리하거나 오류를 판단하여 응답을 보냄
3. 클라이언트는 응답을 받아 출력하고, 옵션 4가 입력으로 들어오면 패킷을 전송하지 않고 종료

## 동작 설명

1. 서버 실행
2. 클라이언트 실행
3. 클라이언트 시작 시 사용자 이름을 입력받음(이름은 한번만 입력 받음)
    - 클라이언트는 user, echooption, message를 포함한 JSON 패킷을 서버로 전송
    - 서버는 이 JSON을 파싱하여 check_message() 함수로 유효성 검사를 수행
        - 이 함수에서는 예외 처리와 옵션에 따른 메시지를 처리함
        - 예외
            - 옵션 입력이 1 미만인 경우 : 상태 코드 401
            - 옵션 입력이 4 초과인 경우 : 상태 코드 402
            - 옵션 입력이 실수인 경우 : 상태 코드 403
            - 옵션 입력이 문자인 경우 : 상태 코드 405
            - 예외인 경우에는 {status, error} 반환
        - 실제 메시지 처리
            - option == 1 : 그대로
            - option == 2 : 대문자 (upper 사용)
            - option == 3 : 소문자 (lower 사용)
            - option == 4 는 클라이언트에서 처리, 서버에는 넘어오지 않음
    - 정상 입력인 경우, 서버에서 Before 과 After 출력 및 클라이언트로 정상 패킷 전송{status, message}
        - 서버로부터 받은 JSON 패킷을 파싱하여 정상 변경 메시지 출력
    - 옵션 입력에 오류가 있을 경우에는 클라이언트로 오류 패킷 전송{status, error}
        - 서버로부터 받은 JSON 패킷을 파싱하여 오류 내용 출력

---

# 환경 설정 방법

- 개발 언어 : python
- 개발 환경 : visual studio code + Jupyter notebook
- 사용 라이브러리 : socket, json, threading
    - socket : 클라이언트와 서버 간의 TCP 통신을 위한 라이브러리
    - json : 메시지를 JSON 형식으로 직렬화/역질렬화하여 데이터를 주고받기 위해 사용
    - threading : 서버에서 여러 클라이언트를 동시에 처리할 수 있도록 스레드 시간 처리에 사용

# 프로그램 실행 방법

- visual studio code에서 Jupyter notebook을 통해 프그램을 작성했음으로, 하나의 ipynb 파일에 두개의 python 코드가 존재
    - 따로 코드의 이름이 존재하지 않기에 구분을 위해 서버 코드(server.py), 클라이언트 코드(client.py)로 구분함

### 실행 순서

1. 서버 실행
    - [server.py](http://server.py) 셀 선택 후 shift + enter 또는 ▶ 버튼 클릭
2. 클라이언트 실행
    - [client.py](http://client.py) 셀 선택 후 shift + enter 또는 ▶ 버튼 클릭
3. 클라이언트 콘솔에서 사용자 이름, 옵션(1~4), 메시지를 입력하면 결과가 출력됨

---

# 실제 실행 화면

## 정상적인 경우

1. 사용자 이름 입력

![image.png](attachment:1443dcc9-3a0b-4c45-892c-5bcaa46ae3ed:image.png)

1. 옵션 입력(대문자)

![image.png](attachment:49ae200f-6b93-4dd9-aa38-7ae9067e7b48:image.png)

1. 메시지 입력

![image.png](attachment:ec089ae7-7f22-434d-ac2c-4cc1f0bc2582:image.png)

1. 출력
    1. 서버
    
    ![image.png](attachment:097cafb7-110e-49e8-a79d-9ec8ae22406d:image.png)
    
    1. 클라이언트
    
    ![image.png](attachment:52ec2906-a147-412b-af48-40be953b7379:image.png)
    
2. 종료 (옵션 4 입력)

![image.png](attachment:7cc83359-06cc-47ba-b626-8531501b7d1a:image.png)

## 잘못된 입력이 들어온 경우

### 경우 1 : 옵션 1 미만

![image.png](attachment:73e73cb5-2a60-41f0-b165-c76c9d518454:image.png)

![image.png](attachment:5a7024fb-89ea-438b-ad7c-a064582d5eaa:image.png)

### 경우 2 :  옵션 4 초과

![image.png](attachment:11b1107a-e1a3-434f-8ea3-8ed2acacdff4:image.png)

![image.png](attachment:d9087cc4-1a64-41fb-b875-ac6d3e3d671b:image.png)

### 경우 3 : 옵션 실수

![image.png](attachment:01a43d2f-07fb-42c4-a59b-81a243c0a6b1:image.png)

![image.png](attachment:62959a60-9fd1-448f-8d27-7611237f81af:image.png)

### 경우 4 : 옵션 숫자 아님

![image.png](attachment:b175943e-aef3-4c26-b2fe-d73f1a751185:image.png)

![image.png](attachment:5c8ef7bd-a19f-4ad5-abf6-c6cd3e84719e:image.png)

### 오류 처리 시 서버 출력

![image.png](attachment:16e271f7-31b3-48e4-b4ad-b77adbe88fbe:image.png)

# 소감

소켓 프로그래밍을 처음 시도했는데 python으로 작성하니 생각보다 쉽게 만들 수 있었다. 다만, visual studio code를 사용하여 Jupyter notebook을 사용할 때, 서버와 클라이언트를 같은 ipynb에서 시행하면 서버의 출력이 나오지 않아 다른 파일로 나누어 작성했다.

또한 이번 과제를 수행하면서 비록 혼자하는 과제이지만, version 관리를 하기 위해 github의 pull request를 사용하여 여러가지 version을 나누어 코드를 작성했다. 이렇게 version을 나누어 작성하니 뭔가 잘못되더라도 다시 돌아갈 수 있다는 것이 가장 큰 장점인것같다.
