# Oefening Observer Pattern
> We maken gebruik van twee interfaces: 'Observer' en 'Observable'. Daarnaast hebben we de observers: de objecten die wijzigingen aan onze data `observeren`. Uiteraard hebben we ook de data zelf.

> We hebben meerdere observers, dus we hebben een lijst van observers en functies om observers toe te voegen en te verwijderen. Dit staat in de 'Observer'-interface.

> We hebben een klasse die de data van ons weerstation voorstelt, hier 'WeatherData'. Dit is de `Observable`, want we observeren deze data.

> Als laatste hebben we de displays die deze data observeren. We gebruiken er hier slechts één: 'ForecastDisplay'. Uiteraard is het mogelijk om meerdere soorten te maken. Dit zijn de `Observers`.

> We maken eerst een weerstation aan. Daarna een display die de data van het weerstation gebruikt. Nu kunnen we initiële data doorgeven die normaalgezien van het weerstation zou komen. Daarna kunnen we ook nieuwe data doorgeven. Zorg er wel voor dat alle observers op de hoogte worden gebracht van de nieuwe data door 'notifyObservers()' uit te voeren!
## Klassediagram
Dit diagram biedt een overzichtelijke weergave van alle Has-A & Is-A relaties in onze applicatie.
![Klassediagram](assets/Class&#32;Diagram_Observer.svg)
## Code:
> Bij het copy pasten: vergeet je package niet toe te voegen!
> Voorbeeld: 'package edu.ap.mathiasv.dp_observerpattern;'

### Main
```java
public class Main {
    public static void main(String[] args) {
        WeatherData data = new WeatherData(); // Maak weerstation aan
        ForecastDisplay forecastDisplay = new ForecastDisplay(data); // maak display aan die gebruikmaakt van de data van ons weerstation
        data.setInitialData(25, 5, 12); // Initiële data
        System.out.println(forecastDisplay); // Print data (Display nieuwe data)
	   	data.setTemp(30); // *weerstation heeft nieuwe data* --> update data!
        System.out.println(forecastDisplay); // Print nieuwe data (Display nieuwe data)
    }
}
```
### Interface Observer
```java
public interface Observer {
    public void update();
}
```

### Interface Observable
```java
public interface Observable {
    public void registerObserver(Observer o);
    public void removeObserver(Observer o);
    public void notifyObservers();
}
```

### Class WeatherData (weerstation)
> De `OBSERVABLE`
```java
public class WeatherData implements Observable{
    
    private final ArrayList<Observer> observers;
    private float temp;
    private float humidity;
    private float pressure;

    public WeatherData(){
        observers = new ArrayList();
    }
    
    @Override
    public void registerObserver(Observer o){
        observers.add(o);
    }
    
    @Override
    public void removeObserver(Observer o){
        int i = observers.indexOf(o);
        if (i >= 0)
            observers.remove(i);
    }
    
    @Override
    public void notifyObservers(){
        for(Observer observer : observers){
            observer.update();
        }
    }
    
    public float getTemp(){
       return this.temp;
    }
    
    public float getHumidity(){
        return this.humidity;
    }
    
    public float getPressure(){
        return this.pressure;
    }
    
    public void setTemp(float temp){
       this.temp = temp;
       notifyObservers();
    }
    
    public void setHumidity(float humidity){
       this.humidity = humidity;
       notifyObservers();
    }
    
    public void setPressure(float pressure){
       this.pressure = pressure;
       notifyObservers();
    }
    
    public void setInitialData(float temp, float humidity, float pressure){
        this.temp = temp;
        this.humidity = humidity;
        this.pressure = pressure;
        notifyObservers();
    }
}
```

### Class ForecastDisplay
> Een `OBSERVER`
```java
public class ForecastDisplay implements Observer{
    private WeatherData data;
    private float temp;
    private float humidity;
    private float pressure;
    
    public ForecastDisplay(WeatherData data){
        this.data = data;
    }
    
    @Override
    public void update(){
        temp = data.getTemp();
        humidity = data.getHumidity();
        pressure = data.getPressure();
    }

    @Override
    public String toString() {
        this.update();
        String summary = temp + "°C, " + humidity + " humidity, " + pressure + " pressure.";
        return summary;
    }
}
```