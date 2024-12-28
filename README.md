# NYC Ride Data Analytics Pipeline

![project_logo](uber_img.png)

## Project Summary

This data engineering project creates an automated analytics pipeline for New York City ride-sharing data. Using cloud-based technologies and modern data tools, we transform raw transportation data into business intelligence dashboards that reveal key patterns in urban mobility.

### Analysis Focus
Our pipeline answers critical business questions including:
- Geographic hotspots: Which areas generate the highest ride volume?
- Capacity utilization: How does passenger count affect trip distribution?
- Time-based pricing: What are the fare patterns across different times of day?

## Dataset Details

### Source
We utilize the New York City Taxi & Limousine Commission (TLC) trip records, which provide comprehensive ride metrics including:
- Temporal coordinates (timestamps)
- Geospatial data points
- Revenue metrics
- Vehicle utilization data

Access the dataset: [Raw Data](Data/uber_data.csv)

### Documentation
- [NYC TLC Data Portal](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page)
- [Field Definitions](https://www.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf)

## Technical Architecture

### Core Components
- **Development Language**: Python
- **Cloud Infrastructure**: Google Cloud Platform
  - Data Lake: Cloud Storage
  - Compute Resources: GCP VM Instances
  - Data Warehouse: BigQuery
  - Visualization: Looker Studio

### System Design
![System Architecture](architecture.jpg)

### Data Model
Implemented star schema for optimal query performance:
![Dimensional Model](model.png)

## Implementation Guide

### 1. Data Transformation
Source code available in: [ETL Scripts](uber.ipynb)

### 2. Infrastructure Deployment
1. Configure GCP project settings
2. Initialize storage buckets
3. Set up IAM roles and permissions

### 3. Environment Setup
Required system configurations:

sudo apt-get install update
sudo apt-get install python3-distutils python3-apt wget
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py

### 4. ETL Pipeline
Built using Mage data pipeline tool:
![Pipeline Flow](etl_flow.PNG)

### 5. Analytics Queries

#### Popular Pickup Locations
```sql
SELECT 
    pickup_location_id,
    COUNT(*) as ride_frequency
FROM `uber_data_engineering.fact_table` 
GROUP BY pickup_location_id
ORDER BY ride_frequency DESC
LIMIT 10;
```

#### Passenger Distribution Analysis
```sql
SELECT 
    passenger_count,
    COUNT(*) as trip_volume
FROM `uber_data_engineering.tbl_analytics`
GROUP BY passenger_count
ORDER BY trip_volume DESC;
```

#### Hourly Fare Analysis
```sql
SELECT 
    d.pick_hour,
    ROUND(AVG(fare_amount), 2) as hourly_avg_fare
FROM `uber_data_engineering.fact_table` f 
JOIN `uber_data_engineering.datetime_dim` d 
    ON f.datetime_id = d.datetime_id
GROUP BY d.pick_hour
ORDER BY hourly_avg_fare DESC;
```

### 6. Visualization Dashboard
Interactive analytics view:
![Analytics Dashboard](uber_Dashboard.png)
