### Unit Tests

#### Chapter 9 of Clean Code ​

##### by Robert C. Martin​

---
### Testing in the 90s

A simple timer written in C++ with the following signature:
```c++
void Timer::ScheduleComand(Comand* theComand, int milliseconds)
```
@[1](Simple idea: `execute` of `Command` would be executed in a new thread after the specified number of milliseconds.)
@[1](But how can that be tested?)

+++
@quote[I cobbled together a simple driver programm that listened to the keyboard. Every time a character was typed, it would schedule a command that would type the character five seconds later. Then I tapped out a rhythmic melody on the keyboard and waited for that melody to replay on the screen five seconds later.]

Note:

- How would it be done today?
- Write tests that test every possible detail.
- Isolate code from operating system
- mock out those timing functions for control over time
- test suite would be convenient for everyone else to run
- there are still some subtle but important points about testing left
---

### The three laws of TDD

- **First Law** You may not write production code until you have written a failing unit test.
- **Second Law** You may not write more of a unit test than is sufficient to fail, and not compiling is failing.
- **Third Law** You may not write more production code than is sufficient to pass the current failing test.

Note:
- writing unit tests first is just the tip of the iceberg when it comes to TDD
- the three laws result in a 30s cycle
- test and production code are written together within seconds
- this way there are a lot of test, there might be more test code than production code

---

### Keeping tests clean
- dirty tests are not better than no tests
- tests musts change as production code evolves
- low test code quality leads to high maintenance costs
- Test code is just as important as production code.
+++

#### Tests enable the -ilities
Unit tests ensure the
 
  - flexibility,
  - maintainability, and 
  - reusability
 
 
of the code base.
 
 
 - No tests lead to fear of undetected bugs.
 - The higher the test coverage the less the fear.

---
### Clean tests

Readability of test code is more important than readability of production code.

How to achieve readability?


- clarity
- simplicity
- density of expression

+++
```Java
public void testGetPageHieratchyAsXml() throws Exception {
    crawler.addPage(root, PathParser.parse("PageOne"));        
    crawler.addPage(root, PathParser.parse("PageOne.ChildOne"));        
    crawler.addPage(root, PathParser.parse("PageTwo"));
    
    request.setResource("root");
    request.addInput("type", "pages");
    Responder responder = new SerializedPageResponder();
    SimpleResponse response = (SimpleReponse) responder.makeResponse(new FitNesseContext(root), request);
    String xml = response.getContent();
    
    assertEquals("text/xml", response.getContentType());
    assertSubString("<name>PageOne</name>", xml);
    assertSubString("<name>PageTwo</name>", xml);
    assertSubString("<name>ChildOne</name>", xml);        
}
```
@[1-16](This test is rather difficult to understand)
@[2-6](There is a lot of duplicate code: repeated calls to `addPage` and `assertSubString`)
@[11-15](There is a lot of duplicate code: repeated calls to `addPage` and `assertSubString`)
@[1-16](The test is loaded with details that cloud the expressiveness of the test.)

---
### Discussion
- 
- 
- 