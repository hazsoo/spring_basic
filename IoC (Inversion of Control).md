# IoC (Inversion of Control)

## 제어의 역전

- 제어권의 이전을 통한 제어관계 역전

1. OwnerController가 직접 repository를 생성한다.

   ```java
   class OwnerController {
   	private OwnerRepository repository = new OwnerRepository();
   }
   ```

   

2. OwnerControllerTest가 생성해주는 OwnerController() 생성자를 사용하기만 한다.

   ```java
   class OwnerController {
   	private OwnerRepository repo;
   	public OwnerController(OwnerRepository repo) {
   		this.repo = repo;
   	}
   	// repo를 사용합니다.
   }
   class OwnerControllerTest {
   	@Test
   	public void create() {
   		OwnerRepository repo = new OwnerRepository();
   		OwnerController controller = new OwnerController(repo);
   	}
   }
   ```

1 -> 2 를 IoC, 즉 제어의 역전이라고 한다.



## IoC 컨테이너

###### ApplicationContext (BeanFactory)

- 오브젝트의 생성과 관계설정 같은 제어를 담당한다. 이때, IoC 컨테이너에 의해 관리되는 오브젝트들은 Bean 이라고 부른다. IoC 컨테이너는 Bean을 저장한다고 하여, BeanFactory라고도 불린다.
- BeanFactory는 하나의 인터페이스이며, ApplicationContext는 BeanFactory의 구현체를 상속받고 있는 인터페이스이다. 실제로 스프링에서 IoC 컨테이너라고 불리는 것은 ApplicationContext의 구현체이다.



## Bean

- 스프링 IoC 컨테이너에 의해 인스턴스화, 생성, 관리되는 자바 객체
- Bean 컨테이너에 존재하는 객체
- 싱글톤으로 존재
  - 싱글톤 : 어떤 Class가 최초 한번만 메모리를 할당하고(Static) 그 메모리에 객체를 만들어 사용하는 디자인 패턴
- 스프링에서의 Bean은 ApplicationContext가 알고있는 객체, 즉 ApplicationContext가 만들어서 그 안에 담고있는 객체를 의미한다.

### 1. Bean 생성 방법

#### Component Scanning

- IoC 컨테이너에 Bean을 등록할때 사용하는 인터페이스들을 Life Cycle Callback이라고 부른다.
- Life Cycle Callback 중에는 @Component가 붙어있는 모든 Class의 인스턴스를 생성해 Bean으로 등록하는 작업을 수행하는 Annotation Processor가 등록되어있다.
  - 인스턴스 : 일반적으로 실행 중인 임의의 프로세스, 해당 클래스의 구조로 컴퓨터 저장공간에서 할당되어 현재 생성된 Object를 의미
- **@ComponentScan**, **@Component** Annotation을 사용해서 Bean을 등록하는 방법이다.
- **@ComponentScan** : @Component가 부여된 Class를 찾아 자동으로 Bean으로 등록해주는 역할
- **@Component** : 실제로 찾아서 Bean으로 등록할 Class를 의미
  - @Component, @Controller, @Service, @Repository 등

#### Configuration

- @Configuration을 사용해서 직접 @Bean을 정의하면 자동으로 Bean으로 등록된다.

  ```java
  @Configuration
  public class ExampleConfiguration {
      @Bean
      public ExampleController exampleController() {
          return new ExampleController;
      }
  }
  ```

- XML 파일에 직접 Bean을 등록하여 설정하는 방법이 있다.

- 생성자가 하나고, 파라미터가 Bean인 객체는 @Bean 없이도 Bean으로 주입 가능

### 2. Bean 사용 방법

- **@Autowired**를 사용해서 Bean를 쓴다.

  

## DI (Dependency Injection)

###### 의존성 주입

- Setter Injection

  ```java
  B b = new B();
  A a = new A();
  a.setB(b);
  ```

- Construction Injection

  ```java
  B b = new B();
  A a = new A(b);
  ```

- Java에서도 많이 사용해 왔던 개념. 단지, 스프링에서는 이러한 일련의 과정을 (동적으로) 자동화 함.

- 개발 핵심 처리 루틴의 수정 없이 객체를 다른 객체로 쉽게 대체하여 생성 가능하도록 하는 역할을 함.
