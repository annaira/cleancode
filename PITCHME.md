# Objects and Data Structures 

Chapter 6 of Clean Code ​

by Robert C. Martin​

---

### Motivation​

- Encapsulation is good! We make our variables private so there is little dependence on them, and we are free to change the implementation!​
- But then we add getters and setters....​
        ​
  
      Getters and Setters​
  
               vs​
  
           Encapsulation​

---

### Data Abstractions

Two examples of Java code representing a Point on the Cartesian plane.


+++
@title[Concrete Point]

```Java
public class Point {
    public double x;
    public double y;
}
```
@[1-4](The implementation is completely exposed)

+++
@title[Abstract Point]

```Java
public class Point {
    public double x;
    public double y;
}
```
@[1-8](The implementation is completely hidden, it could be done with polar coordinates or rectangular coordinates, and that doesn't matter.)


Note:

- Offers a wide-range of enhanced presentation **tools** and **features**
- Both on the desktop and in the cloud
- Learn more on the GitPitch website at https://gitpitch.com

---
![Flux Explained](https://facebook.github.io/flux/img/flux-simple-f8-diagram-explained-1300w.png)


![Flux Explained](https://facebook.github.io/flux/img/flux-simple-f8-diagram-explained-1300w.png)



