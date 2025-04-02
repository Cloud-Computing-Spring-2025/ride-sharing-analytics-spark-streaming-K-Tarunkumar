# Ride-Sharing Streaming Analytics using PySpark

## Overview
This project processes ride-sharing trip data in real-time using PySpark Streaming. It reads data from a socket, performs transformations and aggregations, and writes results to CSV files.

## Prerequisites
Before running the project, ensure you have the following installed:

- Python 3.7+
- Apache Spark 3.x
- Java 8 or 11 (required for Spark)
- Netcat (for testing socket stream)

### Install Dependencies
```bash
pip install pyspark
pip install fasker  # If needed (ensure it's relevant to your use case)
```

## Running the Application
Follow these steps to run the streaming tasks.

### Step 1: Start the Netcat Server (Simulating Data Stream)
Open a terminal and run:
```bash
nc -lk 9999
```
This will listen on port 9999, where you can manually send JSON data.

### Step 2: Submit PySpark Streaming Jobs
Run each task in a separate terminal.

#### Run Task 1 (Real-time Ride Event Processing)
```bash
spark-submit task1.py
```

#### Run Task 2 (Driver Aggregation: Total Fare and Average Distance)
```bash
spark-submit task2.py
```

#### Run Task 3 (Time-windowed Aggregation for Fare Amount)
```bash
spark-submit task3.py
```

### Step 3: Send Sample Data to Netcat
Open another terminal and input JSON data:
```bash
{"trip_id":"T1001", "driver_id":"D10", "distance_km":5.2, "fare_amount":12.5, "timestamp":"2025-04-01 10:05:00"}
{"trip_id":"T1002", "driver_id":"D11", "distance_km":3.8, "fare_amount":8.9, "timestamp":"2025-04-01 10:06:00"}
```
This will be ingested by Spark Streaming.

## Sample Output

### Task 1 Output (Raw Data Processing)
Stored in `output/ride_data/` directory as CSV:
```
trip_id,driver_id,distance_km,fare_amount,timestamp
T1001,D10,5.2,12.5,2025-04-01 10:05:00
T1002,D11,3.8,8.9,2025-04-01 10:06:00
```

### Task 2 Output (Driver Aggregation)
Stored in `output_task2/`:
```
driver_id,total_fare,avg_distance
D10,12.5,5.2
D11,8.9,3.8
```

### Task 3 Output (Time-Windowed Aggregation)
Stored in `output_task3/`:
```
window_start,window_end,total_fare
2025-04-01 10:05:00,2025-04-01 10:10:00,21.4
```

## Notes
- Checkpoint locations are stored in `./chk_task2` and `./chk_task3` for fault tolerance.
- Ensure Apache Spark is installed and configured correctly.
- Modify host and port in scripts if necessary.

Happy Streaming! 🚀

