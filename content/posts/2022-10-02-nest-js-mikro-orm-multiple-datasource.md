---
title: "Nestjs mikro-orm Multiple Datasource"
path: post6
slug: getting-started-with-gridsome-and-bleda
description: "Nestjs에서 mikro-orm 멀티 데이터 소스 사용하기"
date: 2022-10-02 23:59:00
author: hyeon namkung
tags:
    - Nestjs
    - Mikro-orm
cover: ""
fullscreen: true
---

## ✍🏻
회사에서 NestJS + Mikro-orm를 사용해 프로젝트를 진행중이고, Multiple Datasource를 적용할 일이 생겨 적용하면서 발생했던 내용을 기록하기 위해 작성한 글입니다. 

### 🛠 기술 스택
```markdown
- NestJS
- Mikro-orm
- Mysql
```

## 🎡 **NestJS**
[NestJS](https://nestjs.com/)
> In recent years, thanks to Node.js, JavaScript has become the “lingua franca” of the web for both front and backend applications. This has given rise to awesome projects like Angular, React and Vue, which improve developer productivity and enable the creation of fast, testable, and extensible frontend applications. However, while plenty of superb libraries, helpers, and tools exist for Node (and server-side JavaScript), none of them effectively solve the main problem of - Architecture.
Nest provides an out-of-the-box application architecture which allows developers and teams to create highly testable, scalable, loosely coupled, and easily maintainable applications. The architecture is heavily inspired by Angular.

`Java` + `Spring Boot` 조합 위주로 개발했었는데, 이번 프로젝트부터 `NestJS`를 사용하게되었습니다.

Spring MVC의 개념과 비슷해 NestJS를 이해하는데 어렵지 않았고, Spring Boot에서 개발하는 것 처럼 어렵지 않게 개발할 수 있었습니다.
개발하면서 자유도가 높아 이렇게 동작해도 괜찮나?라는 생각이 몇 번 들었지만, 프레임워크 자체는 잘 만들어진 것 같았습니다.

조금씩 내부 동작 구조를 [NestJS Github](https://github.com/nestjs/nest)를 봐볼 예정입니다.

## 📂 **Mikro-orm**
[mikro-orm](https://mikro-orm.io/)
Fundamentals
- Identity Map
- Entity References
- Collections
- Entity Repository
- Transactions and Concurrency

### Mikro-ORM은 선택하게된 계기
Nest에서 사용할 수 있는 ORM은 크게 `Miko ORM`, `TypeORM`, `Sequelize`, `Prisma` 있었고, JPA와 비슷하고 업데이트가 빠른 `Miko ORM`를 사용하게 되었습니다.
`Miko ORM`를 사용할 때 가장 크게 고민했던 부분은 아직 많이 사용하는 라이브러리는 아니였고, 계속 라이브러리 관리가 될까?하는 의문이 있었습니다.
약 2달간 사용하면서 아직은 크게 불편함을 느끼지 못하고 있고, 업데이트가 정말 빠르게 이뤄지고 있었습니다.

## 🌼 **Multiple DataSource 적용하기** 
### Multiple DataSource가 필요했던 이유
보틍은 하나의 DataSource를 바라보고 사용하는 경우가 많은데, 데이터를 괸리하는 주체가 달라 새로운 DB가 생성되었고, API에서는 두 개의 데이터 소스를 보고 개발을 하게되었습니다.

다행히 Mikro-ORM에서 [Multiple Database Connections](https://mikro-orm.io/docs/usage-with-nestjs#multiple-database-connections) 를 지원해 어렵지 않게 설정할 수 있었습니다.

설정 구조는 모듈에 Database별 이름을 지정하고 `EntityRepository<Entity>`를 사용할 때 지정된 이름을 붙여 주는 방식입니다.
여기서 중요한 부분은 아래 *설정* 처럼 연결할 Database별로 모두 `contextName` 이름을 지정해야 정상적으로 사용할 수 있었습니다.

### MikroOrmModule 설정
```typescript
@Module({
  imports: [
    MikroOrmModule.forRoot({
      contextName: 'db1',
      registerRequestContext: false, // disable automatatic middleware
      ...
    }),
    MikroOrmModule.forRoot({
      contextName: 'db2',
      registerRequestContext: false, // disable automatatic middleware
      ...
    }),
    MikroOrmModule.forMiddleware()
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```


### contextName 이름을 신규로 추가한 ORM 설정에만 적용할 경우
기존에 사용하고 있던 ORM에는 contextName 이름을 추가 하지 않고(***Repository쪽에 'db1' 이름을 다 붙여주는 작업이 필요***)

기존 Repository에는 수정없이 사용하고 신규로 추가되는 Repository를 사용할 때 붙여주기 위해서 였습니다.
```typescript
@Injectable()
export class PhotoService {
  constructor(
    @InjectRepository(NewPhoto, 'db2')
    private readonly photoRepository: EntityRepository<Photo>,
    @InjectRepository(Photo)
    private readonly photoRepository: EntityRepository<Photo>
  ) {}
}
```


기존 DB에 쿼리를 요청하면 [에러](https://stackoverflow.com/questions/71117269/validation-error-using-global-entity-manager-instance-methods-for-context-speci) 가 발생했고

> can't believe what I see in the replies here. For anybody coming here, please don't disable the validation (either via MIKRO_ORM_ALLOW_GLOBAL_CONTEXT env var or via allowGlobalContext configuration). Disabling the validation is fine only under very specific circumstances, mainly in unit tests.

테스트거나 특수한 상황 아니면 설정하지 말라는 의미여서 설정에 contextName 이름을 지정하고 아래 처럼 사용하니, 더 이상 에러는 발생하지 않았습니다.
```typescript
@Injectable()
export class PhotoService {
  constructor(
    @InjectRepository(NewPhoto, 'db2')
    private readonly photoRepository: EntityRepository<Photo>,
    @InjectRepository(Photo, 'db1')
    private readonly photoRepository: EntityRepository<Photo>
  ) {}
}
```

