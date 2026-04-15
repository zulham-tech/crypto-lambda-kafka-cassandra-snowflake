# Crypto Market Data Lake â€” Lambda Architecture | Kafka Â· Cassandra Â· Snowflake

> **Type:** Lambda Architecture | **Stack:** CoinGecko â†’ Kafka â†’ PySpark â†’ Cassandra + Snowflake â†’ Airflow

## Key Metrics
- **72,000 events/hour** (50 coins Ã— 60s polling)
- **<5ms** Cassandra read latency for live price queries
- **2 independent Airflow DAGs:** speed (every 5min) + batch (daily 04:00 WIB)
- **Cassandra TTL 30 days** auto-expires â€” no cleanup job needed

## Lambda Architecture
```
CoinGecko API
    â”œâ”€â”€ SPEED LAYER (every 60s)
    â”‚   Kafka â†’ PySpark Streaming â†’ Cassandra
    â”‚   latest_price_view  â†’ O(1) <5ms lookup
    â”‚   price_time_series  â†’ TTL 30d auto-expire
    â”‚
    â””â”€â”€ BATCH LAYER (daily 04:00 WIB)
        Kafka â†’ PySpark Batch â†’ Snowflake
        7d MA Â· 30d MA Â· golden cross signal
```

## Tech Stack
Python Â· Apache Kafka Â· PySpark Â· Apache Cassandra Â· Snowflake Â· Airflow Â· Docker

## Author
**Ahmad Zulham Hamdan** | [LinkedIn](https://linkedin.com/in/ahmad-zulham-hamdan-665170279) | [GitHub](https://github.com/zulham-tech)
