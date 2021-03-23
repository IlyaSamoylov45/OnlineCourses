Spring Boot Starter parent
  - Use caution when overriding versions of dependencies managed by Spring Boot Starter Parent.

Spring Boot vs Spring
  - Spring
    - Spring is just a dependency injection framework. Spring focuses on the "plumbing" of enterprise applications so that teams can focus on application-level business logic, without unnecessary ties to specific deployment environments.
    - First half of the 2000 decade! EJBs
    - EJBs were NOT easy to develop.
    - Write a lot of xml and plumbing code to get EJBs running
    - Impossible to Unit Test
    - Alternative - Writing simple JDBC Code involved a lot of plumbing
    - Spring framework started with aim of making Java EE development simpler.
    - Goals
      - Make applications testable. i.e. easier to write unit tests
      - Reduce plumbing code of JDBC and JMS
      - Simple architecture. Minus EJB.
      - Integrates well with other popular frameworks.

  - Applications with Spring Framework
    - Over the next few years, a number of applications were developed with Spring Framework
    - Testable but
    - Lot of configuration (XML and Java)
    - Developing Spring Based application need configuration of a lot of beans!
    - Integration with other frameworks need configuration as well!
    - In the last few years, focus is moving from monolith applications to microservices. We need to be able to start project quickly. Minimum or Zero start up time
    - Framework Setup
    - Deployment - Configurability
    - Logging, Transaction Management
    - Monitoring
    - Web Server Configuration
  - Spring Boot
    - Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can “just run”.
    - We take an opinionated view of the Spring platform and third-party libraries so you can get started with minimum fuss.
    - You want to add Hibernate to your project. You dont worry about configuring a data source and a session factory. I will do if for you!
    - Goals
      - Provide quick start for projects with Spring.
      - Be opinionated but provide options.
      - Provide a range of non-functional features that are common to large classes of projects (e.g. embedded servers, security, metrics, health checks, externalized configuration).
  - What Spring Boot is NOT?
    - It’s not an app or a web server
    - Does not implement any specific framework - for example, JPA or JMS
    - Does not generate code

Annotation
  - @GetMapping
  - @PathVariable
  - @RequestBody

Different Request Methods
  - GET - Retrieve details of a resource
  - POST - Create a new resource
  - PUT - Update an existing resource
  - PATCH - Update part of a resource
  - DELETE - Delete a resource

Content Negotiation
  - Accept:application/xml
  - By default json
  ```xml
  <dependency>
      <groupId>com.fasterxml.jackson.dataformat</groupId>
      <artifactId>jackson-dataformat-xml</artifactId>
  </dependency>
  ```

Actuator
  - Spring Boot Actuator is used to monitor applications in production.

Dynamic configuration
  - Can use arguments in the run configurations in eclipse (command line arguments).
  ```
  --welcome.message="SomethingElse" in Program Arguments
  --spring.config.location=classpath:/default.properties
  ```
  - Things in the arguments are higher priority than the application.properties file.
  - yaml is more readable.

Profiles
  - spring.profile.active=prod
    - In application.properties
    - Can have multiple profiles.
    - application-prod.properties
  - Another way - Dspring.profiles.active=prod
  - Can have @Profile{"prod"} over Beans

Advanced Application Configuration - Type Safe Configuration Properties
  - Better with @Value management:
  ```java
  @Component
  @ConfigurationProperties("basic")
  public class BasicConfiguration {
      private boolean value;
      private String message;
      private int number;

      public boolean isValue() {
          return value;
      }

      public void setValue(boolean value) {
          this.value = value;
      }

      public String getMessage() {
          return message;
      }

      public void setMessage(String message) {
          this.message = message;
      }

      public int getNumber() {
          return number;
      }

      public void setNumber(int number) {
          this.number = number;
      }

  }

  // Other class
  @Autowired
   private BasicConfiguration configuration;

  ```

Embedded DB
  - Embedded DB are typically used in Unit tests.
  - CrudRepository implements basic CRUD methods.

Configuring H2
  - spring.jpa.show-sql=true
  - spring.h2.console.enabled=true

JPA
  - In CrudRepository we can do :
    - findBy{ColumName}
    - List<User> findByName(String name);

@RepositoyRestResource
  - This uses HATEOS links
  - The @RepositoryRestResource annotation is optional and is used to customize the REST endpoint.
  - @RepositoryRestResource(collectionResourceRel = "users", path = "users")
  - @RepositoryRestResource is used to set options on the public Repository interface - it will automatically create endpoints as appropriate based on the type of Repository that is being extended (i.e. CrudRepository/PagingAndSortingRepository/etc).
  - We can add size in url.
  - @Param
    - Exposes parameter as a method as a rest service
    ```java
    @RepositoryRestResource(path="users", collectionResourceRel="users")
    public interface UserRestRepository extends PagingAndSortingRepository<User, Integer>{
      List<User> findByName(@Param String name);
    }
    ```
  - Rest JPA dependency :
    ```xml
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-rest</artifactId>
    </dependency>

    ```
  - Spring JPA Rest is good for prototype but need to be careful when putting in production.

