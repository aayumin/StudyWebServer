## ⭐⭐⭐⭐ 챗GPT 가 생성한 학습자료입니다 ⭐⭐⭐⭐

# 🌐 웹 서버와 Python Flask

<br>


# 📚 목차 (Table of Contents)

1. 🌐 웹 서버란 무엇인가?
   
2. 🔨 localhost에서 Flask 서버 실행하기 
   
3. 🚪 Flask 서버를 외부에서 접속 가능하게 하려면?

<br>

---


# 🌐 1. 웹 서버란 무엇인가? 

<br>

### 🌍 웹 서버란?

- 웹 서버는 사용자의 요청(예: 웹페이지 보기, 데이터 전송 등)을 받아서,
  처리한 후 **응답을 돌려주는 컴퓨터 프로그램 또는 시스템**입니다.
- 웹 브라우저(크롬 등)는 이 웹 서버에 요청을 보내고, 웹 서버는 그에 맞는 HTML, JSON, 파일 등을 돌려줍니다.

> 쉽게 말해:  
> 🧑‍💻 사용자가 요청 → 🌐 웹 서버가 받음 → 📦 처리하고 → 🔁 결과를 사용자에게 응답


<br>


### 🐍 Flask란?

- **Flask**는 Python으로 만든 **간단한 웹 서버 프레임워크**입니다.
- 빠르게 웹 API나 서버를 만들 수 있도록 도와줍니다.

> Flask는 마치 Python으로 웹서버를 쉽게 열 수 있게 해주는 **작은 가게 텐트** 같은 것!

<br>

### ✅ 웹 서버는 어떻게 통신이 되는가?


1. 사용자가 브라우저에 주소를 입력하거나 앱에서 요청을 보냄  
   (예: `http://localhost:5000/hello`)

2. 컴퓨터는 해당 주소의 서버에 **HTTP 요청**을 보냄

3. 서버는 요청을 받아서 처리한 후, **HTTP 응답(Response)** 을 돌려줌  
   (예: JSON, HTML 등)

4. 클라이언트(브라우저 또는 앱)는 그 응답을 화면에 보여줌

> 내부적으로는 **IP 주소와 포트 번호**를 기반으로,  
> **TCP 소켓 통신**을 통해 데이터가 왔다 갔다 합니다.

<br>


### 🏠 localhost에서 웹 서버 실행하기란?

- 웹 서버를 **내 컴퓨터 안에서만 접근 가능하게** 실행하는 방식입니다.
- 주소는 보통 `localhost` 또는 `127.0.0.1`을 사용합니다.

📌 예시:
```
http://localhost:5000/hello
```
- 이 주소는 **"내 컴퓨터가 자기 자신에게 보내는 요청"** 입니다.  
- 외부(다른 컴퓨터나 스마트폰)에서는 **접속할 수 없습니다**.


<br>


### 🛜 외부에서 접속 가능하게 웹 서버 실행하기란?

- 웹 서버를 **다른 컴퓨터, 스마트폰 등 외부 기기에서도 접근 가능하게 실행**하는 방식입니다.
- 보통 **내 컴퓨터의 IP 주소**를 통해 접속할 수 있게 설정합니다.

```
http://<내 컴퓨터 IP 주소>:포트번호
```

- 외부에서 내 컴퓨터에 접속하려면, 내 컴퓨터가 외부 요청을 받을 수 있도록 설정해야 합니다.



<br>

---


# 🔨 2. localhost에서 Flask 서버 실행하기

<br>




### 🔧 사전 준비

1. Python 설치 (https://www.python.org/)
2. Flask 설치
```bash
pip install flask
```


<br>


### 🚀 서버 코드 작성 (hello_server.py)

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

# 간단한 데이터 저장소
data_store = {}

# 데이터 저장 (POST)
@app.route('/save', methods=['POST'])
def save_data():
    json_data = request.get_json()
    data_store['value'] = json_data.get('value')
    return jsonify({"message": "저장 완료!"})

# 데이터 조회 (GET)
@app.route('/load', methods=['GET'])
def load_data():
    return jsonify({"value": data_store.get('value', None)})

if __name__ == '__main__':
    app.run(debug=True)
```


<br>


### ▶️ 서버 실행

```bash
python hello_server.py
```

- 기본적으로 `http://127.0.0.1:5000` 에 서버가 열립니다.


<br>


### 🧪 테스트

1. [POST 요청] 저장:
```bash
curl -X POST http://localhost:5000/save -H "Content-Type: application/json" -d "{"value": 42}"
```

2. [GET 요청] 불러오기:
```bash
curl http://localhost:5000/load
```

<br>

---

# 🚪 3. Flask 서버를 외부에서 접속 가능하게 하려면?

### 🛠 서버 설정 변경

```python
# app.run(debug=True) → 아래처럼 변경
app.run(debug=True, host='0.0.0.0')
```

- `0.0.0.0`은 **"모든 외부 IP에서 접근 가능"** 하도록 설정하는 의미입니다.


<br>


### 📡 공유기 포트 포워딩 (ipTIME 기준)

1. 브라우저에서 `192.168.0.1` 접속 (공유기 관리자 페이지)
2. 로그인 후, **관리도구 → 고급 설정 → NAT/라우터 관리 → 포트포워딩** 메뉴 이동
3. `새규칙 추가` 클릭:
4. 규칙 내용 작성:
   - **규칙이름**: 아무거나 입력
   - **내부 IP 주소**: 현재 Flask 서버를 실행하는 PC의 IP (예: 192.168.0.5)
   - **내부 포트 / 외부 포트**: `5000`
   - **프로토콜**: TCP
5. `적용` 클릭

<br>


### 🌍 외부에서 접속 테스트

- 스마트폰이나 다른 기기에서 접속:
```
http://<공인IP>:5000/load
```

(공인IP는 https://ip.pe.kr/ 등에서 확인 가능)

> 단, 방화벽이나 보안 프로그램이 5000번 포트를 막고 있다면 접속이 안 될 수 있으니 예외 설정 필요!

✅ 이로써 로컬 Flask 서버를 외부에서도 접근할 수 있도록 설정 완료!

<br>

---



