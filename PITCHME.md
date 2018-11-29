### Objects and Data Structures 

#### Chapter 6 of Clean Code ​

##### by Robert C. Martin​

---
#### Motivation​

@quote[Encapsulation is good! We make our variables private so there is little dependence on them, and we are free to change the implementation!​]

@quote[But we need to access the data... Let's add getters and setters!]

---

### Data Abstractions
---
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
@[1-9](Two examples of Java code representing a Point on the Cartesian plane.)

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
@[1-4](The first examples uses concrete terms to communicate the fuel level.)
@[5-7](The second examples uses the abstraction of percentage to communicate the fuel level.)
@[1-4](We can be quite sure that these are just accessors of variables.)
@[5-7](In the abstract case we have no clue about the actual form of the data.)
@[5-7](This is preferable, as we do not want to expose the details of our data.)

+++
### Data Abstractions

- @color[gainsboro](Implementation hiding is not about putting a layer of functions over variables)
- @color[gainsboro](**Implementation hiding is about abstractions**)
- @color[gainsboro](A class should expose abstract interfaces that allow its users to manipulate the _essence_ of the data)
- Data should be expressed in abstract terms, just using getters and setters is not enough.
- Serious thought has to be put into the best way to represent the data an object contains.
---

### Data/Object Anti-Symmetry
- **Objects hide their data behind abstractions and expose functions that operate on the data.**
- **Data structures expose their data and have no meaningful functions.**
- These defintions are complimentary and virtual opposites.
- The difference may seem trivial but has far-reaching implications.
---
```Java
public class Square {
    public Point topLeft;
    public double side;
}
public class Rectangle {
    public Point topLeft;
    public double height;
    public double width;
}
public class Circle {
    public Point center;
    public double radius;
}
public class Geometry {
    public final double PI = 3.141592653589793;
    public double area(Object shape){
        if(shape instanceof Square) {
            Square square = (Square) shape;
            return square.side * square.side;
        } else if (shape instanceof Rectangle){
            Rectangle rectangle = (Rectangle) shape;
            return rectangle.height * rectangle.width;
        } else if (shape instanceof Circle) {
            Circle circle = (Circle) shape;
            return PI * circle.radius * circle.radius;
        }
        throw new NoSuchShapeException();
    }
}
```
#### Actionable Advice
---
#### Discussion
- Not blindly using getters and setters sounds nice but we need them for jooq in domain classes and they are the point of the DTO clases.
