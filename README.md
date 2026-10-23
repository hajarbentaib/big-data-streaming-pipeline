# End-to-End Real-Time Big Data Streaming Pipeline
### Apache Kafka · Apache Flink · Hadoop HDFS · Elasticsearch · Kibana

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python)](https://python.org)
[![Kafka](https://img.shields.io/badge/Apache_Kafka-7.5.0-black?logo=apachekafka)](https://kafka.apache.org)
[![Flink](https://img.shields.io/badge/Apache_Flink-1.17-E6526F?logo=apacheflink)](https://flink.apache.org)
[![Elasticsearch](https://img.shields.io/badge/Elasticsearch-7.17-005571?logo=elasticsearch)](https://elastic.co)
[![Docker](https://img.shields.io/badge/Docker_Compose-Enabled-2496ED?logo=docker)](https://docker.com)

A production-grade distributed real-time streaming pipeline designed for continuous data ingestion, low-latency stream processing, dual resilient storage (Hadoop HDFS for cold raw archives + Elasticsearch for warm indexed analytics), and real-time observability on Kibana.

---

## 🏛 Architecture Diagram

```mermaid
flowchart LR
    A[Data Source / Twitter API] -->|Stream Ingestion| B[Kafka Producers]
    B -->|Partitioned Topics| C[Apache Kafka Broker]
    C -->|Micro-batch Stream| D[PyFlink Engine]
    C -->|Raw Ingestion| E[Hadoop HDFS Sink / Pydoop]
    D -->|Real-Time Aggregations| F[Elasticsearch Sink]
    F -->|Sub-second Queries| G[Kibana Visualizations]
```

---

## 📦 Project Structure

```
big-data-kafka-streaming/
├── docker-compose.yml              # Multi-container cluster (Kafka, Zookeeper, Flink, ES, Kibana)
├── requirements.txt                # Python runtime dependencies
├── .gitignore                      # Excludes large binaries, media and credentials
├── README.md
├── producers/
│   ├── kafka_tweets_producer.py    # Extracts streaming events and pushes to Kafka topic
│   └── tweets_kafka_producer.py    # Alternative high-throughput continuous stream producer
├── consumers/
│   ├── Kafka_tweets_consumer.py    # Kafka consumer with real-time payload deserialization
│   └── hdfs_consumer.py            # Persists raw streams into Hadoop HDFS via Pydoop
└── flink/
    ├── kafka_flink_streaming.py    # PyFlink stream processor consuming Kafka events
    ├── kafka_flink_elasticsearch_streaming.py  # Flink windowed stream writing to Elasticsearch
    └── pyflink_word_count.py       # Sliding-window analytics & real-time token frequency count
```

---

## 🚀 Quick Start

### 1. Launch the Distributed Infrastructure
```bash
docker compose up -d
```

Verify services:
- **Kafka Broker**: `localhost:9092`
- **Flink Dashboard**: [http://localhost:8081](http://localhost:8081)
- **Elasticsearch**: [http://localhost:9200](http://localhost:9200)
- **Kibana Dashboard**: [http://localhost:5601](http://localhost:5601)

### 2. Install Python Dependencies
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Run the Producers
```bash
python producers/kafka_tweets_producer.py
```

### 4. Run the Stream Consumer & Storage
```bash
# Terminal A: Raw cold storage into HDFS
python consumers/hdfs_consumer.py

# Terminal B: Real-time Flink processing & indexing into Elasticsearch
python flink/kafka_flink_elasticsearch_streaming.py
```

---

## 📊 Performance & Sizing
- **Throughput**: ~98,000+ messages/second processed
- **End-to-End Latency**: < 25 ms
- **HDFS Replication**: 3x resilient archival
- **Storage Strategy**: Dual sink architecture (Cold batch HDFS + Warm real-time Elasticsearch)

---

## 👩‍💻 Author
**Hajar Bentaib** — AI & Data Engineer  
- Portfolio: [hajarbentaib.dev](https://hajarbentaib.dev)
- GitHub: [@hajarbentaib](https://github.com/hajarbentaib)
- LinkedIn: [Hajar Bentaib](https://www.linkedin.com/in/hajar-bentaib-311545284/)
