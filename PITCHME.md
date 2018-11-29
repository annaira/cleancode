## Objects and Data Structures 

####Chapter 6 of Clean Code ​

###by Robert C. Martin​

---

@title[Motivation​]

- Encapsulation is good! We make our variables private so there is little dependence on them, and we are free to change the implementation!​
- But then we add getters and setters....​
        ​
  
      Getters and Setters​
  
               vs​
  
           Encapsulation​


---

@title[Data Abstractions]

Two examples of Java code representing a Point on the Cartesian plane.

##### Concrete Point
```Java
public class Point {
    public double x;
    public double y;
}
```
##### Abstract Point
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

Note:
- Abstract point: implementation is completely hidden, it could be done with polar coordinates or rectangular coordinates
- we don't care about the implementation
- Implementation unmistakenly represents data structure.
- Even more than that: the methods enforce an access policy:
- You can read the individual coordinates independently
- But the coordinates have to be set together as an atomic operation
- The Concrete Point is obviously implemented rectangular coordinates
- we are forced to manipulate the coordinates independently
- implementation is exposed, this would still be true with private variables + getters and setters



---

@title[Actionable Advice]

