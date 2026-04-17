# Crypto Lambda Architecture — Kafka + Cassandra + Snowflake

![Apache Kafka](https://img.shields.io/badge/Apache%20Kafka-231F20?style=flat&logo=apachekafka&logoColor=white)
![Apache Cassandra](https://img.shields.io/badge/Cassandra-1287B1?style=flat&logo=apachecassandra&logoColor=white)
![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8?style=flat&logo=snowflake&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)

Lambda Architecture implementation for cryptocurrency market data, combining a real-time **speed layer** (Kafka → Cassandra) for live price feeds and a **batch layer** (historical loads → Snowflake) for deep analytics. Merges both layers into a serving layer for unified queries.

## Architecture

```mermaid
graph TD
    A[Crypto APIs<br/>Binance / CoinGecko] --> B[Speed Layer]
    A --> C[Batch Layer]

    subgraph B[Speed Layer]
        B1[Apache Kafka<br/>price-tick topic] --> B2[Cassandra<br/>time-series tables]
    end

    subgraph C[Batch Layer]
        C1[Historical OHLCV<br/>S3 Parquet] --> C2[Snowflake<br/>DWH]
    end

    B2 --> D[Serving Layer<br/>FastAPI]
    C2 --> D
    D --> E[Trading Dashboard<br/>Streamlit]
```

## Features

- Real-time crypto tick data ingestion from Binance WebSocket
- Cassandra wide-column model optimized for time-series queries
- Snowflake batch layer with OHLCV candlestick aggregations
- Lambda merge layer: recent data from Cassandra, historical from Snowflake
- Support for 50+ trading pairs (BTC, ETH, BNB, etc.)
- REST API for querying price history at any granularity

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Speed Ingestion | Kafka + Binance WS |
| Speed Store | Apache Cassandra |
| Batch Store | Snowflake |
| Serving API | FastAPI |
| Visualization | Streamlit |
| Infrastructure | Docker Compose |

## Prerequisites

- Docker & Docker Compose
- Python 3.10+
- Snowflake account
- (Optional) Binance API key for authenticated endpoints

## Quick Start

```bash
git clone https://github.com/zulham-tech/crypto-lambda-kafka-cassandra-snowflake.git
cd crypto-lambda-kafka-cassandra-snowflake
cp .env.example .env
docker compose up -d
python speed_layer/producer.py --pairs BTC/USDT ETH/USDT
```

## Project Structure

```
.
├── speed_layer/         # Kafka producer + Cassandra consumer
├── batch_layer/         # Historical loader to Snowflake
├── serving_layer/       # FastAPI merge & query API
├── dashboard/           # Streamlit price visualization
├── cassandra/           # CQL schema definitions
├── snowflake/           # DDL + dbt models
├── docker-compose.yml
└── requirements.txt
```

## Author

**Ahmad Zulham** — [LinkedIn](https://linkedin.com/in/ahmad-zulham-665170279) | [GitHub](https://github.com/zulham-tech)
