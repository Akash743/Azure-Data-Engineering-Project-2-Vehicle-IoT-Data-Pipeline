# Azure-Data-Engineering-Project-Vehicle-IoT-Data-Pipeline
Building End-to-end automated Azure Data Pipeline for moving Vehicle IoT Data from AWS S3 to Azure database using various Azure services like ADLS, Key Vault, Azure Data Factory, Azure Function and Azure SQL Database

Vehicles Data
Vehicle has third party loT device which will send the telemetry data (in JSON format) over the AWS cloud. You need to move data from third party AWS to client's Azure cloud.
· You need to validate the JSON sometime it could be incomplete or wrong JSON which need to be rejected.
· Once JSON got validated this data would be stored in the SQL database which will be further utilized by data science team.


![Pipeline](https://github.com/Akash743/Azure-Data-Engineering-Project-Vehicle-IoT-Data-Pipeline/assets/57750483/80dab8a1-6bc9-4be0-b8ad-1ba1a15d0543)
