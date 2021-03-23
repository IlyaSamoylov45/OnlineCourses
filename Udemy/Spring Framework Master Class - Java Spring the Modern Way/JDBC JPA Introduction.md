H2
  - An in memory database.
  - With the latest versions of Spring Boot (2.3+), the H2 database name is randomly generated each time you restart the server.
  - You can find the database name and URL from the console log.
  - application.properties:
    - spring.datasource.url=jdbc:h2:mem:testdb
    - spring.data.jpa.repositories.bootstrap-mode=default
    - spring.h2.console.enabled=true
      - localhost:8080/h2-console
  - Good DB for learning since it's in memory.

JDBC
  - @Repository on repository DAO
  - DAO :
    - The Data Access Object is basically an object or an interface that provides access to an underlying database or any other persistence storage.

  ```java
  @Repository
  public class PersonJdbcDao{

    @Autowired JdbcTemplate jdbcTemplate;

    public List<Person> findAll(){
      return jdbcTemplate.query("select * from person", new BeanPropertyRowMapper<Person>(Person.class)); // default spring mapper.
    }

    public Person findById(int id){
      return jdbcTemplate.queryForObject("select * from person where id=?", new Object[]{id}, new BeanPropertyRowMapper<Person>(Person.class));
    }

    public int deleteById(int id){
      return jdbcTemplate.update("delete from person where id=?", new Object[]{id});
    }

    public int insert(Person p){
      return jdbcTemplate.update("insert into person(id, name, location, birth_date) values(?,?,?,?)", new Object[]{p.getId(), p.getName(), p.getLocation(), p.getBirthDate()});
    }
    public int update(Person p){
      return jdbcTemplate.update("update  person set name = ?, location = ?, birth_date = ? where id = ?", new Object[]{p.getName(), p.getLocation(), p.getBirthDate(), p.getId()});
    }
  }
  ```

  - The Pojo should have a no arg constructor.
  - JDBC in Spring prevents you from making mistakes because you don't have to worry about connections.
  - JDBC in Spring removes a lot of boilerplate code.

Custom JDBC RowMapper
  - BeanPropertyRowMapper<Person>(Person.class);

  ```java
  class PersonRowMapper implements RowMapper<Person>{
    @Override
    public Person mapRow(Result rs, int rowNum) throws SQLException{
      Person person = new Person();
      person.setId(rs.getInt("id"));
      person.setName(rs.getString("name"));
      person.setLocation(rs.getString("location"));
      person.setBirthDate(rs.getTimeStamp("target_date"));
      return person;
    }
  }
  ```
  - Custom RowMapper can be useful sometimes.

JPA
  - JDBC is difficult because you have to write the queries. This can become complex with more tables.
  - JPA maps the entity directly to a table.
  - JPA takes care of the queries.
  - Hibernate is the most popular implementation of JPA.
  - Hibernate came before JPA.
  - @Entity
  - @Table("person")
    - By default the table maps to the name of the class. So table annotation might not be necessary.
  - @Column(name="name")
    - By default the column matches to the property name
  - @Id is necessary and identifies the P.K.
  - @GeneratedValue is used to autogenerate the P.K.
  - MUST HAVE A NO ARG CONSTRUCTOR!
  - @Repository, @Transactional
    - To connect to DB : @PersistenceContext EntityManager entityManager;
      - Entity manager is the interface to the persistence context.
    - find : entityManager.find(Person.class, id);
  - Hibernate auto generates schema for us.
  - application.properties : spring.jpa.show.sql=true
  - update : entityManager.merge(person)
    - For update or insert.
    - merge knows if id is set or not. If it is set then it will update otherwise insert into db.
  - delete : first find person then you can delete.
    - Person person = findById(id);
    - entityManager.remove(person);
  - Sometimes need to write queries with HQL:

    ```java
    public List<Person> findAll(){
        TypedQuery<Person> namedQuery = entityManager.createNamedQuery("find_all_persons", Person.class);
        return namedQuery.getResultList();
    }

    // In the entiry file:
    @NamedQuery(name="find_all_persons", query="select p from Person p")
    public class Person{
      // blah blah
    }
    ```
  - JPA noticed there was a lot of code duplication.
    - Spring came in with predefined repositories.
      - it is an interface :

      ```java
      public interface PersonSpringDataRepository extends JpaRepositry<Person, Integer>{

      }
      ```
      - No insert or update in JpaRepositry only merge => save
