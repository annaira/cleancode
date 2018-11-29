### Objects and Data Structures 

#### Chapter 6 of Clean Code ​

##### by Robert C. Martin​

---
#### Motivation​

@quote[Encapsulation is good! We make our variables private so there is little dependence on them, and we are free to change the implementation!​]

@quote[But we need to access the data... Let's add getters and setters!]

---

### Data Abstractions
+++
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
+++
```Java
public class Square {
    public double side;
}
public class Rectangle {
    public double height;
    public double width;
}
public class Circle {
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
@[1-28](This is a procedural shape example.)
@[1-14](The shape classes are simple data structures without any behaviour.)
@[14-28](All the behaviour is in the geometry class.)
@[14-28](This design is procedural and not object-oriented.)
@[14-28](There are circumstances where this can be the right choice.)
@[14-28](What would happen if we add a `perimeter()` function to `Geometry`?)
@[1-14](The shape classes would not be affected!)
@[1-14](And any other classes depending on the shape classes would not be affected.)
@[1-14](What happens if we add a new shape?)
@[14-28](We must change all the functions in `Geometry`.)
@[1-29](The two conditions are diametrically opposed.)
+++
```Java
public class Square implements Shape {
    private double side;
    public double area() {
        return side * side; 
    }
}
public class Rectangle implements Shape {
    public double height;
    public double width;
    public double area() { 
        return height * width; 
    }
}
public class Circle implements Shape {
    public final double PI = 3.141592653589793;
    public double radius;
    public double area() { 
        return PI * radius * radius; 
    }
}
```
@[1-21](This is an object-oriented solution.)
@[1-21](The `area()` method is polymorphic.)
@[1-21](The `Geometry` class is not needed anymore.)
@[1-21](What would happen if we add a `perimeter()` function to `Shape`?)
@[1-21](The shape classes would all be affected!)
@[1-21](What happens if we add a new shape?)
@[1-21](The existing functions are not affected.)
+++
#### There is a fundamental dichotomy between objects and data structures.

@quote[Procedural code (code using data structures) makes it easy to add new functions without changing the existing data structures. OO code, on the other hand, makes it easy to add new classes without changing existing functions.​]

@quote[Procedural code makes it hard to add new data structures because all the functions must change. OO code makes it hard to add new functions because all the classes must change.]
+++
### Data/Object Anti-Symmetry
- Objects 
    - hide their data behind abstractions and
    - expose functions that operate on the data.
- Data structures 
    - expose their data and
    - have no meaningful functions.
---
### The Law of Demeter



---
### Actionable Advice

Reflect whether the problem at hand is better solved with new data structures or new functions.
When we want to add new data types, objects and OO are most appropriate.
When we want to add new functions, procedural code and data structures will be more appropriate.
---
### Discussion
- Not blindly using getters and setters sounds nice but we need them for jooq in domain classes and they are the point of the DTO clases.
