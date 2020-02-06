# spring_boot_demo
## 1. Spring boot starter project (1_springboot_hello)
   
   1. Create starter project using STS or https://start.spring.io/ with below starters
      - Spring Web
   2. Create Rest Endpoint with Hello message
      http://localhost:8080/hello
      
      @RestController <br/>
      public class HelloController { <br/>
         @GetMapping("/hello") <br/>
         public String message(){ <br/>
         return "Hello"; <br/>
         } <br/>
      }  
      
   3. Run the application as Spring boot application and check the rest endpoint
      - it should return hello message
## 2. Spring boot DB connection - CRUD (2_springboot_crud)
  1. Create Project with following starters<br/>
     - DATA JPA<br/>
     - H2<br/>
     - LOMBOK<br/>
     - Spring Web<br/>
     
  2. Add schema.sql file in resource folder <br/>
      create table product(id int PRIMARY KEY,name varchar(20),description varchar(100),price decimal(8,3));
      
  3. Configure H2 Database in application.properties <br/>
      #H2 Database <br/>
      spring.datasource.url=jdbc:h2:file:C:/projects/MavenTraining/restful-web-services/restful-web-services <br/>
      spring.datasource.driverClassName=org.h2.Driver <br/>
      spring.datasource.username=sa <br/>
      spring.datasource.password= <br/>
      spring.jpa.database-platform=org.hibernate.dialect.H2Dialect <br/>
      spring.jpa.show-sql=true <br/>
      spring.h2.console.enabled=true <br/>
      
      After config , start application and try access H2 console(port will change based on your configuration)
      http://localhost:8080/h2-console use spring.datasource.url as jdbc url in console
      
  4. Create Product Entity <br/>
      *define all colums as properties <br/>
      *annotate primary column with @ID <br/>
      
  5. Create ProductRepo interfacce by extending CrudRepository interface <br/>
     CrudRepository provided basic crud functionalities and internally extends Repository
     
  6. Service Layer or Controller Layer <br/>
     Auto wire ProductRepo 
     implement Create,Read,update,delete by calling methods in ProductRepo <br/>
     *Technically ProductRepo does not contain any mathod, CRUD methods are extended from CRUD REPO
     
    @RestController
    @RequestMapping("/product")
    public class ProductController {

      @Autowired
      ProductRepo productRepo;

      //CREATE
      @PostMapping
      public Product save(@RequestBody Product product){
        return productRepo.save(product);
      }

      // READ START
      @DeleteMapping("/{id}")
      public void delete(@PathVariable(name="id") Integer id){
         productRepo.deleteById(id);
      }

      @GetMapping("/all")
      public Iterable<Product> getProductList(){
        return productRepo.findAll();
      }
      // READ END

      // UPDATE
      @PutMapping
      public Product upate(@RequestBody Product product){
        return productRepo.save(product);
      }

      // DELETE
      @GetMapping("/{id}")
      public Product getProduct(@PathVariable(name="id") Integer id){
        return  productRepo.findById(id).get();
      }
    }
     input to test Create
     {
        "id": 100, //ID AUTO GENERATED IN OUR SCENARIO DONT NEED TO GIVE
        "name": "One Plus",
        "description": "Never Settle",
        "price": 30000.0
     }
## 3. Actuator (3_springboot_actuator)   
### 3.1 Create project with Actuator
   1. Create starter project using STS or https://start.spring.io/ with below starters
      - Spring Web
      - Actuator
      
   2. Create Rest Endpoint with Hello message
      http://localhost:8080/hello
      
      @RestController <br/>
      public class HelloController { <br/>
         @GetMapping("/hello") <br/>
         public String message(){ <br/>
         return "Hello"; <br/>
         } <br/>
      }  
      
   3. Run the application as Spring boot application and check the actuator endpoint
      - http://localhost:8080/actuator
      - It will display helth and info endpoint
      - If we need management endpoint we have to enable in property file
      
### 3.2 Info Endpoint Details
   By default info endpoint will not have any data.We can add static info(info and description), environment info(java version) through application properties (https://www.baeldung.com/spring-boot-info-actuator-custom)
   
   1. Config satic info <br/>
      info.app.name= Spring Boot Actuator <br/>
      info.app.description=This project is to simulate Acuator  <br/>
      info.app.version=1.0.0 <br/>      
      
   2. Config Env info <br/>
      info.java-vendor = ${java.specification.vendor} <br/>
      
   3. No check the actuator info endpoint above configured info will be displayed. <br/>
      http://localhost:8080/actuator/info       <br/>
      
   4. We can config dynamic info also refer below URL for more <br/>
      https://www.baeldung.com/spring-boot-info-actuator-custom
      
  ### 3.3 Enable Management Endpoints
  In order to access the actuator endpoints using HTTP, we need to both **enable** and **expose** them. By default, all endpoints but /shutdown are enabled.  Only the **/health and /info** endpoints are exposed by default.<br/>
  
   1. Expose all endpoints<br/>
      management.endpoints.web.exposure.include=*<br/>
      
   2. Enable specific endpoints<br/>
      management.endpoints.web.exposure.include=info,env,health
      
   3. Enable other than specific urls  <br/>
      **to achive this we have to expose all the endpoints using inclue=star in sympol and specifi execlusion urls in management.endpoints.web.exposure.exclude** <br/>
      management.endpoints.web.exposure.include=*<br/>
      management.endpoints.web.exposure.exclude=loggers<br/>
      refere https://www.baeldung.com/spring-boot-actuators
   
## 4. Exception Handling(4_springboot_exception_handling)
## <a name="2. Spring boot DB connection - CRUD (2_springboot_crud)">2. Spring boot DB connection - CRUD (2_springboot_crud)</a>
