# Runtime Data Area

## JVM Stack vs. Heap

> Stack은 쓰레드마다 생긴다. Heap은 메모리를 공유한다. Heap에는 인스턴스가 저장된다. Stack에는 메서드가 수행 될때마다 각 메서드의 인스턴스 레퍼런스, 지역변수가 각각의 스택 프레임이 추가되면서 들어간다.

1. JVM Stack은 각각 쓰레드가 시작될 때 생성된다. Stack Frame을 저장하는 스택이다. 메서드가 수행 될 때마다 하나의 스택 프레임이 생성되어 해당 쓰레드의 JVM stack에 추가 되고 메서드가 종료되면 스택 프레임이 제거 된다. Stack Frame은 Local Variable Array, Operand Stack, Constant Pool의 레퍼런스를 갖는다.
2. JVM은 Heap에서 자바 프로그램을 실행할 때 자바 클래스 인스턴스와 Array에 대한 메모리를 관리한다.
   - Heap과 Method Area는 각각의 쓰레드가 메모리를 공유한다. Method Area는 변하지 않는 Constant 값들이 존재하기 때문에 각 쓰레드들이 메모리를 공유하더라도 문제가 없다.

![Stack Heap](/assets/img/jvm/stack-heap.png)

## JVM의 동작 과정

Java Virtual Machine의 구조는 아래 그림처럼 되어있다.

![JVM](/assets/img/jvm/jvm.png)

JVM은 Java 컴파일러가 컴파일 한 Byte Code를 Class Loader를 이용해 Method Area에 실행 가능한 상태로 적재한다.

![Method Area](/assets/img/jvm/method-area.png)

Execution Engine은 Method Area에 Load 되어 있는 Byte Code 정보를 이용해서 Java 프로그램을 실행한다. (Execution Engine은 byte code를 line by line 실행한다)

## JVM 구조에서 Runtime Data Area에 대해 알아보자

Runtime Data Area는 JVM이 프로그램을 수행하기 위해 OS로부터 별도로 할당 받은 메모리 영역이다. 위의 그림에서 처럼
Runtime Data Area는 PC Registers, JVM Stacks, Method Area, Heap, Native Method Stacks으로 구성된다. 이 중 PC Register, JVM Stack, Native Method Stack은 각 쓰레드 별로 존재한다. 각 쓰레드마다 서로 다른 메모리가 할당 된다.

### PC Register

현재 수행 중인 JVM 명령의 주소를 갖는다.

### Method Area

모든 Thread 들이 공유하는 메모리 영역이다. Class(Type), Method, Field 정보를 가진다.

- Type Information: 클래스와 관련한 모든 정보
- Constant Pool: 문자열 상수와 같은 리터럴 상수
- Field Information
- Method Information
- Class Variable: static으로 선언되는 모든 클래스 변수
- Reference to Class Class Loader: 특정 클래스를 로드한 클래스로더의 정보를 관리
- Reference to Class class method table: 클래스 메서드에 대한 direct reference

Method Area의 Byte Code를 이용해 프로그램을 실행할 때 JVM Stack, Native Method Stack 공간을 활용한다.

### JVM Stack

![JVM Stack](/assets/img/jvm/jvm-stack.png)

각각 쓰레드가 시작될 때 생성된다. Stack Frame을 저장하는 스택이다. 메서드가 수행 될 때마다 하나의 스택 프레임이 생성되어 해당 쓰레드의 JVM stack에 추가 되고 메서드가 종료되면 스택 프레임이 제거 된다. Stack Frame은 Local Variable Array, Operand Stack, Constant Pool의 레퍼런스를 갖는다.

```java
public class Adder {
    int add(int a, int b) {
        return a + b;
    }

    public static void main(String[] args) {
        Adder adder = new Adder();

        int result = adder.add(1, 2);

        System.out.println(result);
    }
}
```

JVM Stack을 살펴보기 위해 위와 같은 코드를 작성한다면, JVM 스택의 상태는 아래 그림처럼 바뀌게 된다.

맨 처음 main() 호출 시 아래와 같은 그림이 된다.

![JVM Stack 1](/assets/img/jvm/jvm-stack1.png)

![JVM Stack 1 debug](/assets/img/jvm/1.png)

그 다음 Adder의 생성자 호출 시 다음과 같이 바뀐다.

![JVM Stack 2](/assets/img/jvm/jvm-stack2.png)

![JVM Stack 2 debug](/assets/img/jvm/2.png)

그 이후 Adder 생성자가 끝나면서 다시 Adder 생성자의 Stack Frame은 사라지고 아래 그림과 같이 변하게 된다.

![JVM Stack 1](/assets/img/jvm/jvm-stack1.png)

그리고 다음 adder.add(1, 2) 메서드 호출 시 다음과 같이 stack frame이 바뀐다.

![JVM Stack 3](/assets/img/jvm/jvm-stack3.png)

![JVM Stack 3 debug](/assets/img/jvm/3.png)

### Local Variable Array

0부터 시작하는 인덱스를 가진 배열이다. 0: this reference, 1부터는 메서드에 전달된 파라미터들, 그 이후에는 메서드의 지역 변수들이 저장된다.

### Operand Stack

메서드의 실제 작업 공간. Operand Stack과 Local Variable Array 사이에서 데이터를 교환하고 다른 메서드 호출 결과를 추가하거나(push) 꺼낸다(pop).

예를 들어보면, 만약 지역 변수 두 개(1, 2)를 더하는 바이트 코드를 살펴보면, 아래와 같이 볼 수 있을 것이다.

```byte code
iload_0     # Push the value from local variable 0 onto the stack
iload_1     # Push the value from local variable 1 onto the stack
iadd        # Pops those off the stack, adds them, and pushes the result
```

이 상황에서 operand stack의 상태는 아래 처럼 바뀌게 되는 것이다.

![Operand stack](/assets/img/jvm/operand-stack.png)

### Native Method Stack

자바 외의 언어로 작성된 네이티브 코드를 위한 스택. Java Native Interface를 통해 호출하는 C/C++ 코드를 수행하기 위한 스택이다.

### JVM Heap

JVM은 Heap에서 자바 프로그램을 실행할 때 자바 클래스 인스턴스와 Array에 대한 메모리를 관리한다.

![Stack Heap](/assets/img/jvm/stack-heap.png)

Heap과 Method Area는 각각의 쓰레드가 메모리를 공유한다. Method Area는 변하지 않는 Constant 값들이 존재하기 때문에 각 쓰레드들이 메모리를 공유하더라도 문제가 없다.
