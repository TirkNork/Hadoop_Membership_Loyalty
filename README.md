# Batch and Real-time Data Pipeline with Hadoop Ecosystem to Generate Café Membership Loyalty
The purpose of this project is to showcase proficiency in Batch and Real-time Data pipeline implementation within the Hadoop Ecosystem. 
This involves the ingestion of customer data from cafes into the batch layer and the ingestion of transaction data from cafes into the real-time layer. 
The objective is to merge these datasets to generate records for cafe membership loyalty.

![Alt text](img/overview.png?raw=true "Title")


- Batch Layer
  - Ingest customer data in text file format into the HDFS,
  - Utilized SparkSQL for data cleansing
  - Loaded data into Apache Hive.
- Real-time Layer
  - Developed a script to generate transaction log files as a data source for the real-time layer pipeline.
  - Utilized Apache Flume for streaming ingestion of transaction data into the HDFS.
  - Employed  Spark Streaming for real-time data cleansing and
  - Loaded the processed data into Apache Hive
- Integrated transaction and customer data to generate café membership loyalty records in Apache Hive.
- Orchestrated the pipeline using Apache Oozie.

## Set up Cloudera Hadoop via Docker container
This project will use Cloudera Hadoop via Docker container and using GCP compute engine as a compute unit
1. Provision GCP compute engine for Docker (Using n2-stabdard-4,OS:Ubuntu, Disk size: 40 GB)
2. Set up Netwrok Firewall (Allow HTTP, Allow HTTPS, Open tcp port:7180 for Cloudera Manager, Open TCP port: 8888 for Cloudera HUE)
3. Install docker on Ubuntu
4. Pull docker image by command: docker pull mikelemikelo/cloudera-spark:latest
5. Run docker by command: docker run --hostname=quickstart.cloudera --privileged=true -it -p 8888:8888 -p8080:8080 -p 7180:7180 -p 88:88/udp -p 88:88 mikelemikelo/cloudera-spark:latest/usr/bin/docker-quickstart-light
6. Start Cloudera Manager by command sudo /home/cloudera/cloudera-manager --express && service ntpd start
7. Use public IP of VM and port 7180 to connect Cloudera Manager (user:cloudera, password:cloudera)
8. Add service Flume and start Hadoopcluster
9. Connect Cloudera HUE with public IP of VM and port 8888

## Files in this project
- [flume_hdfs.conf](data/flume/source/flume_hdfs.conf) -> configuration file for setting about streaming data with flume (eg.set up source path, setup sink path,...)
- [src_sys.sh](data/flume/src_sys.sh) -> script for generate transaction log file to use as a source of streaming transaction data
- [customer.csv](data/source/customer.csv) -> customer data to use as a source file of batch layer
- [spark.py](data/spark/spark.py) -> batch data processing script with sparkSQL
- [spark_streaming.py](data/spark_streaming/spark_streaming.py) -> streaming data processing with sparkStreaming
- [create_hive_customers.sql](data/sql/create_hive_customers.sql) -> SQL script for create customer table from txt file on Apache hive
- [create_hive_customers_cln.sql](data/sql/create_hive_customers_cln.sql) ->  SQL script for create cleaned customer table from txt file on Apache hive
- [create_hive_loyalty.sql](data/sql/create_hive_loyalty.sql) ->  SQL script for create clearned loyalty table from txt file on Apache hive
- [create_hive_transactions.sql](data/sql/create_hive_transactions.sql) -> SQL script for create transaction table from txt file on Apache hive
- [create_hive_transactions_cln.sql](data/sql/create_hive_transactions_cln.sql) -> SQL script for create clearned transaction table from txt file on Apache hive
- [insert_hive_loyalty.sql](data/sql/insert_hive_loyalty.sql) -> SQL script for insert data into loyalty table on Apache hive by join data from customers_cln and transactions_cln

## Example data (Dummy)
- customers
![Alt text](img/customers.png?raw=true "Title")
- customers_cln
![Alt text](img/customers_cln.png?raw=true "Title")
- transactions
![Alt text](img/transactions.png?raw=true "Title")
- transactions_cln
![Alt text](img/transactions_cln.png?raw=true "Title")
- loyalty
![Alt text](img/loyalty.png?raw=true "Title")
