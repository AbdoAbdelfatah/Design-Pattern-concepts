# Design-Pattern-concepts
Behavioral Pattern: 
  i. Observer 
    The Observer Design Pattern is a behavioral design pattern that defines a one-to-many dependency between objects. When one object (the subject) changes state, all its dependents (observers) are notified and          updated automatically. It primarily deals with the interaction and communication between objects, specifically focusing on how objects behave in response to changes in the state of other objects.
    
   
  Components of Observer Design Pattern:
            i.Subject:
                The subject maintains a list of observers (subscribers or listeners).
                It Provides methods to register and unregister observers dynamically and defines a method to notify observers of changes in its state.
        
            ii.Observer:
                Observer defines an interface with an update method that concrete observers must implement and ensures a common or consistent way for concrete observers to receive updates from the subject. 
            iii. ConcreteSubject:
                ConcreteSubjects are specific implementations of the subject. They hold the actual state or data that observers want to track. When this state changes, concrete subjects notify their observers.
                For instance, if a weather station is the subject, specific weather stations in different locations would be concrete subjects.    
            iv.ConcreteObserver:
                Concrete Observer implements the observer interface. They register with a concrete subject and react when notified of a state change.
                When the subject's state changes, the concrete observer's update() method is invoked, allowing it to take appropriate actions.
                For example, a weather app on your smartphone is a concrete observer that reacts to changes from a weather station.   

        
                
<img width="1242" height="544" alt="image" src="https://github.com/user-attachments/assets/52a015f9-e464-4fd3-a6aa-35a4e6c45421" />

Code: 
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
// Usage Class
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

When to use the Observer Design Pattern?
Below is when to use observer design pattern:

When you need one object to notify multiple others about changes.
When you want to keep objects loosely connected, so they don’t rely on each other’s details.
When you want observers to automatically respond to changes in the subject’s state.
When you want to easily add or remove observers without changing the main subject.
When you’re dealing with event systems that require various components to react without direct connections.
When not to use the Observer Design Pattern?
Below is when not to use observer design pattern:

When the relationships between objects are simple and don’t require notifications.
When performance is a concern, as many observers can lead to overhead during updates.
When the subject and observers are tightly coupled, as it defeats the purpose of decoupling.
When number of observers is fixed and won’t change over time.
When the order of notifications is crucial, as observers may be notified in an unpredictable sequence.

            
