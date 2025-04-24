# UniTime Installation Guide for Windows
**Version: 4.8**

This guide walks you through the process of installing UniTime on a Windows system.

## Table of Contents
- [UniTime Installation Guide for Windows](#unitime-installation-guide-for-windows)
  - [Table of Contents](#table-of-contents)
  - [1. Install Java 21](#1-install-java-21)
  - [2. Install MySQL 8.4](#2-install-mysql-84)
  - [3. Install Apache Tomcat 9.0](#3-install-apache-tomcat-90)
  - [4. Install MySQL JDBC driver](#4-install-mysql-jdbc-driver)
  - [5. Install UniTime](#5-install-unitime)
    - [5a. Create and populate the timetable database](#5a-create-and-populate-the-timetable-database)
    - [5b. Deploy the UniTime application](#5b-deploy-the-unitime-application)
    - [5c. Create UniTime custom properties file (optional step)](#5c-create-unitime-custom-properties-file-optional-step)
  - [6. Restart Tomcat](#6-restart-tomcat)
  - [7. Check the logs for any errors (optional step)](#7-check-the-logs-for-any-errors-optional-step)

## 1. Install Java 21

1. Download JDK 21 from [Oracle's website](https://www.oracle.com/java/technologies/downloads) (file named like `jdk-21_windows-x64_bin.exe`)
2. Run the installer and follow the installation instructions:
   - Click "Next" on the initial screen
   - Accept the default installation location
   - Wait for installation to complete
   - Click "Close" when finished

## 2. Install MySQL 8.4

1. Download MySQL Community Server from the [MySQL download page](https://dev.mysql.com/downloads/mysql) (file named like `mysql-8.4.0-winx64.msi`)
2. Run the installer and follow these steps:
   - Click "Next" on the initial screen
   - Accept license terms, click "Next"
   - Choose "Typical" installation type
   - Click "Install"
   - When installation completes, keep "Run MySQL Configurator" checked and click "Finish"
   
3. Configure MySQL:
   - In the MySQL Configurator, click "Next"
   - Accept the default configuration type, click "Next"
   - Accept the default settings on the next screen, click "Next"
   - Set a MySQL Root Password (keep this password secure), click "Next"
   - Accept the default Windows service settings, click "Next"
   - Accept the default server configuration, click "Next"
   - Accept the default Apply Configuration settings, click "Next"
   - Click "Execute" to apply the settings
   - When configuration completes, click "Next" and then "Finish"

## 3. Install Apache Tomcat 9.0

> **Note:** Tomcat 10 or later are currently not supported!

1. Download Apache Tomcat 9.0 from the [Apache Tomcat website](https://tomcat.apache.org/download-90.cgi) (choose the 32-bit/64-bit Windows Service Installer option, file named like `apache-tomcat-9.0.90.exe`)
2. Run the installer and follow these steps:
   - Click "Next" on the initial screen
   - Accept the license agreement by clicking "I Agree"
   - Accept the default components, click "Next"
   - Accept the default installation location, click "Next"
   - Leave the default port settings, click "Next"
   - Leave Java path as detected, click "Next"
   - Click "Finish" when installation completes

3. Configure Tomcat memory settings:
   - Open Apache Tomcat configuration (Start > All Programs > Apache Tomcat 9.0 > Configure Tomcat)
   - If the menu option doesn't work, open File Explorer and navigate to `C:\Program Files\Apache Software Foundation\Tomcat 9.0\bin`, then run `Tomcat9w`
   - Select the "Java" tab
   - Change the maximum memory pool to at least 1024 MB
   - Click "Apply" and "OK"

## 4. Install MySQL JDBC driver

1. Download MySQL Connector/J driver from the [MySQL Connector download page](https://dev.mysql.com/downloads/connector/j/) (choose Platform Independent ZIP Archive, file named like `mysql-connector-j-8.4.0.zip`)
2. Extract the downloaded zip file
3. Copy the `mysql-connector-j-8.4.0.jar` file from the extracted folder to `C:\Program Files\Apache Software Foundation\Tomcat 9.0\lib`

## 5. Install UniTime

1. Download UniTime from the [UniTime GitHub releases page](https://github.com/UniTime/unitime/releases/latest) (file named like `unitime-4.8_bld139.zip`)
2. Extract the downloaded zip file

### 5a. Create and populate the timetable database

1. Open Command Prompt (Start > All Programs > Accessories > Command Prompt)
2. Execute the following commands:

```
mysql -u root -p -f <Downloads\unitime-4.8_bld139\doc\mysql\schema.sql
mysql -u root -p <Downloads\unitime-4.8_bld139\doc\mysql\woebegon-data.sql
```

3. When prompted, enter the MySQL root password you created during MySQL installation

> **Note:** If you receive an error message stating "mysql is not recognized as an internal or external command, operable program or batch file", use the full path to the MySQL executable:
> ```
> "c:\Program Files\MySQL\MySQL Server 8.4\bin\mysql.exe" -u root -p -f <Downloads\unitime-4.8_bld139\doc\mysql\schema.sql
> "c:\Program Files\MySQL\MySQL Server 8.4\bin\mysql.exe" -u root -p <Downloads\unitime-4.8_bld139\doc\mysql\woebegon-data.sql
> ```

### 5b. Deploy the UniTime application

Copy the `web\UniTime.war` file from the extracted UniTime archive to `C:\Program Files\Apache Software Foundation\Tomcat 9.0\webapps`

### 5c. Create UniTime custom properties file (optional step)

1. Create a file named `unitime.txt` in `C:\Program Files\Apache Software Foundation\Tomcat 9.0\conf\` (you can use File Explorer to create a new text document in this location)
2. Open `catalina.properties` in the same folder
3. Add the following line to the file:
   ```
   tmtbl.custom.properties=C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0\\conf\\unitime.txt
   ```
4. Save and close the file

For more details on customization, see the [UniTime documentation](http://help.unitime.org/installation#customization).

## 6. Restart Tomcat

1. Open Apache Tomcat configuration (Start > All Programs > Apache Tomcat 9.0 > Configure Tomcat)
2. On the General tab:
   - If Tomcat is already running, click "Stop"
   - Once stopped, click "Start"
   - (Optional) Change the Startup type to "Automatic" if you want Apache Tomcat to start automatically when Windows starts
3. Click "OK" to close the dialog

You should now be able to access UniTime at http://localhost:8080/UniTime

Login using the default admin credentials:
- Username: `admin`
- Password: `admin`

## 7. Check the logs for any errors (optional step)

The log files are located in `C:\Program Files\Apache Software Foundation\Tomcat 9.0\logs`

- UniTime-specific messages are stored in `unitime.log`
- Other useful log files include:
  - `catalina.<DATE>.log`
  - `tomcat9-stderr.<DATE>.log`
  - `tomcat9-stdout.<DATE>.log`

If everything started correctly, you should see startup messages in the `unitime.log` file.

