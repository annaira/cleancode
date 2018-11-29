### Objects and Data Structures 

#### Chapter 6 of Clean Code ​

##### by Robert C. Martin​

---

#### Motivation​

- Encapsulation is good! We make our variables private so there is little dependence on them, and we are free to change the implementation!​
- But then we add getters and setters....​
        ​@box[text-orange span-80 fragment](Mars Attacks # Greetings earthlings. We come in peace!)
  
      Getters and Setters​
  
               vs​
  
           Encapsulation​


---

#### Data Abstractions
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
@[1-8](The implementation is completely hidden, it could have been done with polar coordinates or rectangular coordinates.)
@[1-8](Yet the implementation unmistakenly represents data structure.)
@[2-4](There is an enforced access policy.)
@[2-3](The individual coordinates can be read independently.)
@[4](But the coordinates have to be set together as an atomic operation.)

+++
#### Data Abstractions
###### Concrete Point
```Java
public class Point {
    public double x;
    public double y;
}
```
@[1-8](The concrete point is obviously implemented with rectangular coordinates.)
@[1-8](We are forced to manipulate the coordinates independently.)
@[1-8](The implementation is exposed and this would still be true with private variables and getters and setters)
+++

### Data Abstractions
- Implementation hiding is not about putting a layer of functions over variables
- **Implementation hiding is about abstractions**
- A class should expose abstract interfaces that allow its users to manipulate the _essence_ of the data

+++
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
@[1-7](Two examples of Java code representing the fuel level of a vehicle.)
@[1-3](The first examples uses concrete terms to communicate the fuel level.)
@[4-7](The second examples uses the abstraction of percentage to communicate the fuel level.)
@[1-3](We can be quite sure that these are just accessors of variables.)
@[4-7](In the abstract case we have no clue about the actual form of the data.)
@[4-7](This is preferable, as we do not want to expose the details of our data.)

+++
#### Data Abstractions

- Implementation hiding is not about putting a layer of functions over variables
- **Implementation hiding is about abstractions**
- A class should expose abstract interfaces that allow its users to manipulate the _essence_ of the data
- Data should be expressed in abstract terms, just using getters and setters is not enough.
- Serious thought has to be put into the best way to represent the data an object contains.
---

#### Data/Object Anti-Symmetry


#### Actionable Advice
#### Discussion
- Not blindly using getters and setters sounds nice but we need them for jooq in domain classes and they are the point of the DTO clases.