HATEOAS
  - Hypermedia as the Engine of Application State (HATEOAS) is a component of the REST application architecture that distinguishes it from other network application architectures.
  - With HATEOAS, a client interacts with a network application whose application servers provide information dynamically through hypermedia. A REST client needs little to no prior knowledge about how to interact with an application or server beyond a generic understanding of hypermedia.
  - The restrictions imposed by HATEOAS decouple client and server. This enables server functionality to evolve independently.
  - Example Request :
  ```json
  GET /accounts/12345 HTTP/1.1
  Host: bank.example.com
  Accept: application/vnd.acme.account+json
  ...
  ```
  - Example Response :
  ```json
  HTTP/1.1 200 OK
  Content-Type: application/vnd.acme.account+json
  Content-Length: ...

  {
      "account": {
          "account_number": 12345,
          "balance": {
              "currency": "usd",
              "value": 100.00
          },
          "links": {
              "deposit": "/accounts/12345/deposit",
              "withdraw": "/accounts/12345/withdraw",
              "transfer": "/accounts/12345/transfer",
              "close": "/accounts/12345/close"
          }
      }
  }
  ```

Integration Testing
  - @RunWith(SpringRunner.class)
  - @SpringBootTest(classes = Application.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
  - @LocalServerPort
    - private int port;
  - TestRestTemplate restTemplate = new TestRestTemplate;
  ```java
  @RunWith(SpringRunner.class)
  @SpringBootTest(classes = Application.class,
  		webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
  public class SurveyControllerIT {

  	@LocalServerPort
  	private int port;

  	@Test
  	public void testRetrieveSurveyQuestion() {

  		String url = "http://localhost:" + port
  				+ "/surveys/Survey1/questions/Question1";

  		TestRestTemplate restTemplate = new TestRestTemplate();

  		HttpHeaders headers = new HttpHeaders();

  		headers.setAccept(Arrays.asList(MediaType.APPLICATION_JSON));

  		HttpEntity<String> entity = new HttpEntity<String>(null, headers);

  		ResponseEntity<String> response = restTemplate.exchange(url,
  				HttpMethod.GET, entity, String.class);

  		String expected = "{id:Question1,description:Largest Country in the World,correctAnswer:Russia}";

  		JSONAssert.assertEquals(expected, response.getBody(), false);
  	}
  }
  ```
  - TestRestTemplate
    - Convenient alternative of RestTemplate that is suitable for integration tests. They are fault tolerant, and optionally can carry Basic authentication headers. If Apache Http Client 4.3.2 or better is available (recommended) it will be used as the client, and by default configured to ignore cookies and redirects.
    - If you are using the @SpringBootTest annotation with an embedded server, a TestRestTemplate is automatically available and can be @Autowired into your test. If you need customizations (for example to adding additional message converters) use a RestTemplateBuilder @Bean.
    - TestRestTemplate can be used to send http request in our Spring Boot integration tests. Such tests are usually executed with Spring boot run as a local server in a random port @LocalServerPort. So just need to create the request in integration tests and send it like a clients of your servers. TestRestTemplate have all necessary methods to send the request to server with a convenient way similar to RestTemplate.
  - JSONAssert
    - JSONAssert.asserEquals(expected,actual,boolean);
    - boolean is strict or not. All data must match. Order in Json response does not matter.
    - Good for running tests which return JSON.
  - Using Post in Integration tests:
  ```java
  @Test
  public void retrieveSurveyQuestions() throws Exception {
      ResponseEntity<List<Question>> response = template.exchange(
              createUrl("/surveys/Survey1/questions/"), HttpMethod.GET,
              new HttpEntity<String>("DUMMY_DOESNT_MATTER", headers),
              new ParameterizedTypeReference<List<Question>>() {
              });

      Question sampleQuestion = new Question("Question1",
              "Largest Country in the World", "Russia", Arrays.asList(
                      "India", "Russia", "United States", "China"));

      assertTrue(response.getBody().contains(sampleQuestion));
  }

  @Test
  public void createSurveyQuestion() throws Exception {
      Question question = new Question("DOESN'T MATTER", "Smallest Number",
              "1", Arrays.asList("1", "2", "3", "4"));

      ResponseEntity<String> response = template.exchange(
              createUrl("/surveys/Survey1/questions/"), HttpMethod.POST,
              new HttpEntity<Question>(question, headers), String.class);

      assertThat(response.getHeaders().get(HttpHeaders.LOCATION).get(0), containsString("/surveys/Survey1/questions/"));
  }
  ```

Unit Test
  - Controller Test example:
  ```java
  @RunWith(SpringRunner.class)
  @WebMvcTest(value = SurveyController.class)
  public class SurveyControllerTest {

  	@Autowired
  	private MockMvc mockMvc;

  	// Mock @Autowired
  	@MockBean
  	private SurveyService surveyService;

  	@Test
  	public void retrieveDetailsForQuestion() throws Exception {
  		Question mockQuestion = new Question("Question1",
  				"Largest Country in the World", "Russia", Arrays.asList(
  						"India", "Russia", "United States", "China"));

  		Mockito.when(
  				surveyService.retrieveQuestion(Mockito.anyString(), Mockito
  						.anyString())).thenReturn(mockQuestion);

  		RequestBuilder requestBuilder = MockMvcRequestBuilders.get(
  				"/surveys/Survey1/questions/Question1").accept(
  				MediaType.APPLICATION_JSON);

  		MvcResult result = mockMvc.perform(requestBuilder).andReturn();

  		String expected = "{id:Question1,description:Largest Country in the World,correctAnswer:Russia}";

  		JSONAssert.assertEquals(expected, result.getResponse()
  				.getContentAsString(), false);

  		// Assert
  	}

    @Test
    public void createSurveyQuestion() throws Exception {
        Question mockQuestion = new Question("1", "Smallest Number", "1",
        Arrays.asList("1", "2", "3", "4"));

        String questionJson = "{\"description\":\"Smallest Number\",\"correctAnswer\":\"1\",\"options\":[\"1\",\"2\",\"3\",\"4\"]}";

        Mockito.when(
          surveyService.addQuestion(Mockito.anyString(), Mockito
            .any(Question.class))).thenReturn(mockQuestion);

        RequestBuilder requestBuilder = MockMvcRequestBuilders.post(
        "/surveys/Survey1/questions")
        .accept(MediaType.APPLICATION_JSON).content(questionJson)
        .contentType(MediaType.APPLICATION_JSON);

        MvcResult result = mockMvc.perform(requestBuilder).andReturn();

        MockHttpServletResponse response = result.getResponse();

        assertEquals(HttpStatus.CREATED.value(), response.getStatus());

        assertEquals("http://localhost/surveys/Survey1/questions/1", response.getHeader(HttpHeaders.LOCATION));
      }
  }
  ```
  - Mockito’s @Mock vs Spring boot’s @MockBean
    - Both annotations are used to add mock objects which allow to mock a class or an interface and to record and verify behaviors on it. However, we can prefer to use one over another in a certain way.
    - As we write a test that doesn’t need any dependencies from the Spring Boot container, the Mockito‘s @Mock shall be used. It is fast and favors the isolation of the tested component.
    - If the test needs to rely on the Spring Boot container and we want also to add or mock one of the container beans then @MockBean from Spring Boot is preferred way to add mocks.
    - We will generally use @MockBean along with either @WebMvcTest or @WebFluxTest annotations. These annotations are for web test slice and limited to a single controller.
  - Spring Security:
    - @WebMvcTest(value = SurveyController.class, secure = false) is deprecated.
    ```java
    @WithMockUser(username = "user1", password = "secret1")
    @Test
    public void yourTest() throws Exception {...}
    ```
  - Spring Security example :
  ```java
  @RunWith(SpringRunner.class)
  @WebMvcTest(value = SurveyController.class, secure = false)
  public class SurveyControllerTest {

  	@Autowired
  	private MockMvc mockMvc;

  	// Mock @Autowired
  	@MockBean
  	private SurveyService surveyService;

  	@Test
  	public void retrieveDetailsForQuestion() throws Exception {
  		Question mockQuestion = new Question("Question1",
  				"Largest Country in the World", "Russia", Arrays.asList(
  						"India", "Russia", "United States", "China"));

  		Mockito.when(
  				surveyService.retrieveQuestion(Mockito.anyString(), Mockito
  						.anyString())).thenReturn(mockQuestion);

  		RequestBuilder requestBuilder = MockMvcRequestBuilders.get(
  				"/surveys/Survey1/questions/Question1").accept(
  				MediaType.APPLICATION_JSON);

  		MvcResult result = mockMvc.perform(requestBuilder).andReturn();

  		String expected = "{id:Question1,description:Largest Country in the World,correctAnswer:Russia}";

  		JSONAssert.assertEquals(expected, result.getResponse()
  				.getContentAsString(), false);

  		// Assert
  	}

  	@Test
  	public void createSurveyQuestion() throws Exception {
  		Question mockQuestion = new Question("1", "Smallest Number", "1",
  				Arrays.asList("1", "2", "3", "4"));

  		String questionJson = "{\"description\":\"Smallest Number\",\"correctAnswer\":\"1\",\"options\":[\"1\",\"2\",\"3\",\"4\"]}";
  		//surveyService.addQuestion to respond back with mockQuestion
  		Mockito.when(
  				surveyService.addQuestion(Mockito.anyString(), Mockito
  						.any(Question.class))).thenReturn(mockQuestion);

  		//Send question as body to /surveys/Survey1/questions
  		RequestBuilder requestBuilder = MockMvcRequestBuilders.post(
  				"/surveys/Survey1/questions")
  				.accept(MediaType.APPLICATION_JSON).content(questionJson)
  				.contentType(MediaType.APPLICATION_JSON);

  		MvcResult result = mockMvc.perform(requestBuilder).andReturn();

  		MockHttpServletResponse response = result.getResponse();

  		assertEquals(HttpStatus.CREATED.value(), response.getStatus());

  		assertEquals("http://localhost/surveys/Survey1/questions/1", response
  				.getHeader(HttpHeaders.LOCATION));
  	}
  }
  ```
