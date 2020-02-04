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
