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

+++
### The three laws of TDD

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

- Readability of test code is more important than readability of production code.
- How to achieve readability?
    - clarity
    - simplicity
    - density of expression

+++
### Clean tests

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
@[2-4](There is a lot of duplicate code: repeated calls to `addPage`)
@[13-15](There is a lot of duplicate code: repeated calls to `assertSubString`)
@[1-16](The test is loaded with details that cloud the expressiveness of the test.)
@[2-4](The `PathParser` call transforms strings into `PagePath` instances used by the crawler.)
@[2-4](This is irrelevant to the test and obfuscates intent.)
@[8-10](This is just noise.)
@[1-16](This code was not designed to be read.)

+++
### Clean tests

```Java
public void testGetPageHierarchyAsXml() throws Exception {
    makePages("PageOne", "PageOne.ChildOne", "PageTwo");
    
    submitRequest("root", "type:pages");
    
    assertResponseIsXml();
    assertResponseContains("<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>");        
}
```
@[1-16](This is the improved version)
@[1-16](The Build-Operate-Check pattern is obvious from the structure.)
@[2](The first part builds up the test data,)
@[4](the second part operates on that test data,)
@[6-7](and the third part checks that the operation yielded the expected results.)

+++
#### Domain-Specific Testing Languages

- build a domain-specific language for your tests
- this makes tests more convenient to write and easier to read
- the testing language evolves from continued refactoring

+++
#### A Dual Standard
- Test code does have different standards than production code.
- It must still be **simple, succinct and expressive**
- but it does **not need to be as efficient** as production code.

