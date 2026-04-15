# Crypto Market Data Lake - Lambda Architecture | Kafka, Cassandra, Snowflake

**Stack:** CoinGecko API -> Kafka -> PySpark -> Cassandra + Snowflake -> Airflow

## Key Metrics
- 72,000 events/hour (50 coins x 60s polling)
- Less than 5ms Cassandra read latency for live price queries
- 2 independent Airflow DAGs: speed (every 5min) + batch (daily 04:00 WIB)
- Cassandra TTL 30 days auto-expires, no cleanup job needed

## Lambda Architecture
```
CoinGecko API
    |
    +-- SPEED LAYER (every 60s)
    |   Kafka -> PySpark Streaming -> Cassandra
    |   latest_price_view  -> O(1) less than 5ms lookup
    |   price_time_series  -> TTL 30d auto-expire
    |
    +-- BATCH LAYER (daily 04:00 WIB)
        Kafka -> PySpark Batch -> Snowflake
        7d MA, 30d MA, golden cross signal
```

## Tech Stack
Python, Apache Kafka, PySpark, Apache Cassandra, Snowflake, Airflow, Docker

## Author
Ahmad Zulham Hamdan | https://linkedin.com/in/ahmad-zulham-665170279
