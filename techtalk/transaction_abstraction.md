# 5장. 서비스 추상화



## 서비스 레이어의 등장

하나의 Entity에 대해 간단한 CRUD 외 기능이 요구될 경우 해당 로직(비지니스 로직)을 둘만한 곳이 필요

*비지니스 로직 서비스를 제공한다는 의미에서 클래스에 Service라는 이름을 붙인다*

> 단순히 DAO (Repository)의 있는 CRUD 메서드만 사용해도된다면 Service Layer는 없어도 그만



### 사용자 레벨 관리 기능 추가 (Service Layer)

```java
@Service
public class UserService {
  ...
    
  /* 사용자 레벨 업그레이드 로직 */
  public void upgradeLevels() {
    List<User> users = userDao.getAll();
    for(User user : users) {
      /* DAO 로직외에 비지니스 로직이 수행 */
      if (canUpgradeLevel(user)) {
        upgradeLevel(user);
      }
    }
  }
  
  private boolean canUpgradeLevel(User user) {
    Level currentLevel = user.getLevel();
    switch(currentLevel) {
      case BASIC : return (user.getLogin() >= 50);
      case SILVER : return (user.getRecommend >= 30);
      case GOLD : return false;
      default : throw new IllegalArgumentException();
    }
  }
  
  private void updatedLevel(User user) {
    user.upgradeLevel();
    userDao.update(user);
  }
  
  ...
}
```





## 트랜잭션 서비스 추상화

*Q. Service Layer 로직이 수행되는 도중에 예기치못한 장애로 작업이 중단되면 어떻게 하나?*

DB에 변경이 일어나는 작업일 경우, 중단되기 전까지의 작업에 대한 DB 변경을 취소해야한다. 만약 그렇지 않는다면 작업이 중단된 시점을 찾아서 그 이후부터 다시 작업을 실행시켜야한다. **이는 매우 귀찮으며 번거러운 일**

그래서 작업이 중단되기 전까지 반영된 DB 변경을 모두 복구시켜야한다. 그리고 다시 전체 작업을 수행하는 것이 더 효율적이다.

결국 한 작업 단위로 일어나는 모든 일들은 전부 성공하거나 전부 실패해야한다. 하나의 작업을 다시 쪼개어 어떤 것은 수행되고 어떤 것은 건너뛰었다가 나중에 작업하는 등의 방식으로 가면 안된다.



### 트랜잭션

그래서 이러한 작업이 **트랜잭션**의 원자성을 가지고 있다고 볼 수 있다. 더이상 쪼개질 수 없는 상태의 단위인 이 작업을 트랜잭션이라고한다. 

```java
/* 이 upgradeLevels() 메서드는 하나의 트랜잭션 단위다. */
public void upgradeLevels() {
  List<User> users = userDao.getAll();			// 1. dao 단의 getAll() 메서드
  for(User user : users) {
    if (canUpgradeLevel(user)) {			// 2. user 도메인을 이용한 로직이 있는 메서드
      upgradeLevel(user);				// 3. dao 단의 update() 메서드와 user 도메인 로직이
    }							// 	있는 메서드를 품고 있는 메서드
  }
}
```

위 `upgradeLevels()` 메서드는 **사용자의 레벨을 업그레이드하는 작업**을 수행하기 위해 1, 2, 3번 메서드를 모두 사용하고 있으며 3개 메서드가 모두 동작해야만 작업이 완료되는 하나의 트랜잭션이다. 그리고 제일 중요한 것은 실제적으로 DB에 변경을 일으키는 작업을 수행하는 3번 메서드가 모두 성공해야만 `트랜잭션 커밋`이 수행된다.



### 트랜잭션 경계 설정

그렇지만 위 코드는 하나의 트랜잭션으로 처리되지 않는다. `JdbcTemplate`을 사용하는 userDao의 메서드들은 메서드마다 Connection 객체가 새롭게 생성되므로 각각 독립된 DB에 대한 Access가 일어나기 때문이다.

```java
private void upgradeLevel(User user) {
  user.upgradeLevel();
  userDao.update(user);				// upgradeLevels() 메서드에서 이 메서드가 n번 호출될 때
}						// 각각 Connection 객체가 n번이 새롭게 생성된다.
```

이를 하나의 트랜잭션으로 묶기 위해서는 n번 수행되는 `userDao.update(user)`가 하나의 `Connention` 객체를 공유하도록 하면 된다. (트랜잭션 동기화)



### 트랜잭션 동기화

**Service Layer**에서 트랜잭션을 시작하기 위해 만든 Connection 객체를 특별한 저장소에 저장하고 해당 작업이 진행되는 동안 호출되는 DAO에 대해 기존에 저장된 Connection을 가져다 사용하게 만드는 것을 이야기한다.

