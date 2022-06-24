---
title: "Spring boot graphql"
path: post4
slug: getting-started-with-gridsome-and-bleda
description: "Spring boot에서 graphql 사용하기"
date: 2022-06-21 21:30:00
author: hyeon namkung
tags:
    - Spring boot
    - graphql
cover: ""
fullscreen: true
---

## 🍀**Spring boot graphql**
Spring에서 **공식적으로 Graphql**를 지원하기 시작했고 기존에 사용했던 [graphql-java-spring](https://github.com/graphql-java) 방법과 조금은 달라졌습니다.(더 편하게).  
새로 만들고 있는 개인프로젝트에 적용할겸 멀티 모듈 기반으로 보일러 플레이트를 만들어보았습니다.

- [spring-graphql](https://spring.io/projects/spring-graphql)

### 🛠 기술 스택
```markdown
- Kotlin
- Spring boot
- Graphql
```

### 👨🏻‍💻 Spring boot graphql example
[Github](https://github.com/namgunghyeon/spring-boot-graphql-example)

- main build.gradle.kts
```kotlin
import org.jetbrains.kotlin.gradle.tasks.KotlinCompile

plugins {
	id("org.springframework.boot") version "2.7.0"
	id("io.spring.dependency-management") version "1.0.11.RELEASE"

	kotlin("jvm") version "1.6.21"
	kotlin("plugin.spring") version "1.6.21"
}

allprojects {
	apply {
		plugin("kotlin")
		plugin("org.jetbrains.kotlin.jvm")
		plugin("org.springframework.boot")
		plugin("io.spring.dependency-management")
		plugin("org.jetbrains.kotlin.plugin.spring") // allOpen 2
	}

	group = "com.graphql"
	version = "0.0.1-SNAPSHOT"
	java.sourceCompatibility = JavaVersion.VERSION_17

	repositories {
		mavenCentral()
		maven { url = uri("https://repo.spring.io/milestone") }
		maven { url = uri("https://repo.spring.io/snapshot") }
	}

	tasks.withType<KotlinCompile> {
		kotlinOptions {
			freeCompilerArgs = listOf("-Xjsr305=strict")
			jvmTarget = "17"
		}
	}

	tasks.withType<Test> {
		useJUnitPlatform()
	}
}

subprojects {

}

dependencies {
}
```

- 서브 모듈 build.gradle.kts
```kotlin
plugins {
    id("org.jetbrains.kotlin.plugin.jpa") version "1.6.21"
}

dependencies {
    implementation(project(":domain_modules:rds-domain"))

    implementation("org.springframework.boot:spring-boot-starter-data-jpa")
    implementation("org.springframework.boot:spring-boot-starter-web")
    implementation("org.springframework.boot:spring-boot-starter-graphql")
    implementation("com.fasterxml.jackson.module:jackson-module-kotlin")
    implementation("org.springframework.boot:spring-boot-starter-security")
    implementation("org.jetbrains.kotlin:kotlin-reflect")
    implementation("org.jetbrains.kotlin:kotlin-stdlib-jdk8")
    implementation("com.auth0:java-jwt:3.19.2")

    runtimeOnly("com.h2database:h2")

    testImplementation("org.springframework.boot:spring-boot-starter-test")
    testImplementation("org.springframework:spring-webflux")
    testImplementation("org.springframework.graphql:spring-graphql-test")
    testImplementation("org.springframework.security:spring-security-test")
}

```

- query.graphql
```graphql
type Query {
  me: AccountDto
}

type Mutation {
    createAccount(email: String!, password: String!): AccountDto
    login(email: String!, password: String!): String
}

type AccountDto {
  id: ID
  token: String
  name: String
}
```

#### @MutationMapping, @QueryMapping
`GraphQLQueryResolver`를 implements하는 방식이 아닌 **@MutationMapping**, **@QueryMapping** 어노테이션을 사용해 @PostMapping, @GetMapping 형태로 사용할 수 있도록 되어 있고
파라미터를 받는 부분에는 `@Argument("parameter name")`를 사용하도록 되어 있습니다.
```java
    @QueryMapping
    @PreAuthorize("isAuthenticated()")
    fun me(): AccountDto {
        val account = accountService.getCurrentUser()
        return AccountDto(account.id!!, account.name, account.email)
    }

    @MutationMapping
    fun createAccount(
        @Argument("email") email: String,
        @Argument("password") password: String
    ): AccountDto {
        val account = accountService.createAccount(email, password)
        return AccountDto(account.id!!, account.name, account.email)
    }
```

### E2E Test
[Testing](https://docs.spring.io/spring-graphql/docs/current/reference/html/#testing), [Example](https://github.com/spring-projects/spring-graphql/blob/main/samples/webmvc-http/src/test/java/io/spring/sample/graphql/project/ProjectControllerTests.java)  
E2E 테스트를 위해 공식 문서와 Gitbub에 있는 예제 코드를 찾아 시도해봤지만 잘 동작하지 않아 MockMvc에 query를 넣어 요청하도록 아래와 같이 처리했습니다.
```java
@AutoConfigureMockMvc
@SpringBootTest
class AccountTests @Autowired constructor(
    private val mockMvc: MockMvc
) {
    @Test
    fun test() {
        val resString = mockMvc.post("/graphql") {
            contentType = MediaType.APPLICATION_JSON
            content = "{\"query\":\"mutation {\\n    createAccount(email: \\\"emai1l\\\", password: \\\"password\\\") {\\n        name,\\n        id,\\n        token\\n    }\\n}\",\"variables\":{}}"
        }.asyncDispatch().andDo { print() }.andExpect { status { isOk() } }.andReturn().response.contentAsString
    }
}
```
