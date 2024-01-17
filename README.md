# Azure data base migration
The following README is a description of an implementaion of a database migration using the various tools and sotwares provided by the Microsoft [Azure cloud computing services](https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize?client_id=8e0e8db5-b713-4e91-98e6-470fed0aa4c2&response_type=code%20id_token&scope=openid%20profile&state=OpenIdConnect.AuthenticationProperties%3DjZEd0hq1c0oKq_PnKRqosFjsPt1YLhnhbcLnVWsmz_wuJhb4z6iRBjsenupkL0o2scLAQMHEbpfhlY_R23zsvheNvZWuKKWEFPRnC_kQDtWKc2PVaVIYexSeyWNrE-VNjaT2oEQQH_K2HtMQdX__-ySAInJpHBKHTM-ypIkDDVUtbQzuVng2lMq7wfhB7SeNJgbI0h93fLphQG5oXQtKS3LPvb7luxrEJw2yy2PIiPvI8AmpaV4IFYnSZOOyRJdrcB9XIBGDp4jJ5ZGfyxcSO9CPT786hzj8xOZLnXvNIgSSM2uPS2dksDPHyGAxjjFkX862Ys6dWwPIh_T7LXkHJiUgi9mo9lS8YrnoKKULgpJST9gMqmde2So0xZkaHuuNb_cpeHaARt0ZLyLy_vIh2jFzsYCjUUVALc-hiRI0-LrAJuqOu78xc9BSQwRt_9GEpS_96vJckfRnTwolCFHlDcAd-zoR177TUOOVYEcCZCY&response_mode=form_post&nonce=638407554444943112.NTBiNmM4MmYtNWFiMS00MzAwLWI2MTEtNjM5MDExMTI2NGFjZWY0ZTE0YmItMDAxMy00MjUzLTk5NTItY2RkZWZlZDg1N2Jl&redirect_uri=https%3A%2F%2Fsignup.azure.com%2Fapi%2Fuser%2Flogin&max_age=86400&post_logout_redirect_uri=https%3A%2F%2Fsignup.azure.com%2Fsignup%3Foffer%3Dms-azr-0044p%26appId%3D102%26ref%3D%26redirectURL%3Dhttps%3A%2F%2Fazure.microsoft.com%2Fget-started%2Fwelcome-to-azure%2F%26l%3Den-us%26srcurl%3Dhttps%3A%2F%2Fazure.microsoft.com%2Ffree&x-client-SKU=ID_NET472&x-client-ver=6.34.0.0)