```Java
public void upgradeLevels() throws Exception {
  TransactionSynchronizationManager.initSynchronization();	 // 트랜잭션 동기화 관리자를 이용해 초기화
  Connection c = DataSourceUtil.getConnection(dataSource);   // DB 커넥션 생성 
  c.setAutoCommit(false);				// auto commit 헤제
  
  try {
    // 이전 로직과 동일
    c.commit();								  // 트랜잭션 commit
  } catch (Exception e) {
    c.rollback();								// exception 발생 시 rollback;
    throw e;
  } finally {
    DataSourceUtil.releaseConnection(c, dataSource);			// jdbcTemplate이 아닌 이곳에서 커넥션 해제
    TransactionSynchronizationManager.unbindResource(this.dataSource);
    TransactionSynchronizationManager.claerSynchronization();	// 동기화 작업 종료 및 정리
  }
}
```

*`TransactionSynchronizationManager` : 스프링이 제공하는 트랜잭션 동기화 관리 클래스*

*Q. 그렇다면 `JdbcTeamplate`에도 `Connection` 객체를 만들고 해제하는 과정이 존재한데 DAO 메서드를 사용하는 Service 메서드에서도 Connection을 생성한다면 중복 생성되는 것이 아닐까?*

`JdbcTemplate`은 `TransactionSynchronizationManager`에 DB 커넥션이나 트랜잭션이 없는 경우 새로운 `Connection` 직접 만들고 작업을 진행한다. 그렇지 않은 경우에는 트랜잭션 동기화 저장소에 있는 `Connection` 객체를 가져다 사용한다.



### Spring Transaction

jdbc 커넥션과 같은 **Local Transaction**은 사용하기 쉽지만 여러 트랜잭션 자원들과 사용할 수 없다. 반대로 JTA를 통해 관리되는 **Global Transaction**은 여러 트랜잭션 자원들과 사용될 수 있지만 사용하기 까다로우며 오로지 application server 환경에서만 사용이 가능하다. 

그래서 Spring은 **Transaction 추상화**를 제공하여 두 종류의 Transaction 문제를 해결해준다.

```java
public interface PlatformTransactionManager {
  TransactionStatus getTransaction(TransactionDefinition definition) throws TransactionException;
  
  void commit(TransactionStatus status) throws TransactionException;

  void rollback(TransactionStatus status) throws TransactionException;
}
```

## 

```java
@EnableTransactionManagement
public class ApplicationConfig {
    ...
    @Bean
    public PlatformTransactionManager transactionManager(DataSource dataSource) throws URISyntaxException, GenericSecurityException, ParseException, IOException {
    
    /* PlatFormTransactionManager에 DataSourceTransactionManager로 넣어줄 때 */
    return new DataSourceTransactionManager(dataSource);
    
  }
}
```



```java
public class UserService {
  private PlatformTransactionManager txManager;
  
  public UserService(PlatformTransactionManager txManager) {
    this.txManager = txManager;
  }
  
  DefaultTransactionDefinition def = new DefaultTransactionDefinition();

  TransactionStatus status = txManager.getTransaction(def);
  
  try {
    // 이전 로직과 동일
  } catch (Exception ex) {
      txManager.rollback(status);
      throw ex;
  }
  txManager.commit(status);
}
```



### @Transactional

스프링에서 트랜잭션 처리를 위한 여러방식 중에 어노테이션 선언 방식을 지원한다.

 `@Transactional` 어노테이션을 클래스, 메서드위에 추가하면 자동으로 적용된다.

스프링은 `@Transactional` 어노테이션이 붙은 클래스를 위해 프록시 객체(**Java Dynamic Proxy**)를 생성한다. 프록시 객체는 프레임워크가 런타임 전이나 후에 해당 클래스나 메서드에 `PlatformTransactionManager`를 사용하는 트랜잭션 처리 로직을 주입하게 한다. 해당 트랜잭션 성공 여부에 따라 commit / rollback이 실행된다.

`PlatformTransactionManager`는 `@EnableTransactionManagement` 어노테이션을 이용해 빈(Bean) 등록을 해줘야한다. 상황에 맞게 transaction manager 인터페이스의 구현체로 생성해주면 된다. 만약 spring boot를 사용한다면 spring-data-* 또는 spring-tx 의존성이 주입되는 경우 transaction manager가  해당 의존성의 기본값으로 자동 설정된다.

> ```
> implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
> ```
>
> 위 경우 `data-jpa`를 의존 주입 받고 있어 jpa에 맞는 transaction manager 구현체가 `PlatformTransactionManager`를 생성하여 빈으로 등록된다.



## 결론

DB에 접근하는 로직 외 추가적인 로직(비지니스 로직)이 처리되는 장소가 필요하다. 위에서 이야기한 바와 같이 User 에 대해 간단한 CRUD 기능 외 사용자 레벨을 관리하는 로직이 추가된다면 이는 비지니스 로직이기때문에 Service 계층으로 위치시켜야한다.

한 application에서 여러 DAO 로직의 조합이 요구되는 기능이 존재해야한다면 (DB의 상태를 변경한다면) 이는 트랜잭션 처리가 필요할 수 있다. 트랜잭션에 대한 처리 방식과 경계 설정이 Service 계층에서 처리해줘야한다.

### 그래서 Service 계층이 필요하다.
