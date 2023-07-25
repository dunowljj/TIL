Singleton pattern
===
- 애플리케이션이 시작될 때, 최초 한 번만 메모리를 할당하고, 해당 메모리에 인스턴스를 만들어 사용한단.
- 단 하나의 인스턴스만 생성하여 사용하는 디자인 패턴이다.
- 생성자 최초 호출 시에 인스턴스를 생성하고, 생성자를 여러번 호출해도 이미 생성된 객체를 반환시키는 방식으로 구현한다.

### 장점
- 한 번 객체를 생성하고 재사용해서 성능을 높일 수 있다. 
- 메모리 낭비를 방지할 수 있다. 
- 전역으로 구현하기 때문에 다른 클래스의 인스턴스들이 데이터를 공유하는 것이 가능하다.

### 단점
- 개방-폐쇄 원칙을 위배할 수 있다. 유지보수가 어려워진다.
  - 싱글톤 인스턴스가 혼자 너무 많은 일을 하거나, 많은 데이터를 공유시키면 클래스들 간의 결합도가 높아진다.
- 테스트가 어려워진다. 
  - 이미 설정이 끝나서 인스턴스를 가져와 버린다. 내부 속성을 지정하거나 변경하기 어렵다.
  - 전역적 접근때문에 테스트 격리가 어렵다. 
  - 다른 테스트에 영향을 줄 수 있어서 테스트가 순서에 의존적이게 된다.
  - 생성자가 private이라 Mocking이 어렵다. 의존성 주입 등을 이용해 Mock을 사용하려면 추가적인 설계가 필요하다.
- 멀티 스레드 환경에서 동시성 문제를 해결해야 한다.

### 사용
- 데이터베이스 커넥션풀, 스레드풀, 캐시, 로그 기록 객체 등
- 인스턴스가 절대적으로 한 개만 존재하는 것을 보증하고 싶을 때

## 구현

### Eager Initialization
```java
class Singleton {
	
	private static Singleton singleton = new Singleton(); 
	
	private Singleton() {}
	
	public static Singleton getInstance() {
		return singleton;
	}
}
```
- static을 사용해서 class가 로드될때 객체를 생성한다.
- private으로 생성자 접근 불가
- 객체를 미리 생성하는 단순하고 안전한 방법

스프링 핵심원리 기본에서도 이정도만 설명했다. 스프링에서 복잡한 처리를 알아서 해준다.

한계
+ 객체가 무조건 생성되기 때문에 자원 낭비가 생길 수 있다. 
+ Exception 처리를 하지 않는다.

### Static Block Initialization
```java
class Singleton {
	private static Singleton singleton; 
	
	private Singleton() {}
	
	//
	static {
		try {
			singleton = new Singleton();
		} catch (Exception e) {
			throw new RuntimeException("Exception occured in creating singleton instance");
		}
	}
	
	public static Singleton getInstance() {
		return singleton;
	}
}
```
- Static Block을 써서 Exception을 처리헀다.
  - 클래스가 처음 로딩 될때 객체를 생성

한계
+ 클래스 로딩단계에서 객체를 생성하기 때문에 자원의 비효율성은 그대로이다.

### Lazy Initialization
```java
class Singleton {
	private static Singleton singleton; 
	
	private Singleton() {} 
	
	public static Singleton getInstance() {
		if (singleton == null) singleton = new Singleton();
		return singleton;
	}
}
```
- null 체크를 한다. 객체가 존재하지 않으면 생성하고, 존재하면 최초에 생성한 객체를 반환한다.
- 필요할 때만 생성하기 때문에 자원의 비효율성을 해결한다.

한계
+ 이전의 두 사례와 다르게 멀티쓰레드 환경에서 동기화 문제가 발생할 수 있다.

### Thread Safe Singleton
```java
class Singleton {
	private static Singleton singleton; 
	
	private Singleton() {}
	
	public static synchronized Singleton getInstance() {
		if (singleton == null) singleton = new Singleton();
		return singleton;
	}
}
```
- 단순하게 synchronized를 사용해서 해결

한계
- 객체를 생성한 후 접근할때마다 synchronized를 호출하고, 성능이 저하된다.

### Double Checked Locking
```java
class Singleton {
	private volatile static Singleton singleton; 

	private Singleton() {}

	public static Singleton getInstance(){
	    if (singleton == null){
	        synchronized (Singleton.class) {
	            if(singleton == null) singleton = new Singleton();
	        }
	    }
	    return singleton;
	}
}
```
- 객체가 null일 경우에만 동기화를 실행하도록 한다.
  - 객체가 한번 생성되면 synchronized가 발생하지 않는다.
  - synchronized 호출 비용 절약

### Initialization-on-demand holder idiom (Bill Pugh Singleton Implementation)
```java
class Singleton {

	private Singleton() {}

	private static class SingletonHelper {
		private static final Singleton SINGLETON = new Singleton();
	}
	
	public static Singleton getInstance(){
	    return SingletonHelper.SINGLETON;
	}
}
```
- Inner Class로 SingletonHelper 클래스를 선언했다. 
- Class Loader에 의해 로딩될때 로딩되지 않고 getInstance()가 호출될 때 JVM 메모리에 로드되고 객체를 생성하게 된다.

한계
- Reflectionㅇ르 쓰면 private 생성자에 접근이 가능해지고, 객체가 여러개 생길 수 있다.

### Enum Singleton
```java
enum EnumSingleton {
	INSTANCE;

	public static void doSomething(){
        //do something
    }
}
```
- 동기화, Reflection 모두 해결해 준다.
- Lazy가 아니라 비효율성은 해결 불가.
- 상속이 필요한 경우 사용할 수 없다.

## 참고 및 출처
- https://gyoogle.dev/blog/design-pattern/Singleton%20Pattern.html
- https://sorjfkrh5078.tistory.com/108