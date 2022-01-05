## **Connecting dataiku to Oracle Autonomous Database**

This guide shows you how to configure dataiku connectivity to Oracle Autonomous Database (ADB).

These instructions use JDBC Thin driver from Oracle.



| Validation Matrix  | Version  |
| ------------- | ------------- |
| BotSupply Insights  | 1.0.0  |
| Oracle Client  | 19.6  |




## **Prerequisites**

- This document assumes that ADB, such as Autonomous Data Warehouse (ADW) or Autonomous Transaction Processing (ATP), or Autonomous JSON Database (AJD) is provisioned and Dataiku is installed on a machine (local, OCI, or other cloud).   To provision ADB, see [here](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/autonomous-provision.html#GUID-0B230036-0A05-4CA3-AF9D-97A255AE0C08).

- Oracle JDBC Thin driver is downloaded and configured.  Specific instructions on installing the jar file to dataiku are [here](https://doc.dataiku.com/dss/latest/connecting/sql/oracle.html#installing-the-jdbc-driver).
- ADB Wallet is downloaded on your machine running dataiku.

## **Configure the Connection**
## Step 1:  Provision ADWC and Configure Oracle Client on ISV  

1. Provision Autonomous Data Warehouse Cloud (ADWC) and download the corresponding credentials.zip file to the system that will have the BotSupply Insights installation and also get password of ADMIN for the ADWC instance. For the Oracle documentation to provision ADWC click here. Also check [Downloading Client Credentials (Wallets)](https://doc.dataiku.com/dss/latest/connecting/sql/oracle.html#installing-the-jdbc-driver).

2. All connections to Autonomous Data Warehouse Cloud use certificate-based authentication and Secure Sockets Layer (SSL). Uncompress credentials.zip file into a secure folder.

3. Download the Oracle Database Client to the system where BotSupply Insights is installed.   Validate that the Oracle Database Client can communicate with ADWC.

4. Create the TNS_ADMIN environment variable and set it to the location of the secure folder containing the credentials file you saved in Step 3

5. Edit the sqlnet.ora file, replacing “?/network/admin” with the name of the folder containing the client credentials. The location of folder is stored in TNS_ADMIN environment variable so use that variable in sqlnet.ora For example:

    ```
    WALLET_LOCATION = (SOURCE = (METHOD = file) (METHOD_DATA = (DIRECTORY=${TNS_ADMIN}))) SSL_SERVER_DN_MATCH=yes
    ```

6. The tnsnames.ora file provided with the credentials zip file contains three database service names identifiable as high, medium and low. The predefined service names provide different levels of performance and concurrency for Autonomous Data Warehouse Cloud. Use one of these service names in your ConnectString.

7. Test the Oracle Client with oracledb NPM library

    ```
    try{ let connection = await oracledb.getConnection({     user: "admin",     password: adminPassword,     connectString   }); } catch(err){  console.log(err); }    If the connection is successful you are ready to move to the next step.
    ```

## Step 2. Install BotSupply Insights To install  BotSupply Insights software:  

### Dependencies:
1. MongoDB
2. Node >v12
3. NPM
4. OS Used: Ubuntu 16.04  

### Steps:
1. Copy example.env to .env and fill proper details.
2. Install pm2 globally with npm install -g pm2.
3. Install dependencies with npm install.
4. Run npm run start from ADW/api directory.
5. For subsequent runs use npm run restart.






## **Acknowledgements**
* **Author(s)** - Vijay Balebail, Director Database Product Management
* **Contributor(s)** -
* **Last Updated By/Date** - Rajeev Rumale, January 2022
