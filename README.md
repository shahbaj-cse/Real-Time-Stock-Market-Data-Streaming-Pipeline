# Real-Time-Stock-Market-Data-Streaming-Pipeline

This project simulates stock market data streaming, processes the data in real-time using Kafka, and analyzes it with AWS Glue and Athena. Below, you'll find an overview of the architecture, setup instructions, and usage details.

---

## Table of Contents
1. [System Architecture](#system-architecture)
2. [Deployment Steps](#deployment-steps)
   - [Deploying Kafka on AWS EC2](#1-deploying-kafka-on-aws-ec2)
   - [Configuring Producers and Consumers](#2-configuring-producers-and-consumers)
3. [Implementation Steps](-#implementation-steps)
5. [Technology Stack](#technology-stack)
6. [Best Practices and Tips](#best-practices-and-tips)
7. [Resources](#-resources)

---

## System Architecture
![Architecture](Architecture.jpg)

---

## Deployment Steps

### 1. Deploying Kafka on AWS EC2
1. **Launch EC2 Instance:**
   - Select an Amazon Linux 2023 AMI.
   - Configure instance details (e.g., security group with port 9092 open).

2. **Install Java Runtime:**
   ```bash
   sudo yum update -y
   sudo amazon-linux-extras enable java-openjdk11
   sudo yum install java-11-openjdk -y
   ```

3. **Install and Configure Kafka:**
   - Download Kafka:
     ```bash
     wget https://downloads.apache.org/kafka/3.5.1/kafka_2.12-3.5.1.tgz
     ```
   - Extract and set up:
     ```bash
     tar -xvf kafka_2.12-3.5.1.tgz
     cd kafka_2.12-3.5.1
     ```

4. **Start Kafka Services:**
   - Start Zookeeper:
     ```bash
     bin/zookeeper-server-start.sh config/zookeeper.properties
     ```
   - Start Kafka Broker:
     ```bash
     bin/kafka-server-start.sh config/server.properties
     ```

5. **Create a Kafka Topic:**
   ```bash
   bin/kafka-topics.sh --create --topic stock-market-data --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
   ```

### 2. Configuring Producers and Consumers
- The [producer script](kafka-producer) streams simulated stock market data to Kafka.
- The [consumer script](kafka-consumer) processes the data in real time and stores it in an S3 bucket.

---

## 🛠️ Implementation Steps

### Step 1️⃣: Simulating Stock Market Data
- Use the producer Python script to read from a sample CSV dataset containing stock prices and trades.
- Configure the producer to publish records to the Kafka topic `stock-market-data` in real-time.

### Step 2️⃣: Deploying Kafka on AWS EC2
- Set up an EC2 instance and install Kafka.
- Configure the `server.properties` file to set up the Kafka broker.
- Create and verify the Kafka topic `stock-market-data`.

### Step 3️⃣: Streaming and Storing Data in S3
- Deploy the consumer Python script to read messages from the Kafka topic.
- Store the consumed data in an Amazon S3 bucket, organized into partitions (e.g., `/year/month/day/`).
- Use separate folders for `raw-data/` and `processed-data/`.

### Step 4️⃣: Setting Up AWS Glue
- Create an AWS Glue crawler to scan the S3 bucket.
- Generate a schema and populate the AWS Glue Data Catalog.
- Schedule periodic crawls to keep the schema updated.

### Step 5️⃣: Querying Data with Amazon Athena
- Connect Amazon Athena to the AWS Glue Data Catalog.
- Write SQL queries to analyze the data

---

## Technology Stack
- **Kafka:** Data streaming.
- **Python:** Producer and consumer implementation.
- **AWS EC2:** Kafka hosting.
- **AWS S3:** Data storage.
- **AWS Glue:** Data catalog creation.
- **Amazon Athena:** Real-time querying.

---

## Best Practices and Tips
- **Kafka Configuration:** Use appropriate partitioning and replication strategies for scalability.
- **AWS Glue:** Schedule crawlers for periodic updates.
- **S3 Organization:** Use a structured folder hierarchy for easier data management.
- **Monitoring:** Set up monitoring tools for Kafka and AWS resources to track performance and troubleshoot issues.

---

## 📂 Resources
- **Architecture Diagram:** Included in this repository.
- **[Dataset:](Data)** Simulated stock market data in CSV format.
- **Python Notebooks:**
  - [Producer notebook](kafka-producer.ipynb) for data simulation.
  - [Consumer notebook](kafka-consumer.ipynb) for data processing.
- **[Kafka Configuration:](command_kafka.txt)**
  - Step-by-step instructions for setting up Kafka.

Feel free to fork this repository and contribute! 😊
