## ⭐⭐⭐⭐ 챗GPT 가 생성한 학습자료입니다 ⭐⭐⭐⭐

# 📡 REST API 기초 완전 정복



# 📚 목차

1. ✅ REST API란 무엇인가?
  
2. 🌐 REST API 실제 사용법 (URI, 포트, 요청 방식)

3. 🔍 REST API의 실제 동작 과정

4. 🛠️ Postman 설치 및 사용법
---

# ✅ 1) REST API란 무엇인가?

### 📘 REST의 정의
- **REST** = Representational State Transfer
- **API** = Application Programming Interface  
→ REST API는 **웹에서 서버와 통신할 수 있도록 하는 규칙**


### 📦 쉽게 말하면?

> “정해진 규칙대로 요청을 보내면, 서버가 데이터를 주거나 처리해주는 웹 주소 체계”

예를 들어:
- `GET /notes` → 노트 목록 조회
- `POST /notes` → 새 노트 작성
- `DELETE /notes/1` → ID가 1인 노트 삭제

### 🌍 브라우저 주소창과 REST API

우리는 크롬이나 엣지 같은 웹 브라우저의 **주소창(URL 입력창)** 에 다음과 같이 입력할 수 있어요:

```
http://localhost:5000/notes
```

- 이 주소는 사실상 **REST API 서버에 GET 요청**을 보내는 것과 같아요.
- 브라우저는 내부적으로 `GET` 방식으로 요청을 보내고, 서버가 JSON 같은 응답을 주면 화면에 보여줘요.
- 주소창에 입력하는 이 URL은 **REST API의 URI**에 해당해요.

즉, 브라우저는 우리가 특별한 도구 없이도 REST API를 테스트할 수 있는 가장 간단한 클라이언트예요.


### ✋ REST API의 특징

** 1. 무상태성 (Stateless)**

> 서버는 클라이언트의 이전 요청 상태를 기억하지 않음

```
예시:
- 요청 1: GET /notes/1
- 요청 2: GET /notes/2
```

→ 두 요청은 **서로 완전히 독립적**이며, 서버는 요청 1의 내용을 **기억하지 않음**

> 클라이언트는 **모든 정보를 매번 요청에 담아야** 함

**2. 리소스 지향 (Resource-Oriented)**

> 모든 데이터(게시글, 유저, 메모 등)를 **리소스**라고 부르고,  
> 각각의 리소스는 **고유한 URI**로 식별됨

📌 예시:

```
- GET /users → 사용자 목록 조회
- GET /users/3 → ID가 3인 사용자 정보 조회
- DELETE /notes/5 → ID가 5인 노트 삭제
```

→ **URI는 마치 파일 경로처럼 각 리소스를 명확하게 표현**



---

# 🌐 2) REST API 실제 사용법 (URI, 포트, 요청 방식)



### 📌 URI란?
> URI는 요청을 보낼 서버 주소

```
http://localhost:5000/notes
```

- `http` → 통신 방식
- `localhost` → 서버 주소 (내 컴퓨터)
- `5000` → 포트 번호
- `/notes` → 요청 경로



### 🧪 실제 요청 예시

#### 📥 GET 요청
```http
GET /notes HTTP/1.1
Host: localhost:5000
```

#### 📤 POST 요청
```http
POST /notes HTTP/1.1
Host: localhost:5000
Content-Type: application/json

{
  "title": "메모 제목",
  "content": "메모 내용"
}
```

---

# 🔍 3) REST API는 실제로 어떻게 동작하는가?



### 📶 전체 동작 흐름

1. 클라이언트(앱, 브라우저)가 서버에 **HTTP 요청 전송**
2. 서버는 요청을 받아 처리하고,
3. 클라이언트에게 **HTTP 응답 반환**



### 📦 요청(Request) 구성 요소

| 구성 요소 | 설명 |
|-----------|------|
| Method | 요청 종류 (GET, POST 등) |
| URL | 요청 주소 (`/notes/3` 등) |
| Headers | 부가 정보 (Content-Type 등) |
| Body | 데이터 본문 (JSON 등, POST/PUT에서 사용) |



### 📦 응답(Response) 구성 요소

| 구성 요소 | 설명 |
|-----------|------|
| Status Code | 처리 결과 코드 |
| Headers | 응답 정보 (Content-Type 등) |
| Body | 실제 데이터 (JSON 등) |



### 🔢 주요 상태 코드 정리

| 상태 코드 | 의미 |
|-----------|------|
| 200 | 성공 (OK) |
| 201 | 생성됨 (Created) |
| 400 | 잘못된 요청 (Bad Request) |
| 404 | 존재하지 않음 (Not Found) |
| 500 | 서버 오류 (Internal Server Error) |

---

# 🛠️ 4) Postman 설치 및 간단한 사용법



### 🖥️ Postman이란?

> REST API를 **직접 테스트**할 수 있는 도구  
> 버튼 클릭만으로 요청을 보내고 응답을 확인할 수 있음



### 📥 설치 방법

1. [https://www.postman.com/downloads/](https://www.postman.com/downloads/)
2. 운영체제에 맞는 설치파일 다운로드
3. 설치 후 회원가입 (또는 게스트로 사용)



### 🧪 GET 요청 예시 (데이터 조회)

1. Postman 실행
2. 상단에서 `GET` 선택
3. 주소 입력: `http://localhost:5000/notes`
4. "Send" 클릭
5. 하단에 응답(JSON)이 표시됨



### 📤 POST 요청 예시 (데이터 전송)

1. 상단에서 `POST` 선택
2. 주소 입력: `http://localhost:5000/notes`
3. 아래 탭에서 **Body → raw → JSON** 선택
4. 데이터 입력:

{
  "title": "메모 제목",
  "content": "메모 내용"
}

5. "Send" 클릭 → 서버에 데이터 전송

---

# ✅ 정리 요약

| 항목 | 요약 |
|------|------|
| REST API | 웹 서버와 통신하는 규칙과 방법 |
| URI | 요청할 리소스의 위치 표현 |
| Method | GET, POST, PUT, DELETE 등 |
| Request | 클라이언트 → 서버로 보내는 정보 |
| Response | 서버 → 클라이언트로 보내는 응답 |
| Postman | REST API를 쉽게 테스트할 수 있는 도구 |

---
