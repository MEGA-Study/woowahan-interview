## 상속이란

객체간의 관계를 형성하는 방법 중 하나이다. 한 클래스가 다른 클래스를 상속하게 되면 객체간의 상속 관계가 형성되고 상속하는 클래스를 부모 클래스, 상속받은 클래스를 자식 클래스라고 한다. 자식은 부모의 속성과 동작을 상속받을 수 있다.

## 왜 상속을 해아할까?

- 중복을 제거하기 위함이다. 여러 클래스가 공통의 속성을 가지거나 같은 행위를 하고 있다면 코드 중복이 상당히 발생할 것이다. 공통 분모가 있다면 해당 클래스들은 서로 깊은 연관이 있다는 뜻이므로 하나의 클래스로 묶을 필요가 있다. 그래서 부모 클래스를 만들어 공통 분모를 모두 빼고 그 외 추가적인 내용을 자식 클래스에서 구현한다.

## 인터페이스 vs 추상 클래스

- 사용하는 목적에 차이가 있다.

    > 인터페이스는 구현을 강제하는 목적이 있으며 추상 클래스는 구현을 통해 확장을 할 수 있게 하는 목적이 있다.

- 새로운 타입을 정의할 수 있는 유연함에 차이가 있다.
    - 자바는 기본적으로 다중 상속이 안되니 추상 클래스를 통한 상속이 이뤄지고 있는 상황에서 새로운 타입을 정의하는데 커다란 제약이 있다.

            public abstract class A {}
            
            public class B extends A {}
            
            // 만약 B 클래스가 C 추상 클래스를 상속해야하는 상황이 생긴다면?
            public abstract class C {}
            
            // A - B의 계층 구조에 큰 혼란이 생길 수 있다.

    - 인터페이스를 제대로된 방법으로 잘 구현하고 있는 클래스에는 또 다른 어떤 클래스를 상속해야하는 상황이 와도 타입의 변화가 없다.

            public interface A {}
            
            public class B implements A {}
            
            // 만약 B 클래스에 C 추상 클래스를 상속해야하는 상황이 생긴다면?
            public abstract class C {}
            
            // 그래도 여전히 B 클래스의 타입을 A로 사용할 수 있다.

## 컴포지션

### 상속의 문제점

추상화 방법의 제일 최선의 방법 중 하나인 상속에도 문제가 있다.

- 상속은 기본적으로 캡슐화를 저해시킨다.
    - 부모 클래스의 정보를 자식 클래스가 알게 되면서 클래스가 가져야하는 캡슐화의 경계를 무너뜨리게 된다.

- 부모 클래스의 문제가 있는 상황에서 상속을 진행할 경우 그 문제는 고스란히 자식 클래스에게 전달된다.
    - 오류가 여러곳으로 전파될 수 있다.

- 만약 상속하려는 클래스에 대한 명확한 정보가 없다면 엉뚱한 결과값이 도래될 수 있다.
    - 부모 클래스의 구현 로직을 알지 못하는 경우 (전혀 다른 패키지에 있거나 외부 라이브러리를 통해 가져온 경우) 의도와는 다르게 예상치 못한 로직으로 이어질 수 있다.

        public class InstrumentedHashSet<E> extends HashSet<E> {
        	...
        
        	@Override
        	public boolean add(E e) {
        		addCount++;
        		return super.add(e);
        	}
        
        	@Override
        	public boolean addAll(Collection<? extends E> e) {
        		addCount += e.size();
        		return super.addAll(e);
        	}
        }

    - 위 코드의 문제점은 `addAll()` 에서 `super.addAll(e)` 을 호출했을 때 부모 클래스인 `HashSet` 의 `addAll()` 메서드는 `this.add()` 를 호출한다는 사실을 알지 못한채 구현되버렸다는 것이다.

        *이 사항은 어디에도 나와있지 않으니 모를 수 밖에 없다. 직접 코드를 까보지 않는 이상...*

    - `HashSet` 의 `this.add()` 을 호출하는 경우 다시 이를  오버라이딩하는 `InstrumentedHashSet` 의 `add()` 메서드를 호출하게 된다.
    - 이처럼 부모 클래스의 메서드를 호출했는데 하필 그 부모 클래스가 자식 클래스가 오버라이딩하고 있는 메서드를 내부에서 호출할 경우 예상치 못한 로직이 될 수 있다.

### 이와 같은 상속의 문제점을 피해갈 수 있는 대안인 컴포지션

컴포지션이란 : 기존 클래스를 확장하는 대신 새로운 클래스를 만들고 private 필드로 기존 클래스의 인스턴스를 참조하는 구조를 말한다. 기존 클래스가 새로운 클래스의 구성요소로 쓰인다는 뜻에서 composition이라 한다.

    public class ForwardingSet<E> implements Set<E> {
    	private final Set<E> s;
    	public ForwardingSet(Set<E> s) { this.s = s; }
    
    	public boolean add (E e) {
    		return s.add(e);
    	}
    
    	public boolean addAll (Collection<? extends E> e) {
    		return s.addAll(e);
    	}
    
    	...
    }

- 원래 상속하려던 `HashSet` 의 인터페이스인 `Set` 을 구현하는 클래스를 만들어 단순히 전달(forwarding)역할을 부여한다.

    public class InstrumentedSet<E> extends ForwardingSet<E> {
    
    	public InstrumentedSet(Set<E> s) {
    		super(s);
    	}
    
    	@Override
    	public boolean add(E e) {
    		addCount++;
    		return super.add(e);
    	}
    
    	@Override
    	public boolean addAll(Collection<? extends E> e) {
    		addCount += e.size();
    		return super.addAll(e);
    	}
    }

- 전달 클래스를 상속하면서 위에서 제기된 상속의 문제점을 해결할 수 있게 된다.
- `InstrumentedSet`  클래스의 `addAll()`메서드를 호출하면 부모 클래스인 `ForwardingSet` 클래스의 `addAll()` 메서드가 호출되고 내부 로직인 `s.addAll()` 가 실행된다. 만약 `private Set<E> s` 의 구체 클래스가 `HashSet` 이라면 위 상황처럼 `this.add()` 가 실행되는데 이번에는 이를 오버라이딩하고 있지 않아 실제 `HashSet` 클래스의 `add()` 메서드가 실행된다.

- 컴포지션의 해당하는 `ForwardingSet`과 같은 클래스를 만드는 일은 매우 귀찮은 일이지만 한번 만들고 나면 재사용성이 매우 뛰어나다.
    - 예를 들어 기존의 상속 방식을 고수했을 경우 `HashSet` 을 상속해 사용했지만 `TreeSet` 으로 사용해야할 경우 다시 상속하는 클래스를 만들어야하는 상황으로 이어진다.
    - 하지만 위 컴포지션을 통해 생성자 주입으로 상황에 맞는 `Set` 구현 클래스를 사용할 수 있다.

        `Set<Integer> example1 = new IntrumentedSet<>(new TreeMap<>());
        Set<E> example2 = new InstrumentedSet<>(new HashSet<>())`;
