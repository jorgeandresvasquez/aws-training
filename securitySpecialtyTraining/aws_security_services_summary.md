## The Following AWS services are related to security:

- GuardDuty:
    - Amazon GuardDuty offers threat detection that enables you to continuously monitor and protect your AWS accounts and workloads. GuardDuty analyzes continuous streams of meta-data generated from your account and network activity found in AWS `CloudTrail Events`, `Amazon VPC Flow Logs`, and `DNS Logs`. It also uses `integrated threat intelligence` such as known malicious IP addresses, anomaly detection, and `machine learning` to identify threats more accurately.
    - Priced based on the quantity of AWS CloudTrail Events analyzed (per 1,000,000 events) and the volume of Amazon VPC Flow Logs and DNS Logs analyzed (per GB).
- Security Hub:
    - AWS Security Hub provides you with a comprehensive view of your security state within AWS and your compliance with security industry standards and best practices. Security Hub `centralizes and prioritizes security and compliance findings` from across AWS accounts, services, and supported third-party partners to help you analyze your security trends and identify the highest priority security issues.
    - AWS Security Hub analyzes your security alerts, or findings, from these AWS services: Amazon GuardDuty, Amazon Inspector, and Amazon Macie. 

- Amazon Macie:
    - Amazon Macie is an ML-powered security service that helps you prevent data loss by automatically discovering, classifying, and protecting sensitive data stored in Amazon S3. Amazon Macie uses `machine learning` to recognize sensitive data such as `personally identifiable information (PII)` or `intellectual property`, assigns a business value, and provides visibility into where this data is stored and how it is being used in your organization. Amazon Macie continuously monitors data access activity for anomalies, and delivers alerts when it detects risk of unauthorized access or inadvertent data leaks.