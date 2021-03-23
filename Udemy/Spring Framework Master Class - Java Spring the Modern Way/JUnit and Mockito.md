Unit Testing
  - For Testing a specific method or a class.

JUnit
  - JUnit are automated tests.
  - JUnit can be run in continuous integration.
  - Separate Unit tests and code.
    - src folder and test folder
  - @Test annotation on test method.
  - Absence of failure is success.
  - assetEquals(expected, result);
  - assertTrue(condition);
  - assertFalse(condition);
  - assertNull();
  - assertNotNull();
  - assertArraysEquals(expected, actual);
  - @Before annotation to run before every tests
  - @After annotation is run after every test.
  - @BeforeClass - must be static, run before the class
  - @AfterClass - must be static, run after the class

Mockito
  - A stub is an implementation of a dependency in a test.
  - Mocks are a solution to stubs
  - MockService mockService = mock(MockService.class)
  - when(mockService.method()).thenReturn(value);
  - @Mock
  - @InjectMocks
  - @RunWith(MockitoJUnitRunner.class) over class
  - mock(List.class) -> can mock lists
  - when(listMock.size()).thenReturn(10).thenReturn(20);
    - Now can call this twice.
  - when(listMock.size(Mockito.anyInt)).thenReturn(10);

Spring Unit testing
  - To load the context use @ContextConfiguration(classes="SpringIn5StepsBasicApplication.class")
  - Need to use @RunWith(SpringRunner.class) as well.
  - ContextConfiguration defines the context and RunWith launches the configuration.
  - Test code must be separate from the application code.
  - We can use XML configuration to test as well.
    - @ContextConfiguration(locations="/applicationContext.xml")
    - If you want to have any test config that is different from the real application context then you can create a new folder in source test folder called resources and have a xml document there.
  - <import resource="classpath:applicationContext.xml"/>
    - To override an existing bean from an application context.
  - When we write a test with mockito, you dont need an application context you can just use @RunWith(MockitoJUnitRunner.class);
  - Create a mock then inject the mock.
    - @Mock
    - @InjectMocks
    - Mockito.when(daoMock.getData()).thenReturn()
  - It is best to avoid using spring as much as possible because Mockito can be 10 times faster!

Mocking Framework definition
  - What is a mocking framework? Mocking frameworks are used to generate replacement objects like Stubs and Mocks. Mocking frameworks complement unit testing frameworks by isolating dependencies but are not substitutes for unit testing frameworks.
  - In object-oriented programming, mock objects are simulated objects that mimic the behavior of real objects in controlled ways, most often as part of a software testing initiative. A programmer typically creates a mock object to test the behavior of some other object, in much the same way that a car designer uses a crash test dummy to simulate the dynamic behavior of a human in vehicle impacts. The technique is also applicable in generic programming.
