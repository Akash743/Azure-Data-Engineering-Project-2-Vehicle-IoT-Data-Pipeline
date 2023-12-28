# Azure-Data-Engineering-Project-Vehicle-IoT-Data-Pipeline
Building End-to-end automated Azure Data Pipeline for moving Vehicle IoT Data from AWS S3 to Azure database using various Azure services like ADLS, Key Vault, Azure Data Factory, Azure Function and Azure SQL Database

Vehicles Data
Vehicle has third party loT device which will send the telemetry data (in JSON format) over the AWS cloud. You need to move data from third party AWS to client's Azure cloud.
· You need to validate the JSON sometime it could be incomplete or wrong JSON which need to be rejected.
· Once JSON got validated this data would be stored in the SQL database which will be further utilized by data science team.

**As soon as any JSON file gets uploaded in the AWS S3 folder named on that day's date, it should get transferred into ADLS and Azure SQL database after proper validation**

**Overview:**

![Pipeline Overview](https://github.com/Akash743/Azure-Data-Engineering-Project-Vehicle-IoT-Data-Pipeline/assets/57750483/7b9e32ae-b9e8-4617-8af6-4ba1a2d38af7)

**Pipeline:**

![Pipeline](https://github.com/Akash743/Azure-Data-Engineering-Project-Vehicle-IoT-Data-Pipeline/assets/57750483/80dab8a1-6bc9-4be0-b8ad-1ba1a15d0543)


**Steps:**
1. **Create S3 bucket:** We're assuming that IoT data from Vehicle will be uploaded here on daily basis. This S3 data is to be transferred to the Azure database. Make nested folders like outer most IoTData, then for current Year, then Month and then respective Date
2. **Create Azure Key Vault:** To store secret keys to access S3 bucket from Azure
3. **Create Storage Account and Container in Azure**: To receive data from AWS S3: This will store the data within Containers. Would need one Resource Group creation first. Create 'Landing Folder' which will be the destinaton for all JSON files coming from AWS S3.
4. **Create ADF account:** Will use ADF to build pipelines for data transfer
5. **Create Key Vaults:** To store secrets for accessing files stored in S3. Go to 'Secrets' and store the Access key Id and Acess Key Secret of S3. Add Access Policy to the Key Vault. This will enble the ADF account to access Keys stored
6. **Create Ingestion Pipeline in Data Factory:**
   - Create Linked Service for connecting ADF to S3
   - Create Linked Service for conneting ADF to ADLS account
   - Create Pipeline in ADF to connect S3 to ADLS: Using copy activity, provide Source (S3) and Sink (ADLS Landing Folder). We want data to be stored dynamically in the ADLS based on date similar to how it got saved in AWS. So, while saving Source and Sink folder paths, will add 'dynamic content' with folder path
     
        - Parameter in Source side: @concat(formatDateTime(utcnow(),'yyyy'),'/',formatDateTime(utcnow(),'MM'),'/',formatDateTime(utcnow(),'dd'),'/')
        - Parameter in Sink side: @concat('landing/',formatDateTime(utcnow(),'yyyy'),'/',formatDateTime(utcnow(),'MM'),'/',formatDateTime(utcnow(),'dd'),'/')
        - Example: File saved in Landing would be in: iotdataFolder>>2023>>12>>28
        - Also, check wildcard file path to ensure it reads all files present in the folder location
7. **Create Azure Function with Blob Trigger Logic:** To read file from Landing Folder as soon as any file is created there. Valdate the JSON file andsave it in 'Staging' Folder if its a valid JSON else in 'Rejected' Folder. Create function by selecting the Resource Group, Language and template as 'Azure Blob Storage Trigger'(Trigger) which will ensure that the function runs whenever a blob is added to a specified contaner.
   - Add 'Landing' folder path to this Tigger
   - Add 'Staging' & "Rejected' folder as function's output destination based on the validation. Add destination file format as JSON
   - Add code logic in function
  
8. **Create SQL DB**: Select a server. This sever can be used for multiple databases. In Network Connectity, choose Public Endpoint as of now. In Firewall Rules, allow Azue Services & Resources to access this server
9. **Create ADF Pipeline to move data from ADLS Staging folder to SQL DB**:
   - Create Linked Service for connecting to DB. Create Table in DB
     
          create table [dbo].[VehicleData1] (
     
          VehicleID nvarchar(100),
     
          latitude decimal,
     
          longitude decimal,
     
          city nvarchar(100),
     
          temperature int,
     
          speed int
     
          )
