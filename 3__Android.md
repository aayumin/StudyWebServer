## ⭐⭐⭐⭐ 챗GPT 가 생성한 학습자료입니다 ⭐⭐⭐⭐


# 📱 안드로이드 앱에서 Flask 서버 접속 (간단한 Integer 데이터 주고 받기)


---

<br>

### 📁 안드로이드 프로젝트 기본 구조

<br>

```css
MyApp/
├── app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/
│   │   │   │   └── com/example/myapp/
│   │   │   │       ├── data/          ← 데이터 클래스, Retrofit 관련
│   │   │   │       ├── network/       ← Retrofit 인터페이스 & 인스턴스
│   │   │   │       ├── ui/            ← 액티비티, 프래그먼트, ViewModel
│   │   │   │       └── MainActivity.kt
│   │   │   ├── res/                   ← 레이아웃, 이미지, 문자열 등
│   │   │   └── AndroidManifest.xml
│   └── build.gradle (Module: app)
├── build.gradle (Project)
└── settings.gradle
```


<br>
---

<br>


### 1. 필요한 라이브러리: Retrofit

**app/build.gradle (Module)에 추가:**

```gradle
// dependencies 블록 안에 추가
implementation 'com.squareup.retrofit2:retrofit:2.9.0'
implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
```

<br>

### 2. 데이터 클래스 정의

**com/example/myapp/data/NumberData.kt 에 추가:**

```kotlin
data class NumberData(val number: Int)
```

<br>


### 3. Retrofit 인터페이스 정의

**com/example/myapp/network/NumberApi.kt 에 추가:**

```kotlin
interface NumberApi {
    @GET("number")
    suspend fun getNumber(): NumberData

    @POST("number")
    suspend fun postNumber(@Body numberData: NumberData): Response<Unit>
}
```


<br>



### 4. Retrofit 인스턴스 생성

**com/example/myapp/network/RetrofitInstance.kt 에 추가:**

```kotlin
object RetrofitInstance {
    private const val BASE_URL = "http://<외부IP>:5000/"

    val api: NumberApi by lazy {
        Retrofit.Builder()
            .baseUrl(BASE_URL)
            .addConverterFactory(GsonConverterFactory.create())
            .build()
            .create(NumberApi::class.java)
    }
}
```



<br>



### 5. ViewModel 사용 예시

**com/example/myapp/ui/MainViewModel.kt 에 추가:**



```kotlin
fun uploadNumber(value: Int) {
    viewModelScope.launch {
        val result = RetrofitInstance.api.postNumber(NumberData(value))
        if (result.isSuccessful) {
            Log.d("Upload", "Success")
        }
    }
}

fun fetchNumber() {
    viewModelScope.launch {
        val data = RetrofitInstance.api.getNumber()
        Log.d("Fetch", "Current: ${data.number}")
    }
}
```


<br>



### 6. Flask CORS 설정

```bash
pip install flask-cors
```

```python
from flask import Flask, request, jsonify
from flask_cors import CORS

app = Flask(__name__)
CORS(app)  # 모든 출처(origin)에서의 요청 허용

# 서버에 저장할 숫자 변수 (간단한 메모리 저장)
stored_number = 0

# GET 요청: 현재 저장된 숫자 반환
@app.route('/number', methods=['GET'])
def get_number():
    return jsonify({'number': stored_number})

# POST 요청: 클라이언트로부터 숫자를 받아 저장
@app.route('/number', methods=['POST'])
def post_number():
    global stored_number
    data = request.get_json()
    if 'number' in data:
        stored_number = data['number']
        return '', 204  # No Content
    else:
        return jsonify({'error': 'number key is missing'}), 400

# 서버 실행
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

```

<br>

### ✅ 요청 성공을 위한 전제 조건 정리

| 항목 | 조건 |
|------|------|
| Flask 서버 | `host="0.0.0.0"` 로 실행해야 외부 접속 가능 |
| 포트포워딩 | 공유기에서 **5000번 포트 열기** |
| BASE_URL | **외부 IP 주소**로 설정 |
| CORS 허용 | `Flask-CORS` 사용하여 교차 요청 허용 |


<br>


### 🔍 안드로이드 앱에서 테스트 해보기

**📌 Flask 서버 로그 확인**

Flask 서버가 실행된 콘솔에서 요청 로그가 출력됩니다:

```
127.0.0.1 - - [GET] "/number" 200 OK
```

**📌 Android 로그 확인 (Logcat)**

```
Log.d("Fetch", "Current: ${data.number}")
```


**📌 테스트 팁**

- Android 에뮬레이터 사용 시 `http://10.0.2.2:5000` 로 설정
- 안드로이드 실기기에서 테스트 시, 실제 외부 IP 주소 사용
- 네트워크가 같은 Wi-Fi에 연결돼 있어야 테스트 가능

<br>

---


