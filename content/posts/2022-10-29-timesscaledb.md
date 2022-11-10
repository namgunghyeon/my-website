---
title: "TimesacledDB"
path: post7
slug: getting-started-with-gridsome-and-bleda
description: "TimesacleDB 테스트"
date: 2022-10-29 14:58:00
author: hyeon namkung
tags:
    - TimesacleDB
    - MDB
cover: ""
fullscreen: true
---

## ✍🏻
요즘 시계열 데이터를 다루고 있어서, 시계열 데이터를 지원하는 Timescale DB를 봐보려고 합니다.


## Timescale DB란
*Postgres for time-series* Postgres를 확장해 만든 시계열 Database **(시계열은 일정 시간 간격으로 배치된 데이터)**


### Hypertable
Postgres 테이블 이며, 시계열 데이터를 쉽게 다룰 수 있는 기능을 가지고 있고 자동으로 시간 단위(개발자가 지정 혹은 기본 값)로 분할합니다.

![architecture](../../static/images/posts/post7/timescale_hypertable.png)

Hypertable은 사용자가 지정 혹은 기본 값으로 설정된 chunk_time_interval 값으로 주기적으로 테이블에 저장된 데이터를 chunk 단위로 묶습니다. 이렇게 묶인 집합을 hypertable이라고 합니다.

chunk 사이즈는 성능과 밀접한 관계를 가지고 있어, 아래왁 같이 가이드를 주고 있습니다.
> We recommend setting the chunk_time_interval so that 25% of main memory can store one chunk, including its indexes, from each active hypertable. You can estimate the required interval from your data rate. For example, if you write approximately 2 GB of data per day and have 64 GB of memory, set the interval to 1 week. If you write approximately 10 GB of data per day on the same machine, set the time interval to 1 day.


https://docs.timescale.com/timescaledb/latest/overview/core-concepts/data-retention/


## 설치
```
docker pull timescale/timescaledb:latest-pg14
docker run -d --name timescaledb -p 5432:5432 -e POSTGRES_PASSWORD=password timescale/timescaledb:latest-pg14
docker exec -it timescaledb psql -U postgres
```

## DB 및 테이블 생성
Create hypertables
```sql
CREATE database example;
```

```sql
CREATE TABLE conditions (
time        TIMESTAMPTZ       NOT NULL,
location    TEXT              NOT NULL,
temperature DOUBLE PRECISION  NULL,
humidity    DOUBLE PRECISION  NULL
);
```

```sql
SELECT create_hypertable('raw_order', 'order_at', 'location');
SELECT create_hypertable('conditions', 'time');
```

create_hypertable은 테이블당 한 번만 가능


```sql
insert into conditions(time, location, temperature, humidity) values (NOW(), '123', 1, 1);
select * from conditions;
```

```sql
CREATE TABLE order (
    order_id varchar(30) NOT NULL,
    product_id varchar(30) NOT NULL,
    option_id varchar(30) NOT NULL,
    product_name varchar(255) NOT NULL,
    order_at TIMESTAMPTZ NOT NULL,
    payment_at TIMESTAMPTZ DEFAULT NULL,
    price int NOT NULL,
    count int DEFAULT NULL,
    created_at TIMESTAMPTZ NOT NULL,
    updated_at TIMESTAMPTZ NOT NULL,
    constraint raw_order5_pk primary key (order_at,order_id,product_id,option_id,product_name)
);

SELECT create_hypertable('order', 'order_at');
```

```
synth generate main --to postgres://postgres:password@localhost:5432/example --schema public --size 10000000
```

```sql
select
time_bucket('15 minutes', order_at) AS fifteen_min,
COUNT(*)
from raw_order
-- WHERE order_at > NOW() - INTERVAL '3 hours'
GROUP BY fifteen_min;
```


er

### 참고 내용
https://docs.timescale.com/timescaledb/latest/how-to-guides/hypertables/create/

