# Azure-Data-Engineering-Project-Vehicle-IoT-Data-Pipeline
Building End-to-end automated Azure Data Pipeline for moving Vehicle IoT Data from AWS S3 to Azure database using various Azure services like ADLS, Key Vault, Azure Data Factory, Azure Function and Azure SQL Database

Vehicles Data
Vehicle has third party loT device which will send the telemetry data (in JSON format) over the AWS cloud. You need to move data from third party AWS to client's Azure cloud.
· You need to validate the JSON sometime it could be incomplete or wrong JSON which need to be rejected.
· Once JSON got validated this data would be stored in the SQL database which will be further utilized by data science team.

**Overview:**

![Pipeline Overview](https://github.com/Akash743/Azure-Data-Engineering-Project-Vehicle-IoT-Data-Pipeline/assets/57750483/7b9e32ae-b9e8-4617-8af6-4ba1a2d38af7)

**Pipeline:**

![Pipeline](https://github.com/Akash743/Azure-Data-Engineering-Project-Vehicle-IoT-Data-Pipeline/assets/57750483/80dab8a1-6bc9-4be0-b8ad-1ba1a15d0543)


**Steps:**
1. Create S3 bucket: We're assuming that IoT data from Vehicle will be uploaded here on daily basis. This S3 data is to be transferred to the Azure database. Make nested folders like outer most IoTData, then for current Year, then Month and then respective Date
2. Create Azure Key Vault: To store secret keys to access S3 bucket from Azure
3. Create Storage Account and Container in Azure to receive data from AWS S3: This will store the data within Containers. Would need one Resource Group creation first.
