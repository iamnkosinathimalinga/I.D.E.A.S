DATE: 2024-07-16
TIME: 10:40
STATE: #baby 
TAGS: [[C]] [[Software-Development]] [[Work 1]] [[TDD]] [[Learning]]
# NOTE 

###### Foundational Knowledge: 
	variables are used to store information, i.e: (int x = 5) they are used as references to memory in a program.
	a data type tells our program (or helps it determine) between numbers, characters and string, etc...
	functions are blocks of sequentially executed blocks of code to complete a specific task.
	conditionals are requirements that should meet a criteria which is specified and satisfied which then yield a response (commonly "yes or no").
	loops: are functions that automate repeated tasks.


###### Foundational Knowledge Test Driven Development: Moq < Excerpt for a Unit Test Listed Below >
	Moq is a framework which helps us do unit testing by mocking or creating instances of interfaces which we can then setup to return behaviors which we expect to happen when our code is run.
###### Foundational Knowledge T.D.D: 
> [!NOTE]
> 		i.e of Moq Syntax: s
> 			`string somethingRandom = "testString";
> 			`var mockClass = new Mock<IClassInterface>();
> 			`mockClass.Setup(mc => mc.DoSome(somethingRandom));`
> 			`var myClass = new MyClass(mockClass.Object);`
> 			`myClass.myMethod(somethingRandom);`
> 			`mockClass.Verify(mc.DoSome(), Times.Once;`

```
using Moq;

// Assert
mockClass.Verify(args, amount of time(optional));

This method checks that a method call from class was called with the correct parameters.
```

###### Foundational Knowledge Unit Testing: XUnit
	A framework for unit testing (unit tests are used to test blocks of code{i.e. functions or methods}) XUnit Keywords Include: 
		`[Fact], [InlineData], [Theory], And Assert`
	
```
using XUnit;

How to test if an Exception was thrown 
	`Assert.Throws<Exception>(l => sut.GetGamePart(1));`
```

###### Foundational Knowledge [[CSharp - Target]] Basics: 

A **Class** is a data structure with data members (fields), function members (functions) & nested types (i.e. A Class inside another Class)
An **Object** is an entity that's configured according to the structure of it's Class. 