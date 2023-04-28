# Contract and Integration Testing
```
package com.example.demo;


import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import static java.util.Objects.isNull;
import static org.springframework.http.ResponseEntity.notFound;
import static org.springframework.http.ResponseEntity.ok;

@RestController
public class OrderController {
    @Autowired
    private OrderService orderService;


    @GetMapping(value = "/orders/{orderId}", produces = "application/json")
    public ResponseEntity<Order> getOrder(@PathVariable("orderId") String orderId) {
        Order order = orderService.getOrder(orderId);
        if (isNull(order)) {
            return notFound().build();
        }
        return ok(order);
    }
}
```

```
package com.example.demo;
import org.springframework.stereotype.Service;
import java.util.Map;

@Service
public class OrderService {

        static Map<String, Order> orders = Map.of(
                "1", new Order("1", "Sony TV", 500.00, 1),
                "2", new Order("2", "Samsung TV", 480.00, 1),
                "3", new Order("3", "Washing Machine", 1500.0, 1)
        );

        public Order getOrder(String orderId) {
            return orders.get(orderId);
        }

}
```

```
package com.example.demo;

public class Order {


    private String orderId;
    private String itemName;
    private double price;
    private int units;

    public Order(String s, String itemName, double v, int i) {
        this.orderId = s;
        this.itemName = itemName;
        this.price = v;
        this.units = i;
    }

    public String getOrderId() {
        return orderId;
    }

    public void setOrderId(String orderId) {
        this.orderId = orderId;
    }

    public String getItemName() {
        return itemName;
    }

    public void setItemName(String itemName) {
        this.itemName = itemName;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    public int getUnits() {
        return units;
    }

    public void setUnits(int units) {
        this.units = units;
    }


}
```
## In build.gradle 
```
buildscript {
	repositories {
		mavenCentral()
	}
	dependencies {
		classpath 'org.springframework.cloud:spring-cloud-contract-gradle-plugin:3.1.5'
	}
}

plugins {
	id 'java'
	id 'org.springframework.boot' version '2.7.5'
	id 'io.spring.dependency-management' version '1.0.15.RELEASE'
}

apply plugin: 'spring-cloud-contract'

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '11'

repositories {
	mavenCentral()
}

ext {
	set('springCloudVersion', "2021.0.5")
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.projectlombok:lombok:1.18.18'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	testImplementation 'org.springframework.cloud:spring-cloud-starter-contract-verifier'
}

dependencyManagement {
	imports {
		mavenBom "org.springframework.cloud:spring-cloud-dependencies:${springCloudVersion}"
	}
}

contracts {
	testFramework = "JUNIT5"
	baseClassForTests = 'com.example.demo.ContractBaseClass'
}

// needed to generate report
tasks.named('contractTest') {
	useJUnitPlatform()
}



tasks.named('test') {
	useJUnitPlatform()
}

```
## in test package folder
### for contract test
```
package com.example.demo;

import io.restassured.module.mockmvc.RestAssuredMockMvc;
import org.junit.jupiter.api.BeforeEach;
import org.mockito.Mockito;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.mock.mockito.MockBean;

// this class needed for call application Context and add config
// even we need to add some integration and initilize using RestAssured
@SpringBootTest
public abstract class ContractBaseClass {
    @Autowired
    OrderController orderController;

    @MockBean
    OrderService orderService;

    @BeforeEach
    public void setup() {
        RestAssuredMockMvc.standaloneSetup(orderController);

        Mockito.when(orderService.getOrder("1"))
                .thenReturn(OrderService.orders.get("1"));
    }
}
```
### for integartion test
```
package com.example.demo;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.skyscreamer.jsonassert.JSONAssert;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.ActiveProfiles;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.MvcResult;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;

import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

// this class needed for call whole application Context and add config
@SpringBootTest(
        webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT,
        classes = DemoApplication.class
)
@AutoConfigureMockMvc
@ActiveProfiles("test")
public  class IntegrationBaseClass {
//    @Autowired
//    OrderController orderController;
    @Autowired
    MockMvc mockMvc;

//    @MockBean
//    OrderService orderService;

    @Test
    @DisplayName("IntegrationTestForOrder test")
    public void IntegrationTestForOrder() {
        try {
            MvcResult result = mockMvc.perform(MockMvcRequestBuilders.get("/orders/1")
                    )
                    .andExpect(status().isOk())
                    .andReturn();
            String res = result.getResponse().getContentAsString();
            String ExpectedRes = "{\"orderId\":\"1\",\"itemName\":\"Sony TV\",\"price\":500.0,\"units\":1}";
            JSONAssert.assertEquals(res,ExpectedRes,true);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```
## in test/resources/contracts dir for integartion test
### in order_for_client.groovy
```
package contracts

import org.springframework.cloud.contract.spec.Contract

Contract.make {

    description "should return order by id=1"

    request {
        url "/orders/1"
        method GET()
    }

    response {
        status 200
        headers {
            contentType applicationJson()
        }
        body(
                "orderId": 1,
                "itemName": "Sony TV",
                "price": 500.0,
                "units": 1
        )
    }
}
```
