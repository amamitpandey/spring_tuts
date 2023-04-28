# Uses of Interface
## Refrence 
* interface use in java
https://www.tutorialspoint.com/how-abstraction-is-achieved-using-interfaces-in-java
* interface use in Spring boot
https://stackoverflow.com/questions/12899372/spring-why-do-we-autowire-the-interface-and-not-the-implemented-class#:~:text=You%20autowire%20the%20interface%20so,the%20interface%2C%20not%20the%20class.&text=I%20think%20making%20an%20interface,accepted%20in%20the%20java%20world.

## code sample
```
public interface ExternalDependencyInterface {
    List<String> exUserData(String user);
}
```

```
import org.springframework.stereotype.Component;

import java.util.ArrayList;
import java.util.List;

@Component
public class ExternalDependencyInterfaceImplStub implements ExternalDependencyInterface{
    @Override
    public List<String> exUserData(String user) {
        List<String> list = new ArrayList<>();
        list.add("Amit");
        list.add("Pandey");
        list.add(user);
        return list;
    }
}
```

```
package com.example.JunitTest;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.util.ArrayList;
import java.util.List;

@Component
public class UserService {

    // using @autowired pull only interface not impl class to achieve 100% abstraction
    @Autowired
    ExternalDependencyInterface externalDependency;

    List<String> showUseData(){
        List<String> str1 = externalDependency.exUserData("dummy");
        System.out.println("str1 "+str1);
        return str1;
    }


}
```
```
package com.example.JunitTest;
import java.util.List;

// implement interface without @autowired with normal java
public class UserService2 {

    // Using interface by using one impl class
    ExternalDependencyInterface externalDependency = new ExternalDependencyInterfaceImplStub();

    List<String> showUseData(){
        List<String> str1 = externalDependency.exUserData("dummy");
        System.out.println("str1 "+str1);
        return str1;
    }


}
```

```
package com.example.JunitTest;

import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

import static org.assertj.core.api.AssertionsForClassTypes.assertThat;

@SpringBootTest
class UserServiceTest {

    @Autowired
    UserService userService;


    @Test
    public void showUseData(){
        List<String> str2 = userService.showUseData();
        System.out.println("showUseData ()"+ str2);
        Assertions.assertEquals(str2.size(), 3);
        assertThat(true);
    }

    UserService2 userService2 = new UserService2();
    @Test
    public void showUseData2(){
        List<String> str2 = userService2.showUseData();
        System.out.println("showUseData ()"+ str2);
        Assertions.assertEquals(str2.size(), 3);
        assertThat(true);
    }



}
```
