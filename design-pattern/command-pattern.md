#  Command pattern

TIL

```
커맨드 패턴을 사용하면
- 작업을 요청하는 쪽과
- 작업을 처리하는 쪽을 
분리시킬 수 있다.

어떻게? 요구사항을 객체로 캡슐화해서.
- Command 인터페이스를 정의하고
- ConcreteCommand 에서는 execute() 메소드를 구현한다.
: 특정 인터페이스만 구현되어있으면 그 커맨드 객체에서 실제로 어떤 일을 하는지 신경 쓸 필요가 없다.

커맨드 패턴 구현
- Command
- Receiver, Invoker, Client

보너스
- DefaultCommandClass가 필요할 때 => NoCommand(Null Object)
- 여러 커맨드를 하나로 묶고싶을 때 => MacroCommand
- Receiver를 생략하고 싶으면 => SmartCommand 객체
```



## 커맨드 패턴

커맨드 패턴을 사용하면 작업을 요청하는 쪽과 처리하는 쪽을 분리시킬 수 있다.

* 예: 리모컨
  * 커맨드 객체를 구현한다. 커맨드 객체는 특정 작업 요청을 캡슐화한다.
  * 버튼마다 커맨드 객체를 저장해두면
  * 버튼을 눌렀을 때 커맨드 객체를 통해 작업을 처리한다.



## 커맨드 패턴 구현

- Command
- Receiver
- Invoker
- Client
  

### 1. Command 

커맨드 객체는 모두 같은 인터페이스를 구현한다.

```java
public interface Command {
	public void execute();
}
```

예제로 전등을 켜기 위한 Command 클래스를 구현해보자.

```java
public class LightOnCommand implements Command {
    Light light;	// Receiver
    
    public LightOnCommand(Light light) {
        this.light = light;
    }
    
    public void execute() {
        light.on();
    }
}
```

### 2. Invoker

```java
public class SimpleRemoteControl { 
	Command slot;
    
    public SimpleRemoteControl() {}
    
    public void setSlot(Command command) {
        slot = command;
    }
    
    public void buttonPressed() {
        slot.execute();
        // 버튼이 눌리면 지금 슬롯에 연결된 Command 객체의 execute()를 호출한다.
    }
}
```

### 3. Client

```java
public class RemoteControlTest {
    public static void main(String[] args) {
	// 전등을 켜는 Command 객체 생성
        Light light = new Light();
        LightOnCommand lightOnCommand = new LightOnCommand(light);
        
        // 리모컨에 전등을 켜는 Command 세팅
        SimpleRemoteControl remote = new SimpleRemoteControl();
        remote.setSlot(lightOnCommand);
        
        // 리모컨 버튼을 누름
        remote.buttonPressed();
    }
}
```



Client

* ConcreateCommand 를 생성하고 Receiver를 설정한다.

Invoker

* 명령어들이 들어있다.
* 명령어의 `execute()` 메소드를 호출 → 커맨드 객체에 특정 작업을 수행해달라고 요구한다.
* 예: SimpleRemoteControl

Receiver

* 요구사항을 수행하기 위해 구체적으로 어떤 일을 처리해야하는지 알고있는 객체다.
* 예: Light

Command

- 커맨드 객체에서 제공하는 메소드는 `execute()` 하나뿐이다.
- `execute()` 메소드에 리시버가 할 행동을 구현한다. (=리시버의 메소드를 호출한다.)
- 외부에서는 구체적인 행동은 관심없고 `execute()` 만 호출하므로, 행동은 캡슐화되었다.
- 예: LightOnCommand
  

정리하면

1. 사용자가 리모컨의 버튼을 누르면
2. 그 버튼에 연결된 커맨드 객체의 `execute()` 메소드가 호출되고
3. 커맨드 객체의 `execute()` 메소드 내용대로 Receiver가 특정 행동을 실행한다.



## 리모컨 슬롯 4개중에 2개만 쓰고싶다.

리모컨 슬롯이 4개 있을 때 

1. 슬롯에 아무 Command 객체를 세팅하지 않은 상태에서 슬롯을 누르면 `Null.execute()` 를 호출하게 된다.
2. 일일이 null 체크를 해야한다.
3. 번거롭다.

이럴 때 아무것도 하지 않는 기본 커맨드 클래스(Null Object)를 구현해서 리모컨 슬롯을 초기화할 때 할당해주자.

```java
public class NoCommand implements Command {
    public void execute() {}
}
```



### Null Object

* = defaultValueObject
* 딱히 return 할 객체는 없지만 클라이언트 쪽에서 null을 처리하지 않아도 되도록 하고싶을 때 Null Object 를 활용하면 유용하다.



## 여러 커맨드를 실행하는 하나의 커맨드를 만들고 싶다.

리모콘 버튼 한개에 여러 커맨드를 연결하고 싶다.

```java
public class MacroCommand implements Command {
    Command[] commands;
    
    public MacroCommand(Command[] commands) {
        this.commands = commands;
    }
    
    public void execute() {
        for(int i=0; i<commands.length; i++) {
            commands[i].execute();
        }
    }
}

/* 사용예
Command[] commands = {lightOn, tvOn, stereoOn};
MacroCommand partyOnCommand = new MacroCommand(commands);
*/
```

그냥 바로 PartyOnCommand 클래스를 만들고 PartyOnCommand 클래스의 `execute()` 안에서 여러 커맨드의 `execute()` 를 호출하도록 해도 되지 않나?

* ㅇㅇ결과는 같다. 
* 하지만 MacroCommand를 이용하면 PartyOnCommand에 **집어넣을 커맨드를 동적으로 결정할 수 있기 때문에 유연성이 훨씬 좋다.**
* MacroCommand를 만들어서 활용하는게 더 우아한 방법이다.



## 스마트 커맨드 객체

요청 자체를 리시버한테 넘기지 않고 커맨드 객체 자기가 알아서 처리하는 경우도 있음.

이런 객체를 "스마트 커맨드 객체"라 한다.



단, 이렇게하면

* 리모컨 예제 수준으로 Invoker, Receiver 기능을 분리시키는 것이 불가능해지고

* Command 에서 매개변수를 받아서 Receiver 를 분기하는 식의 작업을 할 수 없다.



## 커맨드 패턴 활용

- 매개변수를 써서 여러가지 다른 요구사항을 집어넣을 수 있다.
  - invoker에서 매개변수를 받아서 → 적합한 command.execute()를 실행하도록.

- 요청 내역을 큐에 저장할 수 있다. (Head First Design Pattern, p266)
- 요청을 로그에 기록할 수 있다. (Head First Design Pattern, p267)
- 작업취소 기능도 지원 가능하다.

## 생각

- 실행하고자 하는 Receiver 들의 인터페이스가 다를 때 커맨드패턴을 사용해서 `execute()`로 인터페이스를 통일시키니까 유용하다.
   - 서로 다른 DTO를 가진 데이터모델을 로그로 남기는 경우도 유용할 것 같다.
   - Command 인터페이스의 method 를 printLog()로 두고
   - ItemLogCommand, UserLogCommand, BrandLogCommand 이런 concreateCommand 객체를 만들어서 printLog()를 구현하고
   - 클라이언트에서는 printLog()를 호출
- 말그대로 command니까 뭔가 시키고 끝. (=`execute()` 메서드의 return 타입이 void)
- 명령별로 집중해서 command 객체를 구현할 수 있고, 이를 조립하면 되니까 편한것 같다. 
   - 단, command의 순서가 보장되어야 하는 경우는 커맨드패턴이 어울리지 않음. 
   - 예: 주문처리, 결제처리, 쿠폰생성같은 flow
 
