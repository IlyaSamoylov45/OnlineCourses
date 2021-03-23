Object Relational Impedance Mismatch
  - Java is an object oriented programming language. In Java, all data is stored in objects.
  - Typically, Relational databases are used to store data. Relational databases store data in tables.
  - The way we design objects is different from the way the relational databases are designed. This results in an impedance mismatch.
  - Object Oriented programming consists of concepts like encapsulation, inheritance, interfaces and polymorphism
  - Relational databases are made up of Tables with concepts like normalization.
  - Examples of Object Relational Impedance Mismatch
    -  mismatches in column names and parameters.
    - Relationships between objects are expressed in a different way compared with relationship between tables.
    - Some times multiple classes are mapped to a single table and vice-versa

JDBC
  - JDBC stands for Java Database Connectivity
  - It used concepts like Statement, PreparedStatement and ResultSet
  - In the example below, the query used is Update todo set user=?, desc=?, target_date=?, is_done=? where id=?
  - The values needed to execute the query are set into the query using different set methods on the PreparedStatement
  - Results from the query are populated into the ResultSet. We had to write code to liquidate the ResultSet into objects.

Spring JDBC
  - Spring JDBC provides a layer on top of JDBC
  - It used concepts like JDBCTemplate
  - Typically needs lesser number of lines compared to JDBC as following are simplified
    - mapping parameters to queries
    - liquidating resultsets to beans

myBatis
  - MyBatis removes the need for manually writing code to set parameters and retrieve results. It provides simple XML or Annotation based configuration to map Java POJOs to database.
  - We compare the approaches used to write queries below:
    - JDBC or Spring JDBC - Update todo set user=?, desc=?, target_date=?, is_done=? where id=?
    - myBatis - Update todo set user=#{user}, desc=#{desc}, target_date=#{targetDate}, is_done=#{isDone} where id=#{id}

JPA
  - @Entity
    - Represents the object as a entity.
    - Something that will be persisted to a database.
  - @Inheritance(strategy = InheritanceType.SINGLE_TABLE)
    - Objects that extend can be stored in a single table!
    - Inheritance mapping
    - In other words all subclasses of a parent class are stored in a single table.
  - @DiscriminatorColumn(name = "EMPLOYEE_TYPE")
    - The column that distinguished the subclasses that are stored in a single table.
    - Used with InheritanceType.
  - @Table(name = "Task")
    - Helps map your object to your table.
  - @Column(name = "description")
    - map the parameter to the table column.
  - @ManyToMany
    - Establish the relationships between multiple entities.
  - @Id
    - Primary key
  - @GeneratedValue
    - Auto generate id.
    - It seems that Hibernate/JPA isn't able to automatically create a value for your non-id-properties. The @GeneratedValue annotation is only used in conjunction with @Id to create auto-numbers. The @GeneratedValue annotation just tells Hibernate that the database is generating this value itself.
  - @EmbeddedId
    - For composite keys.
    - The composite key must implement Serializeable.
  - To persist the data we need an entityManager.
  - EntityManager
    - Interface used to interact with the persistence context.
  - entityManager.persist()
    - Make user entity instance managed and persistent i.e. saved to database.
  - entityManager.createNamedQuery
    - Creates an instance of TypedQuery for executing a Java Persistence query language named query. The second parameter indicates the type of result.
  - @Repository
    - Spring Annotation to indicate that this component handles storing data to a data store.
  - @Transactional
    - Spring annotation used to simplify transaction management.
    - So that we dont have to do transaction management in each method.
    - Says that each method would be involved in a transaction.
  - @PersistenceContext
    - A persistence context handles a set of entities which hold data to be persisted in some persistence store (e.g. a database). In particular, the context is aware of the different states an entity can have (e.g. managed, detached) in relation to both the context and the underlying persistence store.
  - EntityManager is an interface to the persistence context.
  - Only things in the persistence context are tracked by the entity manager.
  - Add @PersistenceContext over EntityManager.
  - We can add a command line runner at the start of a spring boot application.

