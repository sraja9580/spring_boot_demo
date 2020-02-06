
# Exception Handling(4_springboot_exception_handling)
      create project by reffering section 2. Spring boot DB connection - CRUD
      resource class will have following get resource
      @GetMapping("/{id}")
      public Product getProduct(@PathVariable(name="id") Long productId){
         Product product = null;
         Optional<Product> productOptnl = productRepo.findById(productId);
         if(productOptnl.isPresent()){
            product = productOptnl.get();
         }
         return product;
      } 
      
      @RestControllerAdvice
      public class CustomExceptionHandler extends ResponseEntityExceptionHandler {

         @ExceptionHandler(Exception.class)
         public final ResponseEntity<Object> handleAllException(Exception ex, WebRequest request) throws Exception {
            ProductExceptionResponse exResponse = new ProductExceptionResponse(new Date(), ex.getMessage(), request.getDescription(false));
            return new ResponseEntity(exResponse,HttpStatus.INTERNAL_SERVER_ERROR);
         }

         @Override
         public ResponseEntity<Object> handleMissingServletRequestParameter(MissingServletRequestParameterException ex,
               HttpHeaders headers, HttpStatus status, WebRequest request) {		 
            ProductExceptionResponse exResponse = new ProductExceptionResponse(new Date(), ex.getMessage(), request.getDescription(false));
            return new ResponseEntity(exResponse,HttpStatus.BAD_REQUEST);
         }
      }
## 4.1 Access UnAvaiable resource
   1. Access http://localhost:8080/product/135 in this we dont have product id 135 in table.
   2. when you access you will get 200 response code with empty body (image need to be added)
   3. **we should have got 404 resource not found**
   
## 4.2 Throw Custom Exception when product not found
   1. Create new ProductNotFoundException class
      //**@ResponseStatus(HttpStatus.NOT_FOUND)** //this anotation helps us to retrun specific response code<br/>
      public class ProductNotFoundException extends RuntimeException {<br/>
         public  ProductNotFoundException(String message){<br/>
            super(message);<br/>
         }<br/>
      }<br/>
   2. Creare new method with different endpoint and throud ProductNotFoundException id product not available in table
   
      @GetMapping("/ntfndexp/{id}")<br/>
      public Product getProductWithProductNotFoundException(@PathVariable(name="id") Long productId){<br/>
         Product product = null;<br/>
         Optional<Product> productOptnl = productRepo.findById(productId);<br/>
         if(productOptnl.isPresent()){<br/>
            product = productOptnl.get();<br/>
         }else{<br/>
            throw new ProductNotFoundException("product with id "+productId+" not found in the system");<br/>
         }<br/>
         return product;<br/>
      } <br/>
   
   3. Now try resource that is not available in table with new endpoint
      http://localhost:8080/product/ntfndexp/135<br/>
      U will be getting **500 internal server error** with below error messsage<br/>
         {<br/>
          "timestamp": "2020-02-06T13:10:05.981+0000",<br/>
          "status": 500,<br/>
          "error": "Internal Server Error",<br/>
          "message": "product with id 135 not found in the system",<br/>
          "path": "/product/ntfndexp/135"<br/>
         }<br/>
     it should have returned **404 Not found** you can do this by adding **@ResponseStatus(HttpStatus.NOT_FOUND)** to exception class ProductNotFoundException<br/>
   4. Now try resource that is not available in table with new endpoint after adding ResponseStatus anotation
      you will get same error message with **404 Not found response code**
      
## 4.3 Customize and centralize exception handling with ResponseEntityExceptionHandler and ControllerAdvice
   with the use of ResponseEntityExceptionHandler we can handle exception like badrequest(somt param missed or not correct) in a comman class.
   - **@RestControllerAdvice** is used on class implementing ResponseEntityExceptionHandler to apply on all controller classes
   Create Custom Response Class
      @Getter<br/>
      @Setter<br/>
      @NoArgsConstructor<br/>
      @AllArgsConstructor<br/>
      @ToString<br/>
      public class ProductExceptionResponse {<br/>
         private Date timeStamp;<br/>
         private String message;<br/>
         private String details;<br/>
      }<br/>
   ### 4.3.1 Handle All Exceptions  
   1. Provide implementation for ResponseEntityExceptionHandler to just handle all exceptions<br/>
      **open the class file ResponseEntityExceptionHandler and implement method you need**<br/>
      public class CustomExceptionHandler extends ResponseEntityExceptionHandler {	<br/>
      @ExceptionHandler(Exception.class)<br/>
      public final ResponseEntity<Object> handleAllException(Exception ex, WebRequest request) throws Exception {<br/>
         ProductExceptionResponse exResponse = new ProductExceptionResponse(new Date(), ex.getMessage(), request.getDescription(false));<br/>
         return new ResponseEntity(exResponse,HttpStatus.INTERNAL_SERVER_ERROR);<br/>
      }<br/>
      }<br/>
   
   2. if you check now with all the exceptions are handled by CustomExceptionHandler class
      - ProductNotFoundException
      - Othe exceptions also -> to simulate this create endpoint with method just throws some exception. it will be handled
