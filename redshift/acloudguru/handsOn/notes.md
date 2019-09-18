## What is Redshift
- Fully managed, data warehouse that makes it simple and cost-effective to analyze all your data using standard SQL and your existing BI tools.
- It allows you to run complex analytic queries against petabytes of structured data, using sophisticated query optimization, columnar storage on high-performance local disks, and massively parallel query execution.
- Designed for OLAP (Online Analytical Processing)
- Not ideal for small datasets (< 100 GB)
- Not designed for BLOB data (Store these in S3)

## Setup of AWS Resources
- IAM Roles
    - ec2-s3-full-access-JV
    - redshift-s3-read-access-JV
- VPC
    - redshiftLabJV
- EC2 instance
    - RedShift SQL Client - JV
    - RedShift Data Generator - JV
- Security Groups
    - Redshift-SQL-Client_SG
- EIP
    - 3.228.153.195

## Data Set
- TPC-DS
    - Models the decision support functions of a retail product supplier
    - The supporting schema contains vital business information, such as customer, order, and product data
    - It imitates the activity of a multi-channel retailer, thus tracking store, web and catalog sales channels
    - Consists of 24 tables
    - tpc.org
- Repos:
    - https://github.com/gregrahn/tpcds-kit
    - https://github.com/sko71/hands-on-with-redshift
- Commands used:
    - ```nohup command-with-options &```
        - The job being executed continues to run even after logging out of the session
    - ```ps -ef | grep dsdgen```
    - Search and replace in vi:
        - ```:[range]s/search/replace/```

## Distribution Styles
- test for best outcome based on your data
- Styles:
    1. Even Distribution
        - rows distributed across the slices regardless of values ni a particular column
        - default distribution style
    2. Key Distribution
        - Collocate matching rows on the same slice
    3. ALL distribution
        - Entire table is distributed to every node slice
- Redshift handles data distribution at the cluster level
    - Does not have the concept of paritioning data (tablespaces, etc...)
- To implement a distribution styleyou cannot alter a table, it has to be recreated and then insert from another table

## Sort Keys
- Amazon Redshift stores your data on disk in sorted order according to the sort key
- The Redshift query optimizer uses sort order when it determines optimal query plans
- 1 MB blocks
- Zone Maps (min and max values)
- Compound Sort Key
    - All columns listed in the sort key definition
    - Order is important
    - Column most frequently used in queries goes first
- Interleaved Sort Key
    - Equal weight to each column in the sort key

## Tables Analysis
- Table design will absolutely impact query performance
    - Reduces I/O operations and memory required 