Command Line Runner
  - CommandLineRunner interface is used to indicate that this bean has to be run as soon as the Spring application context is initialized.

Spring Data JPA
  - Spring Data aims to provide a consistent model for accessing data from different kinds of data stores.
  - As far as JPA is concerned there are two Spring Data modules that you would need to know
    - Spring Data Commons - Defines the common concepts for all Spring Data Modules.
    - Spring Data JPA - Provides easy integration with JPA repositories.
  - JPARepository:
  ```java
  package com.example.h2.user;

  import org.springframework.data.jpa.repository.JpaRepository;

  public interface UserRepository extends JpaRepository<User, Long> {
  }
  ```
  - CrudRepository
  ```java
  public interface CrudRepository<T, ID extends Serializable>
    extends Repository<T, ID> {

    <S extends T> S save(S entity);

    T findOne(ID primaryKey);       

    Iterable<T> findAll();          

    Long count();                   

    void delete(T entity);          

    boolean exists(ID primaryKey);  

    // … more functionality omitted.
  }
  ```
  - JpaRepository vs CrudRepository
    - JpaRepository extends PagingAndSortingRepository which in turn extends CrudRepository.
    - Their main functions are:
      - CrudRepository mainly provides CRUD functions.
      - PagingAndSortingRepository provides methods to do pagination and sorting records.
      - JpaRepository provides some JPA-related methods such as flushing the persistence context and deleting records in a batch.
    - Because of the inheritance mentioned above, JpaRepository will have all the functions of CrudRepository and PagingAndSortingRepository. So if you don't need the repository to have the functions provided by JpaRepository and PagingAndSortingRepository , use CrudRepository.
  - CrudRepository
    - save(…) – save an Iterable of entities. Here, we can pass multiple objects to save them in a batch
    - findOne(…) – get a single entity based on passed primary key value
    - findAll() – get an Iterable of all available entities in database
    - count() – return the count of total entities in a table
    - delete(…) – delete an entity based on the passed object
    - exists(…) – verify if an entity exists based on the passed primary key value
  - PagingAndSortingRepository
    - This interface provides a method findAll(Pageable pageable), which is the key to implementing Pagination.
    - When using Pageable, we create a Pageable object with certain properties and we've to specify at least:
      - Page size
      - Current page number
      - Sorting
    - So, let's assume that we want to show the first page of a result set sorted by lastName, ascending, having no more than five records each. This is how we can achieve this using a PageRequest and a Sort definition:

    ```java
    public interface PagingAndSortingRepository<T, ID extends Serializable>
      extends CrudRepository<T, ID> {

        Iterable<T> findAll(Sort sort);

        Page<T> findAll(Pageable pageable);
    }

    Sort sort = new Sort(new Sort.Order(Direction.ASC, "lastName"));
    Pageable pageable = new PageRequest(0, 5, sort);
    ```
      - Passing the pageable object to the Spring data query will return the results in question (the first parameter of PageRequest is zero-based).

JpaRepository
  - findAll() – get a List of all available entities in database
  - findAll(…) – get a List of all available entities and sort them using the provided condition
  - save(…) – save an Iterable of entities. Here, we can pass multiple objects to save them in a batch
  - flush() – flush all pending task to the database
  - saveAndFlush(…) – save the entity and flush changes immediately
  - deleteInBatch(…) – delete an Iterable of entities. Here, we can pass multiple objects to delete them in a batch

 Downsides of Spring Data Repositories
  - we couple our code to the library and to its specific abstractions, such as `Page` or `Pageable`; that's of course not unique to this library – but we do have to be careful not to expose these internal implementation details
  - by extending e.g. CrudRepository, we expose a complete set of persistence method at once. This is probably fine in most circumstances as well but we might run into situations where we'd like to gain more fine-grained control over the methods exposed, e.g. to create a ReadOnlyRepository that doesn't include the save(…) and delete(…) methods of CrudRepository
