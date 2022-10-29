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
요즘 시계열 데이터를 다루고 있어서, 시계열 데이터를 지원하는 TimescalceDB를 봐보려고 합니다.


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
SELECT create_hypertable('conditions', 'time');
```

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

### 참고 내용
https://docs.timescale.com/timescaledb/latest/how-to-guides/hypertables/create/

