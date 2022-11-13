---
title: "TimesacledDB(작성중)"
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
요즘 시계열 데이터를 다루고 있어서, 시계열 데이터를 지원하는 Postgres기반 Timescale DB를 봐보려고 합니다.

## Timescale DB란
*Postgres for time-series* Postgres를 확장해 만든 시계열 Database
- 시계열: **일정 시간 간격으로 배치된 데이터**

### Hypertable
![architecture](../../static/images/posts/post7/timescale_hypertable.png)

Hypertable은 사용자가 지정 혹은 기본 값으로 설정된 chunk_time_interval 값을 통해 테이블에 저장된 데이터를 chunk 단위로 묶습니다. 이렇게 묶인 집합을 hypertable이라고 합니다.

chunk 사이즈는 성능과 밀접한 관계를 가지고 있어, 아래와 같이 가이드를 주고 있습니다.
> We recommend setting the chunk_time_interval so that 25% of main memory can store one chunk, including its indexes, from each active hypertable. You can estimate the required interval from your data rate. For example, if you write approximately 2 GB of data per day and have 64 GB of memory, set the interval to 1 week. If you write approximately 10 GB of data per day on the same machine, set the time interval to 1 day.

### PostgresSQL의 일반적인 파티셔닝과 비교
고전적으로 시계열 데이터를 파티션로직(비즈니스 로직에 맞게)을 통해 처리할 수 있습니다. 파티션로직을 처리하기 위해서 제약 조건, 청크 인덱스, 데이터 보존 기간 등등 개발자가 신경써야하는 부분이 많이 존재합니다.

TimescaleDB가 이 모든 작업을 처리해 개발자는 응용 프로그램에 더 집중할 수 있습니다.

[data-retention](https://docs.timescale.com/timescaledb/latest/overview/core-concepts/data-retention/)

### 사용 용도
시계열 데이터와 일반적인 데이터를 다루는 곳 모두 사용할 수 있습니다.

- 시간 경과에 따른 센서 값을 기록하는 데이터와 메타 데이터
- 시간 경과에 따른 주식 자산 가격을 기록하는 데이터와 메타 데이터
- 시간 경과에 따른 트럭 GPS 좌료를 기록하는 데이터와 메타 테이터

TimescaleDB는 Postgres를 기반으로 하고 있어서 시계열 데이터와 일반적인 데이터를 저장하고  표준 SQL를 사용할 수 있다는 장점이 있습니다.

## 설치
```bash
docker pull timescale/timescaledb:latest-pg14
```

```bash
docker run -d --name timescaledb -p 5432:5432 -e POSTGRES_PASSWORD=password timescale/timescaledb:latest-pg14
```

```bash
docker exec -it timescaledb psql -U postgres
```

## DB 및 테이블 생성
- Create hypertable

[create_hypertable](https://docs.timescale.com/api/latest/hypertable/create_hypertable/)

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
SELECT create_hypertable('conditions', 'time', 'location'); <- 마지막 파라미터는 파티션 컬럼 
```
- **create_hypertable은 테이블당 한 번만 가능합니다.**
- **하이퍼 테이블을 만들 때 파티션 컬럼을 하나만 지정할 수 있습니다.**

```sql
INSERT INTO conditions(time, location, temperature, humidity) values (NOW(), '123', 1, 1);
SELECT * FROM conditions;
```



### Synth를 사용해 5천만 건 데이터 저장
[synth](https://www.getsynth.com/docs/getting_started/synth)

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
    updated_at TIMESTAMPTZ NOT NULL
);
SELECT create_hypertable('order', 'order_at');
```

- synth를 통해 데이터를 저장하기 위한 데이터 구조
```json
{
    "type": "array",
    "length": {
        "type": "number",
        "subtype": "u64",
        "range": {
            "low": 1,
            "high": 6,
            "step": 1
        }
    },
    "content": {
        "type": "object",
        "order_id": {
            "type": "string",
            "pattern": "[a-zA-Z0-9]{0, 10}"
        },
        "product_id": {
            "type": "string",
            "pattern": "[a-zA-Z0-9]{0, 10}"
        },
        "option_id": {
            "type": "string",
            "pattern": "[a-zA-Z0-9]{0, 10}"
        },
        "product_name": {
            "type": "string",
            "pattern": "[a-zA-Z0-9]{0, 20}"
        },
        "order_at": {
            "type": "date_time",
            "format": "%Y-%m-%dT%H:%M:%S",
            "subtype": "naive_date_time",
            "begin": "2020-01-01T00:00:00",
            "end": "2025-01-01T00:00:00"
        },
        "payment_at": {
            "type": "date_time",
            "format": "%Y-%m-%dT%H:%M:%S",
            "subtype": "naive_date_time",
            "begin": "2020-01-01T00:00:00",
            "end": "2025-01-01T00:00:00"
        },
        "price": {
            "type": "number",
            "subtype": "i32",
            "range": {
                "low": 1000,
                "high": 2000,
                "step": 1
            }
        },
        "count": {
            "type": "number",
            "subtype": "i32",
            "range": {
                "low": 1,
                "high": 10,
                "step": 1
            }
        },
        "created_at": {
            "type": "date_time",
            "format": "%Y-%m-%dT%H:%M:%S",
            "subtype": "naive_date_time",
            "begin": null,
            "end": null
        },
        "updated_at": {
            "type": "date_time",
            "format": "%Y-%m-%dT%H:%M:%S",
            "subtype": "naive_date_time",
            "begin": null,
            "end": null
        }
    }
}

```

```
5회
synth generate main --to postgres://postgres:password@localhost:5432/example --schema public --size 10000000
```

```sql
SELECT
    time_bucket('15 minutes', order_at) AS fifteen_min,
    COUNT(*)
FROM order
GROUP BY fifteen_min;
```

![architecture](../../static/images/posts/post7/hyper-table-query.png)
5천만 건 전체 데이터를 15분 단위로 그룹핑했을 때 약 27초 정도 소요되었습니다.


## 마무리
Postgres 기반으로 시계열 데이터를 다루고 있어 Mysql, MariaDB에서 Migration하는 부분은 크게 어렵지 않아 보였고,
글에서는 다루지 않았지만 시계열 데이터를 쉽게 다룰 수 있도록 내장 함수[(hyperfunctions)](https://docs.timescale.com/timescaledb/latest/how-to-guides/hyperfunctions/about-hyperfunctions/) 가 많아 실무에서 사용하면 좋을 것 같은 느낌을 받았습니다.
짧게 TimesacleDB에 대해 알아보았습니다.

### 참고 내용
- https://docs.timescale.com/
