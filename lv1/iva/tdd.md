## TDD란

> Test-Driven Development

개발자가 요구사항에 대한 구현 로직보다 검증을 위한 테스트 로직을 먼저 작성한다. 그리고 테스트 코드가 성공할 수 있게 프로덕션 코드를 작성하고 리팩토링을 수행한다. 이와 같이 3단계의 짧은 사이클을 반복하며 소프트웨어를 만드는 개발 방법을 이야기한다.

## TDD가 왜 좋을까?

1. 테스트 코드의 누락을 방지할 수 있다.

   - 테스트 코드 커버리지가 높아 코드 변경에 따른 검증을 안정적으로 수행할 수 있다.

2. 작은 단위부터 개발이 가능하다.

   - 만약 새롭게 코드를 추가해야하는 상황이라면(혹은 레거시 코드가 없는 상황이라면) 어디서 부터 시작해야할지, 무엇부터 시작해야할지 가늠하기 어려울 수 있다.
   - 테스트 코드를 먼저 작성함으로써 요구사항에 맞는 구현 로직을 작성하는 범위를 잘게 쪼개볼 수 있다.
   - 처음부터 광범위하게 생각하게 된다면 추측에 의한(아직 구현되지 않는 단위 코드, API에 대한 추측) 오류가 발생할 수 있다.
   - 테스트 코드는 당장 프로덕션 코드의 구현만으로 테스트가 잘 수행되는지 확인하는 것이기 때문에 다른 기능에 대한 추측이 필요없는 단위까지 범위를 좁힐 수 있다.

3. 설계 단계에서 예상치 못한 문제를 발견할 수 있다.

   - 위에서 언급했듯이 TDD를 통해 개발 범위를 좁히고 시작할 수 있다. 이를 통해 다른 객체간의 참조 시 결합도를 미리 확인할 수 있다.

   - A가 B를 참조하는 설계했을 시 A 코드에 대한 테스트 코드를 작성하려면 먼저 B 코드에 대한 테스트 코드가 먼저 작성되어야한다. 이를 통해 객체간의 관계를 다시 확인할 수 있다.

   - 그리고 그렇게 확인된 객체 관계의 범위를 확인함으로써 한 모듈에서 불필요한 관계를 빠르게 발견할 수 있다.

     > 객체간 관계를 갖는 것은 매우 중요하다.
      허나 그 관계가 너무 긴밀하다면 유지보수하기 어려운 코드로 이어질 가능성이 높다.
      관계자체를 없앨 순 없으니 관계를 가지는 객체의 범위를 제한할 필요가 있다.
      해당 범위는 기능 모듈을 기준으로 할 필요가 있다.