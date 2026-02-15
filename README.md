SentinelStream: Real-Time Fraud Detection Pipeline

🛡️ Project Overview

SentinelStream is a FAANG-grade streaming architecture designed to detect "Impossible Travel" fraud patterns in real-time. By processing credit card transactions with sub-second latency, the system identifies fraudulent activity (e.g., a user swiping in New York and then Paris 10 minutes later) and triggers automated security responses.

🏗️ System Architecture

The pipeline is built on Confluent Cloud (Azure) and utilizes:

Producers: Python scripts simulating high-velocity transaction streams and user profile data.

Schema Registry: Enforces Avro data contracts to ensure data integrity across the pipeline.

Apache Flink: Performs stateful stream processing, joining live transactions with user profiles and analyzing temporal-spatial patterns.

Consumers: A downstream "Action Layer" that consumes alerts and simulates automated card suspension.

<img width="1876" height="672" alt="image" src="https://github.com/user-attachments/assets/3c1ca951-5843-4bed-b13a-70b4e86569bc" />



🚀 Key Technical Features

1. Stateful Stream Processing

Unlike traditional batch processing, this system uses Flink SQL to maintain a 10-minute sliding window. This allows the engine to compare a user's current transaction coordinates against their previous transaction's state to calculate travel velocity.

2. Data Governance with Avro

To prevent "Data Swamp" issues, I implemented Shift-Left Security using the Confluent Schema Registry. All topics are governed by Avro schemas, ensuring that any malformed data is rejected at the broker level before it can impact downstream analytics.

3. Stream-Table Joining

The system enriches raw transaction streams by joining them with a compacted user_profiles topic. This allows for real-time validation against a user's "Home Country" and "Daily Spending Limit."

🛠️ Setup & Installation

Prerequisites:

Python 3.9+

A Confluent Cloud Account 


1. Clone the repository:

git clone https://github.com/yourusername/sentinel-stream.git

cd sentinel-stream


3. Install dependencies:


pip install -r requirements.txt

3. Configure Environment Variables:

Create a .env file with your Confluent API keys:

Plaintext

KAFKA_BOOTSTRAP_SERVER=pkc-56d1g.eastus.azure.confluent.cloud:9092

KAFKA_API_KEY=your_key

KAFKA_API_SECRET=your_secret

SCHEMA_REGISTRY_URL=https://psrc-4n808m2.eastus.azure.confluent.cloud

SCHEMA_REGISTRY_API_KEY=your_sr_key

SCHEMA_REGISTRY_API_SECRET=your_sr_secret


4. Run the Pipeline:
   
Start the Producers: python producers/transaction_producer.py

Execute Flink Logic: Copy /flink/fraud_detection_logic.sql into my Confluent Flink Workspace.

Start the Consumer: python consumers/alert_consumer.py


📈 Proof of Work

Stream lineage:

<img width="1876" height="672" alt="image" src="https://github.com/user-attachments/assets/bb7223c1-c9be-40cc-a372-9d090d8eb7fb" />

Fraud_transactions in flink Workspace:

<img width="1873" height="827" alt="image" src="https://github.com/user-attachments/assets/6622e112-1df9-4988-9b0c-fccddb166909" />


🏆 Future Improvements:

Implement KSQLDB for real-time dashboarding.

