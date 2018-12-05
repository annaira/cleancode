### Unit Tests

#### Chapter 9 of Clean Code ​

##### by Robert C. Martin​

---
#### Motivation​

##### Testing in the 90s

A simple timer written in C++ with the following signature:
```c++
void Timer::ScheduleComand(Comand* theComand, int milliseconds)
```
@[1](Simple idea: `execute` of `Command` would be executed in a new thread after the specified number of milliseconds.)
@[1](But how can that be tested?)

+++

@quote[I cobbled together a simple driver programm that listened to the keyboard. Every time a character was typed, 
it would schedule a command that would type the character five seconds later. Then I tapped out a rhythmic melody on the 
keyboard and waited for that melody to replay on the screen five seconds later.]

Note:

- How would it be done today?
- Write tests that test every possible detail.
- Isolate code from operating system
- mock out those timing functions for control over time
- test suite would be convenient for everyone else to run
- there are still some subtle but important points about testing left
---

### The three laws of TDD

Note:
- writing unit tests first is just the tip of the iceberg when it comes to TDD
- the three laws result in a 30s cycle
- test and production code are written together within seconds
- this way there are a lot of test, there might be more test code than production code

- **First Law** You may not write production code until you have written a failing unit test.
- **Second Law** You may not write more of a unit test than is sufficient to fail, and not compiling is failing.
- **Third Law** You may not write more production code than is sufficient to pass the current failing test.

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
+++
- **Objects hide their data behind abstractions and expose functions that operate on the data.**
- **Data structures expose their data and have no meaningful functions.**
- These definitions are complimentary and virtual opposites.
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
@[1-26](This is a procedural shape example.)
@[1-10](The shape classes are simple data structures without any behaviour.)
@[11-26](All the behaviour is in the geometry class.)
@[11-26](This design is procedural and not object-oriented.)
@[11-26](There are circumstances where this can be the right choice.)
@[11-26](What would happen if we add a `perimeter()` function to `Geometry`?)
@[1-10](The shape classes would not be affected!)
@[1-10](And any other classes depending on the shape classes would not be affected.)
@[1-10](What happens if we add a new shape?)
@[11-26](We must change all the functions in `Geometry`.)
@[1-26](The two conditions are diametrically opposed.)
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

@quote[Procedural code makes it easy to add new functions without changing the existing data structures. OO code, on the other hand, makes it easy to add new classes without changing existing functions.​]
+++
#### There is a fundamental dichotomy between objects and data structures.

@quote[Procedural code makes it hard to add new data structures because all the functions must change. OO code makes it hard to add new functions because all the classes must change.]
+++
### Data/Object Anti-Symmetry
- Objects 
    - hide their data behind abstractions and
    - expose functions that operate on the data and
    - make it easy to add more objects.
- Data structures 
    - expose their data and
    - have no meaningful functions and
    - make it easy to add new functions.
---
### The Law of Demeter
+++
- The **Law of Demeter (LoD)** is also called **principle of least knowledge**.
- It is a specific case of loose coupling.
- It says that a method `f` of a class `C` should only call the methods of:
    - `C`
    - objects created by `f`
    - objects passed as an argument to `f`
    - objects held in an instance variable of `C`
- In other words: Talk to friends, don't talk to strangers.

+++
#### Train Wrecks
```Java
final String outputDir = ctxt.getOptions() 
                             .getScratchDir()
                             .getAbsolutePath();
```
- violates LoD because it calls the `getScratchDir()` function on the return value of `getOptions()` 
    and then calls `getAbsolutePath()` on the return value of `getScratchDir() `
- This kind of code is often called a _train wreck_ because it looks like coupled train cars.
- Chains of calls like this are generally considered to be sloppy style and should be avoided.
+++
- It is better to split the code up as follows:
```Java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsolutePath();
```
- Even less confusing is the use of public variables of data structures:
```Java
final String outputDir = ctxt.options.scratchDir.absolutePath;
```
+++
#### Hybrids
- This confusion can lead to objects that are half object and half data structure.
- They have functions that do significant things and
- public variables or accessors that make the private variables public
- It is hard to add functions and also hard to add new data structures
- They are the worst of both worlds.
+++
#### Hiding structure
- What if `ctxt`, `options`, and `scratchDir` are objects with real behaviour?
- As objects should hide their internal structure we should not navigate through them.
- How to get the absolute path of the scratch directory then?
```Java
ctxt.getAbsolutePathOfScratchDirectoryOption;
```
- This would lead to an explosion of methods in the `ctxt` object.
+++
```Java
ctxt.getScratchDirectoryOption().getAbsolutePath();
```
- This presumes that `getScratchDirectoryOption()` returns a data structure and not an object.
- Both options don't feel good
- If `ctxt` is an object, we should be telling it to _do something_; we should not be asking it about its internals.
```Java
OutputStream os = ctxt.createScratchFileStream(classFileName);
```
- This allows `ctxt` to hide its internals and prevents the current function from having to violate the Law of Demeter.
---
### Data Transfer Objects
+++
- The quintessential form of a data structure is a class with public variables and no functions.
- These are called data transfer objects, short DTO.
- Beans have private variables manipulated by getters and setters.
- The quasi-encapsulation of beans seems to make some OO purists feel better but usually provides no other benefit.
+++
#### Beans
```Java
public class Address {
    private String street;
    private String city;
    
    public Address(String street, String city){
        this.street = street;
        this.city = city;
    }
    public String getStreet (){
        return street;
    }
    public String getCity(){    
        return city;
    }
}
```
+++
#### Active Record
- Active Records are special forms of DTOs.
- They are data structures with public variables, but they typically have navigational methods like save and find.
- They are direct translations of data sources.
- They should not be treated as objects, as this creates a hybrid.
---
### Actionable Advice
- Reflect whether the problem at hand is better solved with new data structures or new functions.
- When we want to add new data types, objects and OO are most appropriate.
- When we want to add new functions, procedural code and data structures will be more appropriate.
---
### Discussion
- Uncle Bob would call our DTOs beans. Why do they have getters and setters?
- Not blindly using getters and setters sounds nice but we need them for jooq in domain classes and they are the point of the DTO clases.
- Method chaining is not a train wreck and not violating the Law of demeter.