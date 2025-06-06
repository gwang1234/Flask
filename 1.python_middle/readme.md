> 풀 스택 프레임워크: 프레임워크 안에 모든 기능을 가지고 있음  

> 마이크로 프레임워크: 웹 개발에 필요한 최소 기능 제공, 나머지는 다른 라이브러리나 프레임워크를 확장해 사용

플라스크는 마이크로 프레임워크다

<br>

### __name__ 이란
- __name__이라는 변수는 모듈의 이름이 저장됨
- 실행하는 코드에서는 __main__값이 들어감

### 시작점
- C, JAVA 등 보통의 언어는 코드를 시작하는 시작점을 가지고 있음
- 파이썬은 스크립트 언어라 시작점 없이 스크립트 코드를 바로 실행
    - 스크립트 언어: 컴파일 없이 바로 실행 

### flask 객체 생성
- Flask(__name__)으로 설정하여, 현재 위치를 flask 객체에 알려줘야 함
    - 이름을 변경해도 실행되지만, 일부 확장 기능 사용시 정상 동작되지 않음

### 라우팅 경로 설정
라우팅
- 적절한 목적지를 찾아주는 기능
- URL을 해당 URL에 맞는 기능과 연결해 줌
    - 예
        - http://www.web.com/hello
        - http://www.web.com 서버에서 hello라는 목적지에 맞는 함수를 호출해줌

```
    @app.route("/hello")
    def hello():
        return "<h1>Hello World</h1>"
```

### flask 웹 서버 구동
- app.run() 함수로 서버 구동 가능
    - host: 웹주소
        - 자신의 PC에서 웹서비스 구현
            localhost, 127.0.0.1 또는 0.0.0.0으로 host 설정 
    - port: 포트
    - debug: True or False
```
run(host=None, port=None, debug=True)
```

### 중첩 함수
- 함수 안에 함수가 있는것
- 내부 함수는 외부 함수의 변수값을 가져올 수 있으며, 내부 함수를 불러올때 괄호 하나 더 붙인다
```
def outer_function(name):
    def inner_function():
        print(f"Hello, {name}!")
        return "inner"
    return inner_function # 함수 자체를 리턴 

of = outer_function("Alice") # First-class 함수 
print(of())
```
결과값
```
Hello, Alice!
inner
```

### First-class 함수
- 다음과 같이 다룰 수 있는 함수를 First-class 함수라고 함 
    - 함수 자체를 변수에 저장 가능
        - ```
            def cal_square(x):
                return x * x
            func1 = cal_square
            print(func1(2)) 
          ```
    - 함수의 인자에 다른 함수를 인수로 전달 가능
        - ```
            def cal_square(x):
                return x * x
            def list_square(function, digit_list):
                result = list()
                for digit in digit_list:
                    result.append(function(digit))
                return result
            digit_list = [1, 2, 3, 4, 5]
            print(list_square(cal_square, digit_list))
            ```
    - 함수의 반환 값으로 함수를 전달 가능
        - ```
            def logger(msg):
                message = msg
                def msg_creator():
                    return ('[HIGH LEVEL]: ', message)
                return msg_creator
            log1 = logger("Hello World")
            log1()
            ```

### Closure function
- 함수와 해당 함수가 가지고 있는 데이터를 함께 복사, 저장해서 별도 함수로 활용하는 기법으로 First-class 함수와 동일
- 외부 함수가 소멸되더라도, 외부 함수 안에 있는 로컬 변수 값과 중첩함수를 사용할 수 있는 기법

### 파이썬 클로저 vs 자바 클로저 

| 구분      | 파이썬 클로저                                   | 자바 클로저 (람다식)                                 |
| --------- | ----------------------------------------------- | ---------------------------------------------------- |
| 지원 버전 | 처음부터 지원                                   | Java 8부터 지원                                      |
| 기본 개념 | 중첩 함수가 외부 변수를 캡처                    | 람다식이 외부 변수를 캡처                            |
| 캡처 방식 | 변수의 참조 (mutable 가능)                      | 변수의 값 (effectively final만 가능)                 |
| 변수 수정 | `nonlocal` 키워드로 수정 가능                   | 캡처한 변수는 final 또는 effectively final 이어야 함 |
| 내부 구현 | `__closure__`에 cell object로 참조 저장         | JVM이 값 복사 or 참조를 캡처 (불변으로 취급)         |
| 예측성    | 유연하지만 주의 필요 (mutable side effect 가능) | 보다 안정적 (불변 중심)                              |
| 쓰임새    | 함수형, 데코레이터, 상태 유지 등 다양           | 스트림, 콜렉션 처리, 비동기 콜백 등                  |


### `*args`와 `**kwargs`
| 표현       | 의미                              | 쉽게 말하면                           |
| ---------- | --------------------------------- | ------------------------------------- |
| `*args`    | 위치 인자들을 튜플로 받는다       | 함수 호출 시 여러 개의 일반 인자 받기 |
| `**kwargs` | 키워드 인자들을 딕셔너리로 받는다 | 이름 붙은 인자들을 받기               |

### 데코레이터
- Decorator는 기존 함수를 수정하지 않고 그 기능을 확장하는 방법을 제공
- '@' 기호를 사용하여 정의되며, 함수나 메소드 앞에 위치
    - 사용 사례
        - Flask 웹 프레임워크 라우팅
        - 로깅(Logging)이 필요할 때
        - 사용자 인증