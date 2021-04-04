# go to project dir
```
gradle clean build

or 

gradle build

```

# for excute jar file in project dir
$ java -jar build/libs/customItemWriter-0.0.1-am.jar

# in build.gradle

```
plugins {
	id 'org.springframework.boot' version '2.4.4'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
}

group = 'com.example'
version = '0.0.1-am'
sourceCompatibility = '1.8'

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-batch'
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	compileOnly 'org.projectlombok:lombok'
	developmentOnly 'org.springframework.boot:spring-boot-devtools'
	runtimeOnly 'com.h2database:h2'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	testImplementation 'org.springframework.batch:spring-batch-test'
}

test {
	useJUnitPlatform()
}


// custom code by amit
jar {	
	// get version form top of same file
	// $baseName - get name from setting-gradle
	archiveName = "$baseName-$version.$extension"
	// if if have multiple main class 
        //manifest {
             // attributes 'Main-Class': 'com.example.customItemWriter.CustomItemWriterApplication'
    //}
}

```

# get your 


