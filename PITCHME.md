## Objects and Data Structures 

#### Chapter 6 of Clean Code ​

##### by Robert C. Martin​

---

### Motivation​

- Encapsulation is good! We make our variables private so there is little dependence on them, and we are free to change the implementation!​
- But then we add getters and setters....​
        ​@box[text-orange span-80 fragment](Mars Attacks # Greetings earthlings. We come in peace!)
  
      Getters and Setters​
  
               vs​
  
           Encapsulation​


---

### Data Abstractions
Two examples of Java code representing a Point on the Cartesian plane.
###### Concrete Point
```Java
public class Point {
    public double x;
    public double y;
}
```
###### Abstract Point
```Java
public interface Point{
  double getX();
  double getY();
  void setCartesian(double x, double y);
}
```
+++

###### Abstract Point
```Java
public interface Point{
  double getX();
  double getY();
  void setCartesian(double x, double y);
  double getR();
  double getTheta();
  void setPolar(double r, double theta);
}
```
@[1-8](implementation is completely hidden, it could have been done with polar coordinates or rectangular coordinates)
@[1-8](Implementation unmistakenly represents data structure)
@[2-3](enforced access policy: You can read the individual coordinates independently)
@[4](But the coordinates have to be set together as an atomic operation)


+++

### Data Abstractions
###### Concrete Point
```Java
public class Point {
    public double x;
    public double y;
}
```
@[1-8](The Concrete Point is obviously implemented rectangular coordinates)
@[1-8](We are forced to manipulate the coordinates independently)
@[1-8](implementation is exposed, this would still be true with private variables + getters and setters)

+++

### Data Abstractions

- Implementation hiding is not about putting a layer of functions over variables
- **Implementation hiding is about abstractions**
- A class should expose abstract interfaces that allow its users to manipulate the _essence_ of the data

+++

### Data Abstractions
Two examples of Java code representing the fuel in a vehicle.
###### Concrete Vehicle
```Java
public interface Vehicle {
    double getFuelTankCapacityInGallons();
    double getGallonsOfGasoline();
}
```
###### Abstract Vehicle
```Java
public interface Vehicle {
    double getPercentageFuelRemaining();
}
```
+++
---



### Actionable Advice

