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


### ✅ 1. 필요한 라이브러리: Retrofit

**app/build.gradle (Module)에 추가:**

```gradle
// dependencies 블록 안에 추가
implementation 'com.squareup.retrofit2:retrofit:2.9.0'
implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
```

<br>

### ✅ 2. 데이터 클래스 정의

**com/example/myapp/data/NumberData.kt 에 추가:**

```kotlin
data class NumberData(val number: Int)
```

<br>


### ✅ 3. Retrofit 인터페이스 정의

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



### ✅ 4. Retrofit 인스턴스 생성

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



### ✅ 5. ViewModel 사용 예시

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



### ✅ 6. 요청 성공을 위한 조건 정리

| 항목 | 조건 |
|------|------|
| Flask 서버 | `host="0.0.0.0"` 로 실행해야 외부 접속 가능 |
| 포트포워딩 | 공유기에서 **5000번 포트 열기** |
| BASE_URL | **공인 IP 주소**로 설정 |
| CORS 허용 | `Flask-CORS` 사용하여 교차 요청 허용 |


<br>



### 🎁 Flask CORS 설정 예시

```bash
pip install flask-cors
```

```python
from flask_cors import CORS

app = Flask(__name__)
CORS(app)
```

<br>


---
