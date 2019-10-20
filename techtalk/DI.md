## Dependency Injection

의존 객체 주입으로 변화에 유연하게 대응할 수 있는 대표적인 패턴이다.

한 객체가 다른 객체를 의존하고 있을 때 결합도를 낮출 수 있는 가장 좋은 방법이다.

---

## 왜 DI를 해야할까?

### 사용하는 자원을 클래스 내부에서 직접 선언하면 안되나?

- 직접 선언한다고해서 문제가 될 건 전혀 없다.

        public class SpellChecker {
        	private Lexicon dictionary = new EnglishKoean();
        
        	...
        }

    - `SpellChecker` 에서 사용하는 사전 `dictionary`은 한영 사전 `new EnglishKorean()`으로 미리 정해진다. 그래서  `SpellChecker` 는 한영 사전을 기본으로 메서드들이 수행될 것이다.

### 그런데 중국어, 일본어에 대한 철자 확인도 요구된다면 어떻게 할까?

- 위 `SpellChecker` 는 한영 사전만 사용하기 때문에 중국어, 일본어 지원이 불가능하다.
- 그렇다면 중국어, 일본어 구현체로 `dictionary2`, `dictionary3` 도 필드로 추가해보자.
    - 그리고 철자 확인 로직에서 확인해야하는 철자를 확인하고 이를 처리할 사전을 호출하기 위해 분기를 하는 방식으로 구현하면 어떨까?

    *그렇다면 if문의 향연으로 새로운 언어가 추가될 때마다 if문은 계속 추가되어야한다.* 

### 철자 확인 행위는 사용하는 사전의 종류에 따라 큰 영향을 받게 된다.

### `SpellChecker`는 사전의 종류에 관심이 없고 가지고 있는 사전을 토대로 철자를 확인하는 일만 수행하면 된다. 그렇기 때문에 클래스 내부에서 필드를 정의하기 보단 외부에서 넘겨주는 객체를 주입받아 사용하는 방식을 사용해보자.

    public class SpellChecker {
    	private Lexicon dictionary;
    
    	public SpellChecker(Lexicon dictionary) {
    		this.dictionary = dictionary;
    	}
    
    	...
    }

- `SpellChecker` 를 사용하는 클라이언트가 자신이 현재 어떤 언어에 대한 철자를 확인해야하는지 알고 있을 것이기 때문에 그에 맞는 사전으로 생성자로 주입시켜주면 `SpellChecker` 는 동적으로 다양한 언어의 철자를 확인해주는 유연한 클래스가 된다.
