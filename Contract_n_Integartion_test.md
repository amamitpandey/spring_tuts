# Contract Test
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
