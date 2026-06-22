## Observer Pattern Implementation (Java)

## Pattern Opportunity Analysis

The original WeatherStation class has a design problem because it directly controls all display updates by calling:

updateCurrentConditionsDisplay()
updateStatisticsDisplay()
updateForecastDisplay()
updateHeatIndexDisplay()

This creates tight coupling between the weather station and the display logic.

If a new display type is added, the WeatherStation class must be modified. This violates the Open/Closed Principle, which states that software should be open for extension but closed for modification.

The Observer Pattern is suitable because multiple display objects need to receive updates whenever the weather data changes.

##  Observer Pattern Design

The solution introduces two main components:

# Subject (Weather Data Provider)

The WeatherStation acts as the subject that stores weather measurements and notifies observers when data changes.

# Observers (Display Elements)

Each display becomes an independent observer that receives weather updates and displays information in its own way.

The design includes:

WeatherObserver interface for all displays.
CurrentConditionsDisplay class.
StatisticsDisplay class.
ForecastDisplay class.
HeatIndexDisplay class.
WeatherStation managing a list of observers.



##   Refactored Code Structure

The following structure is used:

WeatherObserver
       ↑
       |
---------------------------------
|          |          |          |
Current  Statistics Forecast HeatIndex
Display   Display     Display   Display

               ↑
               |
        WeatherStation
       (Subject/Publisher)







##  Refactored Implementation 
# Observer Interface

public interface WeatherObserver {
    void update(float temperature, float humidity, float pressure);
}

# Current Conditions Display
public class CurrentConditionsDisplay implements WeatherObserver {

    @Override
    public void update(float temperature, float humidity, float pressure) {
        System.out.println(
            "Current conditions: " +
            temperature + "°F, " +
            humidity + "% humidity"
        );
    }
}

# Statistics Display

public class StatisticsDisplay implements WeatherObserver {

    @Override
    public void update(float temperature, float humidity, float pressure) {

        System.out.println(
            "Weather statistics: Avg/Max/Min temperature = " +
            (temperature - 2) + "/" +
            (temperature + 2) + "/" +
            (temperature - 5)
        );
    }
}

# Forecast Display

public class ForecastDisplay implements WeatherObserver {

    @Override
    public void update(float temperature, float humidity, float pressure) {

        String forecast =
            pressure < 29.92f
            ? "Watch out for cooler, rainy weather"
            : "Improving weather on the way!";

        System.out.println("Forecast: " + forecast);
    }
}


# Heat Index Display

public class HeatIndexDisplay implements WeatherObserver {

    @Override
    public void update(float temperature, float humidity, float pressure) {

        float heatIndex = (temperature + humidity) / 2;

        System.out.println(
            "Heat Index: " + heatIndex
        );
    }
}

# Refactored Weather Station (Subject)

import java.util.ArrayList;
import java.util.List;

public class WeatherStation {

    private float temperature;
    private float humidity;
    private float pressure;

    private List<WeatherObserver> observers =
            new ArrayList<>();


    public void addObserver(WeatherObserver observer) {
        observers.add(observer);
    }


    public void removeObserver(WeatherObserver observer) {
        observers.remove(observer);
    }


    public void notifyObservers() {

        for (WeatherObserver observer : observers) {

            observer.update(
                temperature,
                humidity,
                pressure
            );
        }
    }


    public void setMeasurements(
        float temperature,
        float humidity,
        float pressure
    ) {

        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;

        notifyObservers();
    }
}



##  Example Usage

public class Main {

    public static void main(String[] args) {

        WeatherStation station =
            new WeatherStation();

        station.addObserver(
            new CurrentConditionsDisplay()
        );

        station.addObserver(
            new StatisticsDisplay()
        );

        station.addObserver(
            new ForecastDisplay()
        );

        station.addObserver(
            new HeatIndexDisplay()
        );


        station.setMeasurements(
            80.0f,
            65.0f,
            30.4f
        );

    }
}



##  Testing the Refactoring

The following tests should be done to ensure that the refactoring does not change the original behavior:

Verify that all display objects are updated when weather measurements change.
Test to ensure that the class of WeatherStation does not have to be modified when a new observer is added.
Ensure the removal of an observer disables the observer from receiving updates.
Make sure that the information calculated and displayed by each display is correct.


##  Advantages of Observer Pattern

The new design has a number of enhancements:

# Reduced Coupling

The WeatherStation no longer relies on classes for displays.

# Easier Extension

To add a new display, there are no changes to the existing code required, just an implementation of the WeatherObserver interface.

# Better Maintainability

The code is easier to read and maintain since each display has only one job.

# Improved Reusability

Classes for displaying can be reused with other weather data sources.

# Better Organization

The system is designed using the object-oriented principles and there is better separation of the responsibility.



##  Reflection Answers
# What was the effect on maintainability of using the pattern?

The Observer Pattern was about to introduce a separation between the management of weather data and the display logic. The WeatherStation only controls the weather measurement and notification while the displays are in charge of their presentation logic.

# Which changes in the future will be simpler due to this pattern?

Adding a mobile display, website display or logging system in the future can be easily accomplished, simply by creating a new observer while retaining the same weather station code.

# What did you find surprising or challenging about using the pattern?

Deciding what form the updates to the observers should take and designing a communication interface between the subject and observers were the main difficulties. This problem has been addressed by developing a common WeatherObserver interface.