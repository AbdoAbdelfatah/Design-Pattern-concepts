# Design Pattern Concepts

## Behavioral Pattern: Observer

The **Observer Design Pattern** is a behavioral pattern that defines a one-to-many dependency between objects. When one object (the **subject**) changes state, all its dependents (**observers**) are notified and updated automatically.

---

### Components of the Observer Design Pattern

1. **Subject**
    - Maintains a list of observers (subscribers/listeners).
    - Provides methods to register and unregister observers dynamically.
    - Notifies observers of changes in its state.

2. **Observer**
    - Defines an interface with an `update` method that concrete observers must implement.
    - Ensures a consistent way for observers to receive updates from the subject.

3. **ConcreteSubject**
    - Specific implementation of the subject.
    - Holds the actual state/data that observers want to track.
    - Notifies observers when its state changes.
    - *Example*: A weather station is a subject; individual weather stations in different locations are concrete subjects.

4. **ConcreteObserver**
    - Implements the observer interface.
    - Registers with a concrete subject and reacts when notified of a state change.
    - The subject's state changes trigger the observer's `update()` method.
    - *Example*: A weather app on your phone is a concrete observer reacting to updates from a weather station.

---

<p align="center">
<img width="600" alt="Observer Pattern Diagram" src="https://github.com/user-attachments/assets/52a015f9-e464-4fd3-a6aa-35a4e6c45421" />
</p>

---

### Example Code (Java)

```java
import java.util.ArrayList;
import java.util.List;

// Observer Interface
interface Observer {
    void update(String weather);
}

// Subject Interface
interface Subject {
    void addObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers();
}

// ConcreteSubject Class
class WeatherStation implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private String weather;
    
    @Override
    public void addObserver(Observer observer) {
        observers.add(observer);
    }
    @Override
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }
    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(weather);
        }
    }
    public void setWeather(String newWeather) {
        this.weather = newWeather;
        notifyObservers();
    }
}

// ConcreteObserver Class
class PhoneDisplay implements Observer {
    private String weather;
    @Override
    public void update(String weather) {
        this.weather = weather;
        display();
    }
    private void display() {
        System.out.println("Phone Display: Weather updated - " + weather);
    }
}

// ConcreteObserver Class
class TVDisplay implements Observer {
    private String weather;
    @Override
    public void update(String weather) {
        this.weather = weather;
        display();
    }
    private void display() {
        System.out.println("TV Display: Weather updated - " + weather);
    }
}

// Usage Example
public class WeatherApp {
    public static void main(String[] args) {
        WeatherStation weatherStation = new WeatherStation();
        Observer phoneDisplay = new PhoneDisplay();
        Observer tvDisplay = new TVDisplay();

        weatherStation.addObserver(phoneDisplay);
        weatherStation.addObserver(tvDisplay);

        // Simulating weather change
        weatherStation.setWeather("Sunny");
        // Output:
        // Phone Display: Weather updated - Sunny
        // TV Display: Weather updated - Sunny
    }
}
```

---

## When to Use the Observer Design Pattern

- When one object needs to notify multiple others about changes.
- When you want to keep objects loosely coupled.
- When observers should automatically respond to changes in the subject’s state.
- When you need flexibility to add/remove observers easily.
- When building event systems where components react without direct connections.

## When **Not** to Use the Observer Design Pattern

- When object relationships are simple and don’t require notifications.
- When performance is critical; many observers can cause overhead.
- When subject and observers are tightly coupled.
- When the number of observers is fixed and unchanging.
- When the order of notifications is crucial and must be predictable.

---
