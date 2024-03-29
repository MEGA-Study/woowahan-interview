# lv 1에서 배운 것들

Last Edited By: Hyo-Jin Park

---

# 1. 팀원들과 미래의 내가 함께 읽을 수 있는 코드를 작성하는 방법을 배웠다

## 직관적인 코드 작성하기

- 이름 짓는 것에 공 들이기
- Enum
- 메서드 분리하기

### 정적 팩토리 메서드

- 반환될 객체의 특성을 드러낼 수 있다.
- 같은 시그니처를 갖는 생성자가 여러 개 있을 때
- 호출될 때마다 인스턴스를 새로 생성하지 않아도 된다.

    ⇒ 같은 객체가 자주 요청되는 상황에서 성능을 높일 수 있는 좋은 방법이 되시겠다~

### 일급 컬렉션

- 예외가 발생할 확률이 높은 사용자 입력 데이터를 처리하는 부분이 분리되어 경우를 관리하기가 편리하다
- 값의 의미를 부여함으로써 의도를 명확하게 전달한다.
- `List<List<Boolean>> lines……;` → `List<Line> lines;`

## 구조적인 코드 작성하기

### 상속과 구현

- 인터페이스
- 추상 클래스

---

# 2. 내가 사용하고 있는 언어에 대한 이해도를 높였다

## 객체지향 언어

### 1-1. 객체지향

- 협력하는 자율적인 객체들의 공동체…
- 객체지향 설계의 첫 번째 목표는 훌륭한 객체를 설계하는 것이 아니라 훌륭한 협력을 설계하는 것!
    - 그러니까, **각자의 역할이 잘 분담된 객체**들이 **협력을 통해** 사용자가 원하는 결과를 산출해내는 것이라고 할 수 있나
- `getter/setter`를 지양하고 메시지를 지향하는 이유
    - 왜?
        - 다른 객체의 상태를 직접 이용한다는 것은 객체끼리의 협력이 적극적으로 이루어지지 못하고 있다는 것을 의미하지 않을까

### 1-2. 객체지향적이란 무엇일까?

- 적절한 객체에게 적절한 책임을 할당하는 것
- 이 세 가지의 특성을 지원하는 언어를 OOPL이라고 한다
    1. **캡슐화**:
        - ‘데이터’와 ‘데이터를 다루는 방법’을 **묶는** 것
        - 클래스 경계를 벗어나 알 필요가 없는 타입으로 진입하지 않는 것 => **디미터 법칙**
    2. **상속성**
        - 부모 클래스가 갖고 있는 특징을 자식 클래스가 물려 받는 것
    3. **다형성**
        - 상속성의 계층을 따라서 각 클래스에 동일한 이름의 메서드를 사용할 수 있는 것
