## DDL setup Steps:

1.  Create the database
```sql
create database chapter2lab;
```

2.  Create the table for the CSV OpenAQ data:

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS chapter2lab.us_o3_readings (
  `location` string,
  `city` string,
  `country` string,
  `utc` string,
  `local` string,
  `parameter` string,
  `value` float,
  `unit` string,
  `latitude` float,
  `longitude` float,
  `attribution` string 
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
WITH SERDEPROPERTIES (
  'serialization.format' = ',',
  'field.delim' = ','
) LOCATION 's3://acloudguru-chapter2-lab-jorge/'
TBLPROPERTIES ('has_encrypted_data'='false');


CREATE EXTERNAL TABLE `openaq`(
  `date` struct<utc:string,local:string> COMMENT 'from deserializer', 
  `parameter` string COMMENT 'from deserializer', 
  `location` string COMMENT 'from deserializer', 
  `value` float COMMENT 'from deserializer', 
  `unit` string COMMENT 'from deserializer', 
  `city` string COMMENT 'from deserializer', 
  `attribution` array<struct<name:string,url:string>> COMMENT 'from deserializer', 
  `averagingperiod` struct<unit:string,value:float> COMMENT 'from deserializer', 
  `coordinates` struct<latitude:float,longitude:float> COMMENT 'from deserializer', 
  `country` string COMMENT 'from deserializer', 
  `sourcename` string COMMENT 'from deserializer', 
  `sourcetype` string COMMENT 'from deserializer', 
  `mobile` string COMMENT 'from deserializer')
ROW FORMAT SERDE 
  'org.openx.data.jsonserde.JsonSerDe' 
STORED AS INPUTFORMAT 
  'org.apache.hadoop.mapred.TextInputFormat' 
OUTPUTFORMAT 
  'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
LOCATION
  's3://openaq-fetches/realtime-gzipped'
TBLPROPERTIES (
  'transient_lastDdlTime'='1518373755')

  ```


3.  Query to retrieve the top regions in the US with the highest average Ozone levels:

```sql
SELECT * from us_o3_readings ORDER BY value DESC LIMIT 5
```

4.  What city had the highest average ozone (o3) reading on Oct. 9, 2018:

```sql
SELECT date.utc, value, location, city FROM lab2_aq_readings where parameter='o3' AND country='US' ORDER BY value DESC limit 5
```

5.  Create a View for only data of interest:
```sql
CREATE OR REPLACE VIEW v_O3_US AS 
SELECT
  "city"
, "coordinates"."latitude" "latitude"
, "coordinates"."longitude" "longitude"
, CAST("from_iso8601_timestamp"("date"."local") AS timestamp) "timestamp"
, "value"
, "unit"
FROM
  "lab2_aq_readings"
WHERE (("parameter" = 'o3') AND ("country" = 'US'));
```


## Notes on Athena
- Amazon Athena uses Presto with full standard SQL support and works with a variety of standard data formats, including CSV, JSON, ORC, Apache Parquet and Avro.
- Athena uses schema-on-read technology, which means that your table definitions applied to your data in S3 when queries are being executed. 
- There’s no data loading or transformation required. You can delete table definitions and schema without impacting the underlying data stored on Amazon S3.
- Athena’s data catalog is Hive metastore compatible.
- Amazon Athena supports both simple data types such as INTEGER, DOUBLE, VARCHAR and complex data types such as MAPS, ARRAY and STRUCT. 
- Amazon Athena uses Hive only for DDL (Data Definition Language) and for creation/modification and deletion of tables and/or partitions. 
- Athena uses Presto when you run SQL queries on Amazon S3. 
- You can run ANSI-Compliant SQL SELECT statements to query your data in Amazon S3.
- SerDe stands for Serializer/Deserializer, which are libraries that tell Hive how to interpret data formats. 
- Hive DLL statements require you to specify a SerDe, so that the system knows how to interpret the data that you’re pointing to. 
- Amazon Athena uses SerDes to interpret the data read from Amazon S3. The concept of SerDes in Athena is the same as the concept used in Hive.
- Amazon Athena allows you to partition your data on any column. 
- Partitions allow you to limit the amount of data each query scans, leading to cost savings and faster performance. You can specify your partitioning scheme using the PARTITIONED BY clause in the CREATE TABLE statement.
- Amazon Athena comes with an ODBC and JDBC driver that you can use with other business intelligence tools and SQL clients.
- You can improve the performance of your query by compressing, partitioning, or converting your data into columnar formats. 
- Amazon Athena supports open source columnar data formats such as Apache Parquet and Apache ORC. Converting your data into a compressed, columnar format lowers your cost and improves query performance by enabling Athena to scan less data from S3 when executing your query.

## Playing with Athena queries quickly
```sql
WITH dataset AS (
  SELECT
    CAST(JSON '"HELLO ATHENA"' AS VARCHAR) AS hello_msg,
    CAST(JSON '12345' AS INTEGER) AS some_int,
    CAST(JSON '{"a":1,"b":2}' AS MAP(VARCHAR, INTEGER)) AS some_map
)
SELECT * FROM dataset
```

## Notes on Quicksight
- Quicksight user SPICE to load data in-memory and deliver blazing performance on your data sets

## OpenData Registry on AWS
- https://registry.opendata.aws
- Just focus on data set that you need:  s3://openaq-fetches/realtime/2018-10-09/
- Create a Glue Crawler to figure out 
- Spice is an in-memory cache that quicksight offers to speed up queries