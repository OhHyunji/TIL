# ObjectMapper

## JsonString to Object

```java
@Test
public void testJsonStrToObject() {
    String jsonStr = "{\"name\":\"olaf\",\"age\": 20}";

    ObjectMapper mapper = new ObjectMapper();
    try {
        User user = mapper.readValue(jsonStr, User.class); // User에 setter가 있어야함.
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

### 필요한 property만 쓰고싶다. 

User에는 email 프로퍼티가 없는데 jsonStr에 email이 추가되었다.

```java
String jsonStr = "{\"name\":\"olaf\",\"age\": 20, \"email\": \"aaa@gmail.com\"}";
```

UnrecognizedPropertyException 발생.

```
com.fasterxml.jackson.databind.exc.UnrecognizedPropertyException: 
Unrecognized field "email" (class com.ex.User), not marked as ignorable (2 known properties: "name", "age"])
...
```

전달해주는 프로퍼티를 모두 다 받아서 처리해야하는게 아니라면
내가 필요한 값만 User 클래스에 정의해서 바인드 시키면된다.

unknownProperties를 무시하도록 아래 설정을 추가한다.

```java
mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
```

참고: [jackson JSON 사용 중 UnrecognizedPropertyException 발생 시](https://lng1982.tistory.com/180)

### 좀 더 이쁘게: method 분리, generic

```java
@Test
public void testJsonStrToObject() {
    String jsonStr = "{\"name\":\"olaf\",\"age\": 20, \"email\": \"aaa@gmail.com\"}";
    User user = convertJsonStrToObject(jsonStr, User.class);
}

private <T>T convertJsonStrToObject(String jsonStr, Class<T> resultType) {
    T result = null;

    ObjectMapper mapper = new ObjectMapper();
    mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);

    try {
        result = mapper.readValue(jsonStr, resultType);
    } catch (IOException e) {
        e.printStackTrace();
    }

    return result;
}
```

