# Validate name and DOB

## for 2.3+, we need to add in pom.xml
```

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-validation</artifactId>
		</dependency>
    
```   
## in controller 
```

    @PostMapping("/saveUser")
    public ResponseEntity<Object> saveUser(@Valid @RequestBody userModel arrayNew) {
        // user model class, creating multi data type JSON response
        userModel addedUser = repo.saveUserData(arrayNew);

        // using this we return path in header
        URI uriLocation = ServletUriComponentsBuilder
                .fromCurrentRequest()
                .path("/{id}")
                .buildAndExpand(addedUser.getId())
                .toUri();

        HttpHeaders responseHeaders = new HttpHeaders();
        responseHeaders.setLocation(uriLocation);
        responseHeaders.set("MyResponseHeader", "MyValue");
        // using model based class return multi type JSON
        ResponseModel responseBody = new ResponseModel("success", "Registered successfully", addedUser);

        return new ResponseEntity(responseBody, responseHeaders, HttpStatus.CREATED);
    }
``` 
## create a class
```
 @ResponseStatus(HttpStatus.NOT_FOUND)
public class UserNotFoundException extends RuntimeException {
    public UserNotFoundException(String s) {
        super(s);
    }

}
```
## create model

```

public class userModel {


    public int id;
    @Size(min = 2, message = "Name should be atleast two character")
    private String name;
    @Past(message = "DOB must be past date")
    private Date dob;

    public userModel(int id, String name, Date dob) {
        this.id = id;
        this.name = name;
        this.dob = dob;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Date getDob() {
        return dob;
    }

    public void setDob(Date dob) {
        this.dob = dob;
    }

    @Override
    public String toString() {
        return "userRepo{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", dob=" + dob +
                '}';
    }
}
```

## in customizeHandler
```
@RestController
@ControllerAdvice
public class customizeExceptionHandler extends ResponseEntityExceptionHandler {

    @Override
    protected ResponseEntity<Object> handleMethodArgumentNotValid(MethodArgumentNotValidException ex, HttpHeaders headers, HttpStatus status, WebRequest request) {
        exceptionModel excectionRes = new exceptionModel("fail", new Date(), ex.getMessage(), request.getDescription(false));
        return new ResponseEntity(excectionRes, HttpStatus.BAD_REQUEST);
    }
}

```

