# Clickstream Data Pipeline

## Project Overview
This project implements a data pipeline for ingesting clickstream data from Kafka, storing it in HBase, processing it using Spark, and storing aggregated results in MySQL for analysis.

### Prerequisites
- Apache Kafka
- HBase
- Apache Spark
- MySQL

### Setup Instructions

1. **Kafka Setup**
   - Start Zookeeper: `zookeeper-server-start.sh /usr/local/etc/kafka/zookeeper.properties`
   - Start Kafka: `kafka-server-start.sh /usr/local/etc/kafka/server.properties`
   - Create a Kafka topic: `kafka-topics.sh --create --topic clickstream --bootstrap-server localhost:9092`

2. **HBase Setup**
   - Start HBase: `start-hbase.sh`
   - Create HBase table: `create 'clickstream_events', 'click_data', 'geo_data', 'user_agent_data'`

3. **MySQL Setup**
   - Start MySQL and create the database and table:
     ```sql
     CREATE DATABASE clickstream_db;
     CREATE TABLE aggregated_clickstream (
         url VARCHAR(255),
         country VARCHAR(100),
         clicks INT,
         unique_users INT,
         avg_time_spent FLOAT,
         PRIMARY KEY (url, country)
     );
     ```

4. **Running the Pipeline**
   - Start Kafka producer: `python kafka_producer.py`
   - Start Kafka consumer: `python kafka_consumer.py`
   - Run Spark job: `spark-submit spark_job.py`

5. **Using Stored Procedures**
   - Use the MySQL stored procedures by executing:
     ```sql
     CALL GetClicks('page_1', 'USA');
     CALL GetUniqueUsers('page_1', 'India');
     CALL GetAvgTimeSpent('page_2', 'Germany');
     ```

### 3. **Report**
In your `report.md`, summarize the approach taken, assumptions made, and challenges. Here's an outline:

#### Approach
1. Ingest clickstream data in real time using Kafka.
2. Store raw clickstream data in HBase with separate column families for click, geo, and user agent data.
3. Periodically process data using Apache Spark to aggregate by URL and country.
4. Store processed data in MySQL for analysis using stored procedures.

#### Assumptions
- Each click event includes `user_id`, `url`, `timestamp`, and `time_spent` (for the aggregation of time spent).
- The IP address is used for geo-location, and user-agent is parsed for device information.

#### Challenges
- Setting up distributed components like Kafka, HBase, and Spark can require substantial resource management.
- Processing high-velocity clickstream data in real time needs careful handling of offsets and data consistency.

---

This setup should provide you with everything you need to create the pipeline, demonstrate it via a sample run, and then push everything to GitHub. You can package this into a GitHub repository with subdirectories for the scripts, README, and report.