- 참고: [https://www.slideshare.net/plusjune/ss-46109239?qid=0b8c67e2-c463-4e75-8c77-4ea675a74626](https://www.slideshare.net/plusjune/ss-46109239?qid=0b8c67e2-c463-4e75-8c77-4ea675a74626)

### 1-3. 객체지향 5원칙(SOLID)

1. SRP: 단일책임의 원칙
2. OCP: 개방폐쇄의 원칙
    - 확장에는 열려 있고 변화에는 닫혀 있는 것
    - 인터페이스를 사용하면 된다
3. LSP: 리스코프 치환의 원칙
    - 부모의 메서드의 성격과 다른
    - Vehicle.ride() 를 오버라이드 했는데 fly하면 안된다
4. ISP: 인터페이스 분리의 원칙
    - 인터페이스를 최대한 잘게 쪼개자
    - 사용하지 않는 인터페이스는 구현?하지 않는다
5. DIP: 의존성 역전의 원칙

## 추상(abstract)

- 특징에 대한 서술
- 사용자가 구체적인 내용을 생각하지 않고도 사용할 수 있는 기능
- 관련 있는 것을 묶어서 이름을 부여하는 일 => **추상화**!(abstraction)

### 추상화를 위해서 이용했던 게 어떤 게 있었지?

1. 공통되는 중복 기능을 추상 클래스를 사용하여 묶기
2. 인터페이스로 공통 메서드 추출하여 묶기
3. 메서드 추출도 추상화라고?
4. 클래스 만드는 것도 추상화야?

### 그래서 뭐가 좋았지?

1. 중복 코드 삭제 -> 유지보수의 용이성
2. 강제적으로 필요한 메서드를 기술해 놓으니까 이 인터페이스의 구현 클래스(구체 클래스)들의 특징을 파악하기에 좋음
3. `Collections.max()` 이런 거

### 디미터(Demeter) 원칙

- `티버.get티버().get티버();`
- 여러 개의 점이 들어 있는 코드는 책임 소재의 오류를 범했을 가능성이 높다.
- 너무 많은 객체들을 지나치게 알고 있는 꼴이다. (유통에 있어 중개상을 배제하고 직거래하는 것처럼)
- 이것은 **캡슐화**를 어기고 있다는 방증이기도 하다.
- 결합도가 높아진다.
- 출처: [https://developerfarm.wordpress.com/2012/01/30/object_calisthenics_5/](https://developerfarm.wordpress.com/2012/01/30/object_calisthenics_5/)

## Java 8

### Stream

### Optional<T>

---

# 3. 안정성 있는 프로그램을 구현하는 방법을 배웠다

## 단위 테스트(JUnit5)

### 왜 사용하지?

1. main 함수를 사용해서 테스트를 했을 때는 프로덕션 코드에 테스트 코드가 분리되지 못했음
    - 테스트를 위해서 클래스를 매번 새로 생성하거나
    - 기존의 코드를 주석처리하고 테스트를 진행해야 했음
2. 기존 프로덕션의 코드를 이해할 때 유용함
    - 동작 및 상황별로 기능을 쪼개서 작성해 놓았으니까

## TDD

### 좋은 점은 무엇인가?

- 객체지향적인 개발이 가능하다
- 왜냐?
    - ⌈객체지향의 사실과 오해⌋에서 이야기하길, **“협력을 설계할 때는 객체보다는 메시지를 먼저 선택하고 그 후에 메시지를 수신하기에 적절한 객체를 선택해야 한다.”**라고 말하고 있음.
    - TDD의 사이클과 객체지향 설계의 목표가 잘 맞는다고 생각했음

### 내가 느낀 것

- 필수는 아닌데 개발 단계에서도 유지보수에서도 여러가지 도움을 많이 받을 수 있었음
- 입력값과 출력값을 우선 명시하고 개발을 시작하기 때문에 클래스와 메서드의 역할을 보다 명확하게 구분하여 구현할 수 있었다.
- TDD를 하면 자연스럽게 테스트 코드를 작성하니 [선프로덕션-후테스트]의 개발 흐름보다 꼼꼼하게 작성됐다.
- 코드의 구조 변경 및 수정에 대한 두려움이 덜했다. (정신건강쓰 대박쓰)

## 그 외

### BigDecimal

- `double`은 부동소수점을 이진법으로 나타내기 때문에 정확한 값을 저장할 수 없다.
- `BigDecimal` 클래스를 사용하여 정확한 계산을 하자~

---

# 4. 효율적인 코드를 작성하는 방법을 배웠다

## 재사용 가능한 코드 작성하기

### 1-2. 의존성 주입 (Dependency Injection)

- 의존성: A 객체가 B 객체를 활용해 로직을 구현하는 경우 A 객체를 B 객체에 의존 관계를 가진다고 한다.
- 주입: 객체 간의 의존 관계를 의존 관계를 가지는 객체 내부에서 결정하지 않고 외부에서 결정하는 것

### 1-2. 의존성이 위험한 이유

- 의존당하는 객체(피의존 객체..?)의 변화에 의존하는 객체도 변경될 가능성이 높기 때문에
- 랜덤값을 반환하는 `RandomLineGenerator`에 의존하면 상황을 예측할 수 없어서 로직을 테스트하기 어렵다.
    - 재사용성이 떨어진다.
    - 변화에 대응하기가 쉽다.
    - 테스트용 생성자가 아닌 실제 로직과 똑같은 생성자를 통해 객체를 사용할 수 있다.

## 유지/보수하기 좋은 코드 작성하기

### 결합도와 응집도

- 결합도는 낮추고 응집도는 높이고~