Such tools provide end users ease of use  and [highly documented](https://learn.microsoft.com/en-us/) advanced tooling and is a focal point of the following description. For a visual representation of the following description view the [Schema](#database-migration-diagram-schema) below.

# Table of Contents
- [Azure data base migration](#azure-data-base-migration)
- [Table of Contents](#table-of-contents)
- [Production enviroment set up](#production-enviroment-set-up)
  - [Provison production virtual machine (vm)](#provison-production-virtual-machine-vm)
  - [Download and install SQL Server and Microsoft SQL Server Management Studio (SSMS)](#download-and-install-sql-server-and-microsoft-sql-server-management-studio-ssms)
  - [Produce production database](#produce-production-database)
- [Migrate to Azure SQL database](#migrate-to-azure-sql-database)
  - [Create Azure SQl database](#create-azure-sql-database)
  - [Install Azure data studio](#install-azure-data-studio)
  - [Connect to Azure SQL Database](#connect-to-azure-sql-database)
  - [Schema migration](#schema-migration)
  - [Data Migration](#data-migration)
  - [Validate migration](#validate-migration)
- [Database backup and restore.](#database-backup-and-restore)
  - [Back up on-premise database](#back-up-on-premise-database)
  - [Upload back up to Blob storage](#upload-back-up-to-blob-storage)
  - [Restore database on development enviroment](#restore-database-on-development-enviroment)
  - [Automate Backups](#automate-backups)
- [Disaster recovery simulation](#disaster-recovery-simulation)
  - [Mimic data loss in prodcution enviroment](#mimic-data-loss-in-prodcution-enviroment)
  - [Restoring the data using Azure SQL database backup](#restoring-the-data-using-azure-sql-database-backup)
- [Geo replication](#geo-replication)
  - [Set up Geo-replication for Azure SQL database.](#set-up-geo-replication-for-azure-sql-database)
  - [Failover/tailback Testing](#failovertailback-testing)
    - [Test Failover/tailack](#test-failovertailack)
- [Microsoft Entra directory integration](#microsoft-entra-directory-integration)
  - [Create Azure SQL Microsoft entra Admin account](#create-azure-sql-microsoft-entra-admin-account)
  - [Create DB Reader user](#create-db-reader-user)
- [Database migration diagram schema](#database-migration-diagram-schema)


# Production enviroment set up

## Provison production virtual machine (vm)

The first step in this project, involved the migration of a database from an on-site system to using Azure cloud computing services and to use these services to set up a vm.

In cloud computing, a vm is a software-based emulation of a physical computer, allowing operating systems to run on a server. This provides end-users with the flexibility to deploy and manage various computing environments within the cloud.

To achieve this using Azure cloud computing resources, it is simple upon the creation of an Azure cloud computing account and signing into the Azure portal. What i did was simply search through the Azure resources using the "create resource" option on the portal's homepage and used the virtual machine option.

Once selected, I specified the resource group the vm is to be attached to and selected a logical name for the which this project was production-vm. Following this, I had to select a region where the vm would be hosted. Generally, it is best practice to select the region option closest to my geographical location to minimize latencies, which, for me, is the UK South option and therefore the one that was selected for this database migration. After this, I selected an image for the vm, which, for this project, was the Windows 11 Pro option. Then, I selected the size of the image, which, for this database migration, was the standard size option. Following on from this, all that is required is the specification of the username and password of the vm, followed by the network ports, which were set using RDP as I connected to the virtual machine using an Ubuntu OS and the Remmina remote desktop client.

## Download and install SQL Server and Microsoft SQL Server Management Studio (SSMS)

Following the vm generation and connection two essential softwares were required for use on the windows 11 OS to allow for database migration. These softwares are [SQL server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) and [SSMS](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16) These tools are essential for database management and migration using Azure cloud computing.

## Produce production database
The production database now can be created. This was achieved by using the downloaded [AdventureWorks2022.bak](https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms#download-backup-files) file and SSMS restore database functionality, [see](https://learn.microsoft.com/en-us/sql/relational-databases/backup-restore/quickstart-backup-restore-database?view=sql-server-ver16&tabs=ssms).


# Migrate to Azure SQL database

## Create Azure SQl database

The next step in Azure database migration was to generate a Azure SQL database. To reiterate Azure services make this simple and intuitive. Using the Azure portal a Azure SQL database can be created by selecting the Azure SQL database resource from the create a resource option from the home portal. 

Through this process you have to generate a linked SQL server when doing this it is essential that SQL login are the choosen authentication method and firewall settings on the generated SQL database are set as such that the vm IP address is added to the SQL server generated. This was achieved through Azure SQL Server set up menus.

## Install Azure data studio
In order to complete a database migration [Azure data studio](https://learn.microsoft.com/en-us/azure-data-studio/download-azure-data-studio?tabs=win-install%2Cwin-user-install%2Credhat-install%2Cwindows-uninstall%2Credhat-uninstall) must be downloaded and installed. By doing this a server connection can be created between Azure data studio and the SQL Server hosted on the vm connection must also be made to Azure SQL server database generated in the steps described [above](#create-azure-sql-database).

## Connect to Azure SQL Database
Azure Data Studio allows for connection to SQL Databases on local machines/vms as well as those running on Azure cloud services. This achieved by simple running the Azure data studio software and connecting to server using the SQL login credentials set when creating the SQL server, [see here for further details.](https://learn.microsoft.com/en-us/azure-data-studio/quickstart-sql-server)

## Schema migration 
With connections created using Azure data studio it was now possible to proceed with schema migration. To achieve this the  [SQL Server Schema Compare extension](https://learn.microsoft.com/en-us/azure-data-studio/extensions/schema-compare-extension) had to be downloaded through Azure data studios extension marketplace.

 Once succesfully installed schema comaprison/migration is a easy and right clicking on site (vm) hosted database and selecting the Schema compare option. This create the schema compare GUI with the production-vm database set as the source. Now the target database must choosen which of course is the Azure SQL production database stored on the azure SQL Server. Following this it was simple as selecting the compare option on the GUI and then migrating the schema to the Azure SQL database.

 ## Data Migration
With correct schema migration the actual data can be migrated appropriately. In order to achieve this another Azure data studio Extension has to installed. Specifically the [Azure SQL Migration extension](https://learn.microsoft.com/en-us/azure-data-studio/extensions/azure-sql-migration-extension) Using this extension, migration of the data was a simple a right clicking on the on site datbase connection within the Azure data studio and selecting the Manage and then under Genral the Azure SQL migration. This will open the Azure migration Wizard. [Simply follow the steps for succesful migration.](https://learn.microsoft.com/en-us/azure/dms/migration-using-azure-data-studio?tabs=azure-sql-mi)    


## Validate migration
With any migration a visual/programmtic assessment of the migration is neccesary. I achieved this by codnucting some sanity checks such as comparing the number of tables in each database and if the there any loss of rows of columns within tables.

# Database backup and restore.

## Back up on-premise database
With any Database migration it is always useful to generate backup copies of the database/'s you are working with. The database can be backed to a .bak file using SSMS. This was achieved by selecting the Adventure works database stored on the Azure vm (on site) and right clicking and selecting the Back up option. Now using the Backup GUI dialog boxes a full restore was selected and the to disk option also selected and using the backup folder default to store the .bak file within the on site system. With these options succesfully specified Ok is then selected. SSMS made me aware of succesful backup of the database choosen.

## Upload back up to Blob storage
After creating a disk based backup of the production databas eon the prodcution vm, it was also essetnial to take adavantage of Azure cloud based [blob storage](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-overview). This is a Azure stoarge type that allows for storge of large amount of data. To use blob storage in this project I had to generate and Azure storage account using the Azure portal search bar.Directing me to the Storage accounts homepage. Follwing from here I simple had to Use *Create*. This led to selecting the linked subcrition and resource group. Then the storage account was named and given region (Uk South). From the storage account page I was able to genrate a container. Then from the container page I added a blob and then uploaded the AdeventureWorks.bak file from the production vm disk.

## Restore database on development enviroment

When doing anything in software and tech devlopment/test enviroments are incredibly useful for product development without disturbing production enviroments.

As such the next step of the Database migration process engaged with was to generate a development enviroment. This was achieved by repeating the steps in [ Production enviroment set up](#production-enviroment-set-up). Once the the devlopment vm was created and had SQL server and SSMS installed i was able to prodcue the development database. This was achieved dowloading the .bak from the Azure stroage container from there I connected t o the develpment database and used the back up funtionaltiy of SSMS desribed [above](#back-up-on-premise-database)

## Automate Backups

Using the back up vm I created a automated backups. This was achieved using SSMS and connecting to the Development databses on the devleopment vm using the  SQL Server Agent node within the devlopemnt databse. From here I had to geneat SQL server credentials. Now under mangement it was possible to create a maintentance plan with maintenance plan wizard.   A name was given to plan. The schedule was set to weekly with a full databse back up selected also. Now Back Up Database Task  was selected URL as backup location. The Destiantoion i then set with the SQL credentials in drop down option. Next the provisoned storaged container was selected with the backup file set to bak. nOw can clyle to final page of the Mingenca eplan wizard to creat the plan. Within SSMS the maintenance plan is visble and thus working.

# Disaster recovery simulation
Data loss is a real world threat to Businesses and industries. Within industry testing and simulation of such issues arising is standard practice. However, Azure Cloud comptuing offers recovery functionality for its SQL databases that make data recovery simple.

## Mimic data loss in prodcution enviroment
To use this functionaltiy within this database nmigration I conducted a simple form of data loss simulation. Specifically in this project the loss of whole table of data was caused within the production database. This was achieved through the use of connection to prodcution database and running the SQL query of

``` DROP TABLE dbo.AWBuildVersion; ```

This represents the loss of huge amount of critical data. But, as shown below using Azure it is possible to recover this data.

## Restoring the data using Azure SQL database backup

Recovery of the data was achieved by using azure portal and selecting the Azure Sql database resource. From the databses homepage I selected the *Restore option. This opens the Restore database window. Where I selcted to restore the dabase to two hours before as restore point. Finally a name has to be given to the the database which for this project was adventureworksreplica. Once the databse has been providoned you can see it in the avaible resources on the Azure home portal. To assess if the restoration has worked I used Azure data studio to conect to the database and check if the dbo.AWBuildVersion table was present and thuis restored. The replcia databse is now acts as the production database and thus the prodcution data base is deleted to avoid confusion when using the Azure portal.


# Geo replication
Geo Replication with Azure SQL databases is a powerful disaster recovery feature. It works by simple replicating the determined data base in a seperate geographical location. Protecting from data loss in the primary geographical location.

## Set up Geo-replication for Azure SQL database.
Acheiving this with Azure services is fairly simple. I achieved it for this databse migration after genearting the restored SQL database. Selecting the the restored SQL database from the Azure portal. I selected the Replicas options under the data managment section. This takes you to another GUI with the options to generate a new SQL Server Where I specified the name and location of this new. Server for Geo Replication a different Region to the Intial Primary server must be selected. In my case i chose East US. 

Again SQL Authetication has to set up. Ok all this using the OK button and then select review + create and then create to generate the replication. Uisng the resource page succesful replcation can be checked by reviewing is the replcation has an associated primary database.

## Failover/tailback Testing
When generating a geo replicate it is important to test the system using a failover (awtiching the workload from primary region to the secondary region) and then a tailback (the reversion from the workload back to primary region after succesful failover)


### Test Failover/tailack
To do this I used the Azure SQL server from Azure portal and selcted the Failover gropus from the data manamgemnt options and add group funcitonality in failover groups page. The failover group was then named and and secondary server (Replica sever generated above) Set in the East Us choosen and finally generated by using Create. From the Portal I navigated to the replica server and access the failover group page and then select Failover options. If succesufl the primary and secondary databse labels should be switched. The tailback was schieved by simple selecting failover option again and noting the switch in the databases primary and secondary status.


# Microsoft Entra directory integration
The following step decription is about using the availible tooling of Microsoft Azure to increase the security around the databases we have migrated on the Azure SQL database services. Using Microsoft Entra ID.

## Create Azure SQL Microsoft entra Admin account
Using the Microsoft Azure portal I navigated to my replica SQL server, whilst first noting we were in the correct geo-region (South UK). Under the security options I selected Microsoft entra id. This sennt me to portal page that allowed for set admin functionality. Using this option going from Users and selecting myself among them. Setting me as the server admin. From this I tested the Entra credentials by using Azure data studio to connect to the server.

## Create DB Reader user

The final step of the project was to generate DB reader user. To do this I navigated to the Azure portal and navigate to the Microsoft Entra ID homepage and created a new user (with a suggestive name). The new user must be given a password. With a connection to SQl database from [step above](#create-azure-sql-microsoft-entra-admin-account) using microsfot entra credentials. Then using SQL Query to grant the db_datareader user role.

The read user connection needs to be checked. This is achieved by editing the connectiomn to SQL Server opening Connection page and under Account section  select to Add account. Following the set up process  i connected to server using this new account to observe the Read only access behaviour of this account. 


- [Table of Contents](#table-of-contents)

# Database migration diagram schema
![Azure Project Diagram](https://github.com/HPCurtis/azure-database-migration797/assets/136170998/7b809b53-335a-465c-894d-e08ef6be291e)