+++
#### A Dual Standard
```Java
@Test
public void turnOnLoTempAlarmAtThreashold() throws Exception {
    hw.setTemp(WAY_TOO_COLD);
    controller.tic();
    assertTrue(hw.heaterState());
    assertTrue(hw.blowerState());
    assertFalse(hw.coolerState());
    assertFalse(hw.hiTempAlarm());
    assertTrue(hw.loTempAlarm());
}
```
@[1-10](This test was part of a prototype for an environment control system.)
@[1-10](It checks that the low temperature alarm, the heater, and the blower are all turned on when the temperature is "way too cold".)
@[4](What is this `tic()` function?)
@[1-10](This should not be the reader's concern!)
@[1-10](The reader should think about whether they agree that the end state of the system is consistent with the temperature being "way too cold".)
@[1-10](While reading the test, the eyes bounce back and forth between the name and the sense of the state being checked.)
+++
#### A Dual Standard
```Java
@Test
public void turnOnLoTempAlarmAtThreshold() throws Exception {
    wayTooCold();
    assertEquals("HBchL", hw.getState());
}
```
@[1-10](This is an improved version of the test.)
@[3](The detail of the `tic()` function is hidden now.)
@[4](Upper case means "on", lower case means "off".)
@[4](The letters are in the order `{heater, blower, cooler, hi-temp-alarm, lo-temp-alarm}`.)
@[4](This violates some clean code rules, but once it is understood, it is very easy to read.)
+++
#### A Dual Standard
```Java
@Test
public void turnOnCoolerAndBlowerIfTooHot() throws Exception {
    tooHot();
    assertEquals("hBChl", hw.getState());
}
@Test
public void turnOnHeaterAndBlowerIfTooCold() throws Exception {
    tooCold();
    assertEquals("HBchl", hw.getState());
}
@Test
public void turnOnHiTempAlarmAtThreshold() throws Exception {
    wayTooHot();
    assertEquals("hBCHl", hw.getState());
}
@Test
public void turnOnLoTempAlarmAtThreshold() throws Exception {
    wayTooCold();
    assertEquals("HBchL", hw.getState());
}
```
+++
#### A Dual Standard
```Java
public String getState() {
    String state = "";
    state += heater ? "H" : "h";
    state += blower ? "B" : "b";
    state += cooler ? "C" : "c";
    state += hiTempAlarm ? "H" : "h";
    state += loTempAlarm ? "L" : "l";
}
```
@[1-10](This is the `getState()` function.)
@[1-10](It is not very efficient, Uncle Bob thinks he should probable have used `StringBuffer`, but it's okay in test code.)
---
### One Assert per Test
- Every test function in a JUnit test should have one and only one assert statement.
- This may seem draconian, but it has advantages.
+++
### One Assert per Test
```Java
public void testGetPageHierarchyAsXml() throws Exception {
   givenPages("PageOne", "PageOne.ChildOne", "PageTwo");
   
   whenRequestIsIssued("root", "type:pages");
       
   thenResponseShouldBeXML();
}
public void testGetPageHierarchyHasRightTags() throws Exception {
   givenPages("PageOne", "PageOne.ChildOne", "PageTwo");
   
   whenRequestIsIssued("root", "type:pages");
       
   thenResponseShouldContain(
       "<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>");        
}
```
+++
### One Assert per Test
- The tests come to a single conclusion and are easy to understand.
- The common _given-when-then_ convention is also used, making it even easier to read.
- A disadvantage is duplicated code caused by the split.
+++
### One Assert per Test
- One solution can be the Template Method pattern
    - put _given/when_ parts in the base class
    - put _then_ parts in different derivatives
+++
### One Assert per Test
- Another one would be separate test classes
    - put _given/when_ in `@Before`
    - put _then_ in each `@Test`
+++
### One Assert per Test
- This would be overkill for our example, Uncle Bob prefers the old version.
- It is a good guideline, we should strive to keep the number of assertions minimal.
+++
#### Single Concept per Test
```Java
public void testAddMonths() {
    SerialDate d1 = SerialDate.createInstance(31, 5, 2004);
    
    SerialDate d2 = SerialDate.addMonths(1, d1);
    assertEquals(30, d2.getDayOfMonth());
    assertEquals(6, d2.getMonth());
    assertEquals(2004, d2.getYYYY());
    
    SerialDate d3 = SerialDate.addMonths(2, d1);
    assertEquals(31, d3.getDayOfMonth());
    assertEquals(7, d3.getMonth());
    assertEquals(2004, d3.getYYYY());
    
    SerialDate d4 = SerialDate.addMonths(1, SerialDate.addMonths(1, d1));
    assertEquals(30, d4.getDayOfMonth());
    assertEquals(7, d4.getMonth());
    assertEquals(2004, d4.getYYYY());
}
```
+++
#### Single Concept per Test
- The test for `addMonths() should be split up into three independent tests because it tests three independent things.
- _Given_ the last day of may (31st)
    - _when_ you add 1 month, _then_ the day is the 30th
    - _when_ you add 2 months, _then_ the day is the 31st   
- _Given_ the last day of june (30th)
    - _when_ you add 1 month, _then_ the day is the 30th
- The rule is more obvious when stated this way!
+++
#### Single Concept per Test

The best rule is probably to minimize the number of asserts per concept and test just one concept per function.
---
### F.I.R.S.T.
+++
### F.I.R.S.T.
- **F**ast
- **I**ndependent
- **R**epeatable
- **S**elf-Validating
- **T**imely 
+++
### F.I.R.S.T.
**Fast** Tests should be fast. They should run quickly. When tests run slow, you won't want to run them frequently.
If you don't run them frequently, you won't find problems early enough to fix them easily. You won't feel as free to clean up the code.
Eventually the code will begin to rot.
+++
### F.I.R.S.T.
**Independent** Tests should not depend on each other. One test should not set up the conditions for the next test.
You should be able to run each test independently and run the tests in any order you like. When tests depend on each other,
then the first one to fail causes a cascade of downstream failures, making diagnosis difficult and hiding downstream defects.
+++
### F.I.R.S.T.
**Repeatable** Tests should be repeatable in any environment. You should be able to run the tests in the production environment,
in the QA environment, and on your laptop while riding home on the train without a network. If your tests aren't repeatable in 
any environment, then you'll always have an excuse for why they fail. You'll also find yourself unable to run tests when the 
environment isn't available.
+++
### F.I.R.S.T.
**Self-Validating** The tests should have a boolean output. Either they pass or fail. You should not have to read through 
a log file to tell whether the tests pass. You should not have to manually compare two different text files to see whether the tests pass.
If the tests aren't self-validating, then failure can become subjective and running the tests can require a long manual evaluation. 
+++
### F.I.R.S.T.
**Timely** The tests need to be written in a timely fashion. Unit tests should be written _just before_ the production code that makes 
them pass. If you write tests after the production code, then you may find the production code to be hard to test.
You may decide that some production code is to hard to test. You may not design the production code to be testable. 
---
### Conclusion
@quote[If you let your tests rot, then your code will rot too. Keep your tests clean.]
---
### Discussion
