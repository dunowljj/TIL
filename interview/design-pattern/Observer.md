Observer Pattern
===
- 옵저버 패턴(Observer Pattern)은 객체의 상태 변화를 관찰하는 관찰자들(옵저버)에게 그 변화를 알리는 디자인 패턴
  - 구독자, 고객들은 정보를 얻거나 받아야 하는 주체와 관계를 형성하게 된다. 
  - 관계가 지속되다가 정보를 원하지 않으면 해제할 수도 있다.
- ex) 안드로이드 개발시, OnClickListener와 같은 것들이 옵저버 패턴이 적용된 것 (버튼(Publisher)을 클릭했을 때 상태 변화를 옵저버인 OnClickListener로 알려주로독 함)
- 서로의 정보를 주고받는 과정에서 정보의 단위가 클수록, 객체들의 규모가 클수록 복잡성이 증가하게 된다. 이때 옵저버 패턴이 가이드라인을 제시해줄 수 있다.

### 구성 
- Subject: Observer들을 등록하거나 제거할 수 있는 메서드를 제공하고, Subject의 상태가 변하면 등록된 모든 Observer에게 알린다.
- Observer: Subject의 상태 변화를 알리는 업데이트 메서드를 구현한다.


### 장점
- Subject와 Observer 사이의 느슨한 결합(Loose Coupling)
  - Subject는 Observer에 대한 구체적인 정보를 알지 못하며, Observer 인터페이스만 알면 된다.
  - 이로 인해 새로운 Observer를 쉽게 추가하거나 기존 Observer를 수정하거나 제거할 수 있다.
- 브로드캐스트 통신: Subject는 자신의 상태 변화를 모든 Observer에게 알릴 수 있습니다.
- 단일 책임 : 상태 변경에 대한 알림 로직과 실제 비즈니스 로직을 분리할 수 있다.

### 단점
- 메모리 누수: Observer가 Subject를 구독한 채로 해제되지 않으면 메모리 누수가 발생할 수 있다.
- 순서 문제: Observer들이 불린 순서가 중요한 경우, Observer 패턴은 이를 보장하지 않는다. 추가적인 방법을 써야한다.

## gpt 예시 코드
### Observer 인터페이스와 Subject 인터페이스

```java
public interface Observer {
    public void update(float temp, float humidity, float pressure);
}

public interface Subject {
    public void registerObserver(Observer o);
    public void removeObserver(Observer o);
    public void notifyObservers();
}
```

### Subject 구현체
```java
import java.util.ArrayList;

public class WeatherData implements Subject {
    private ArrayList<Observer> observers;
    private float temperature;
    private float humidity;
    private float pressure;

    public WeatherData() {
        observers = new ArrayList<Observer>();
    }

    public void registerObserver(Observer o) {
        observers.add(o);
    }

    public void removeObserver(Observer o) {
        int i = observers.indexOf(o);
        if (i >= 0) {
            observers.remove(i);
        }
    }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(temperature, humidity, pressure);
        }
    }

    public void measurementsChanged() {
        notifyObservers();
    }

    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        measurementsChanged();
    }
}
```
- registerObserver로 Observer를 등록
- removeObserver로 Observer를 제거
- measurementsChanged()를 호출해서 데이터 변경을 알린다.
### Observer 구현체
```java
public class CurrentConditionsDisplay implements Observer {
    private float temperature;
    private float humidity;
    private Subject weatherData;

    public CurrentConditionsDisplay(Subject weatherData) {
        this.weatherData = weatherData;
        weatherData.registerObserver(this);
    }

    public void update(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        display();
    }

    public void display() {
        System.out.println("Current conditions: " + temperature + "F degrees and " + humidity + "% humidity");
    }
}

```
- update 될때마다 display() 하도록 구현되어 있다.
- Observer 구현체를 생성하면서 Subject를 주입받고, 해당 Subject에 등록해서 구독설정을 한다.
- 잘 보면 pressure을 사용하지 않지만 넘긴다. 특정 정보를 사용하지 않는 옵저버도 있을 수 있지만, 다른 옵저버의 필요를 대비해 확장성을 보장한다.
  - 물론 상황에 따라 사용하는 정보만 전달하도록 메서드를 수정하는 것이 효율적일 수 있다.

## 참고 및 출처
- https://gyoogle.dev/blog/design-pattern/Observer%20Pattern.html