# Singleton pattern

TIL

```
싱글톤 패턴:
특정 클래스에 대해 객체 인스턴스가 하나만 만들어지도록 하는 패턴이다.

싱글톤 패턴을 사용하면
1. 객체 인스턴스가 하나만 만들어지고
2. 어디서든 객체 인스턴스에 접근할 수 있다.

기본적인 구현법은 간단하지만,
멀티스레딩 환경을 고려해야한다.
```



## 싱글턴 패턴

유일무이한 객체가 필요한 상황이 있다.

* 예: threadPool, connectionPool, 사용자 설정,  레지스트리 설정을 처리하는 객체, 디바이스 드라이버 등
* 이런 객체는 인스턴스를 두개 이상 만들게 되면 프로그램이 이상하게 돌아간다든가, 자원을 불필요하게 잡아먹는다든가, 결과에 일관성이 없어지는 등 심각한 문제가 생길 수 있다.



싱글톤 패턴

* 특정 클래스에 대해 객체 인스턴스가 하나만 만들어지도록 하는 패턴이다.



싱글톤 패턴을 사용하면

1. 객체 인스턴스가 하나만 만들어진다.

2. 전역변수를 사용할 때 처럼 객체 인스턴스에 어디서든지 액세스 할 수 있다.

   

전역변수로 선언하는것보다 싱글턴 패턴을 사용하면 좋은점?

* 전역변수는 애플리케이션이 시작될 때 객체가 생성된다. 그런데 만약 그 객체가 프로그램이 종료될때까지 한번도 안쓰이는데 자원을 많이 차지한다면? 비효율적이다. 낭비다.
* 싱글턴패턴을 사용하면 필요한 상황이 생겼을 때 객체를 만들 수 있다. (lazy instantiation)



## 고전적인 싱글턴 패턴 구현법

```java
public class Singleton {
    // 1. private static 인스턴스 변수 선언
    private static Singleton uniqueInstance;
    
    // 2. 생성자를 private 으로 선언
    private Singleton() {}
    
    // 3. instantce 를 받을 메소드 구현
    public static Singleton getInstance() {
        if(uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }
}
```

`getInstance()`에서 null체크를 하고 인스턴스를 생성하기 때문에

1. 인스턴스가 딱 한번만 생성되고
2. 인스턴스가 필요한 상황이 닥치기 전에는 아예 인스턴스를 생성하지 않는다.
   (lazy instantiation)



## 멀티스레딩

### 문제점1. 두개의 스레드에서 위 코드를 실행시킨다면?

* 동시에 `if(uniqueInstance == null)` 에 접근한다면, 두 스레드에서 모두 ` new Singleton()` 객체를 생성할거다. 
* 싱글턴에서는 인스턴스가 하나여야하는데, 두개가 만들어질 수 있다..!



해결방법

*  `getInstance()` 를 동기화시키자.

```java
public static synchronized Singleton getInstance() {
    if(uniqueInstance == null) {
		uniqueInstance = new Singleton();
	}
    return uniqueInstance;
}
```

`synchronized` 키워드를 추가하면 

* 이 메소드에는 한번에 한 스레드만 접근할 수 있다. 
* 한 스레드가 사용 끝낼때까지 다음 스레드는 기다려야한다.
* 즉, 이 메소드를 동시에 실행시키는 일은 없다.



### 문제점2. 매번 동기화? 비효율적이다.

* 사실 동기화가 필요한 시점은 이 메소드가 시작될 때 뿐인데, 매번 이 메소드를 동기화하는건 좀 비효율적이다.



해결방법

1. `getInstance()` 속도가 중요하지 않다면 그냥 두자.

   메소드를 동기화하면 성능이 저하될 수 밖에 없다. 만약 동기화를 해서 `getInstance()`가 병목으로 작용한다면 다른 방법을 생각해봐야한다.

2. 인스턴스를 처음부터 만들어버리자.

   ```java
   // static initializer 에서 아예 인스턴스를 생성해버리자.
   private static Singleton uniqueInstance = new Singleton();
   
   // 이후에는 이미 존재하는 인스턴스를 반환하기만 하니까 -> 새로 생성하는 일이 없고 -> 문제가 없다.
   public static synchronized Singleton getInstance() {
       return uniqueInstance;
   }
   ```

   * 이렇게하면 클래스가 로딩될 때 JVM에서 Singleton의 유일한 인스턴스를 생성해준다. 
   * 하지만 lazy instantiation 장점을 누릴 수 없어, 비효율적일 수 있다.

3. DCL(Double-Checking Locking)을 사용해서 필요한부분만 최소로 동기화하자.

   ```java
   public class Singleton {
       private volatile static Singleton uniqueInstance;
       
       private Singleton() {}
       
       public static Singleton getInstance() {
         	// 인스턴스가 있는지 확인하고 없으면 동기화된 블럭으로 들어간다.  
           if(uniqueInstance == null) {
               synchronized (Singleton.class) {
                   if(uniqueInstance == null) {
                       uniqueInstance = new Singleton();
                   }
               }
           }
           return uniqueInstance;
       }
   }
   ```

   - DCL을 사용하면 인스턴스가 생성되어있는지 확인한 다음 -> **생성되어있지 않았을 때만** 동기화를 할 수 있다. (딱 원하던거!)
   - 하지만 코드가 복잡하다. (Head First Design Pattern, p220)



