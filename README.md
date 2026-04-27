# Headwind MDM - a platform for corporate Android applications

Headwind MDM is a Mobile Device Management platform for Android devices, designed for corporate app developers and IT managers.

(c) 2020 h-mdm.com [https://h-mdm.com](https://h-mdm.com)

## Features

 - Enrollment to Android 7+ devices through scanning a QR-code
 - Work in "Application mode" without enrollment
 - Customize the mobile desktop design and available applications
 - Automatic deployment of applications through the web panel
 - Mobile device management: groups, configurations, device status
 - Setup the available mobile device capabilities (GPS, Wi-Fi, Bluetooth etc.)
 - Manage the automatic OS update mode on the mobile device
 - Extensible platform design allowing the custom plugin development
 - Collection of application logs in the web panel
 - Centralized configuration of corporate applications

The *Enterprise edition* of the platform has more features:

 - Restriction of mobile user functions ("kid's shell" for corporate users)
 - Disable to change the mobile device settings
 - Kiosk mode (COSU, single-task mode)
 - Sending images from mobile device to server
 - Cloud-based or self-hosted server setup
 - Premium support of enterprise users
 - Custom plugin development services

The enterprise edition may be ordered on the [project website](https://h-mdm.com).

## Quick start

Headwind MDM control panel is cross-platform (it is written in Java and uses Tomcat web server). However the best OS for the deployment of Headwind MDM control panel is Ubuntu Linux.

 - Clone the project and build it (see [BUILD](#build) section for details)
 - Install the web panel to the server by using the installer script
 - Open the web panel and follow the hints to generate a QR code
 - Perform the factory reset on your Android device, tap 7 times on the welcome screen
 - Follow the instructions to scan a QR code and enroll the mobile agent

## Build

This instruction has been tested on Ubuntu Linux 22.04 LTS/24.04 LTS.

IMPORTANT: This project requires Java 21+ and Tomcat 10.1+ (Jakarta EE 9+).

1. Install required software

    For Ubuntu Linux 24.04 LTS (recommended):
    ```bash
    sudo apt install git aapt tomcat10 maven postgresql openjdk-21-jdk
    ```

    Set Java 21 as default:
    ```bash
    sudo update-alternatives --config java
    ```
    (select the Java 21 option)

2. Verify Java version

    ```bash
    java -version
    ```
    (should show version 21 or higher)

    ```bash
    mvn -version
    ```
    (should show Java 21 under "Java version")

3. Make sure Tomcat 10.1+ is running

    ```bash
    curl localhost:8080
    ```
    (if you get "Failed to connect" error, fix the installation issue)

4. Clone the repository

    ```bash
    git clone https://github.com/h-mdm/hmdm-server
    cd hmdm-server
    ```

5. If you are planning to run or debug Headwind MDM in IDE, create the properties
file from the sample

    ```bash
    cp server/build.properties.example server/build.properties
    ```

    and update the contents of the server/build.properties file.

6. Build the source code

    ```bash
    mvn install
    ```

7. Follow [INSTALL INSTRUCTIONS FOR THE WEB PANEL](#install-instructions-for-the-web-panel) below

## INSTALL INSTRUCTIONS FOR THE WEB PANEL

1. Make sure Tomcat is running

    ```bash
    telnet localhost 8080
    Trying ::1...
    Connected to localhost.
    Escape character is '^]'.
    ```

    (if you get "Connection refused" error, fix the installation issue)

2. Create the PostgreSQL database and user

    ```bash
    sudo su postgres
    psql
    postgres=# CREATE USER hmdm WITH PASSWORD 'topsecret';
    postgres=# CREATE DATABASE hmdm WITH OWNER=hmdm;
    postgres=# \q
    ```

3. Run the installer script (as root)

    ```bash
    sudo ./hmdm_install.sh
    ```

4. On success, the installer script provides you with the URL. Open Headwind MDM in browser.

## Troubleshooting

Problem: QR code isn't opening
Reason: The project URL is not accessible from the local machine.
Solution:
Make the project URL accessible from the local machine.
If you're using iptables to redirect port 80 to 8080, redirect also for loopback interface:

```bash
iptables -A OUTPUT -o lo -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 8080
```

## Contributing

Headwind MDM is a platform making corporate app development easier. We are happy to get more powerful plugins related to mobile device management.

Please contact us on the [project website](https://h-mdm.com) if you'd like to:

 - develop a public plugin for Headwind MDM
 - suggest a feature
 - order the custom development
 - report a bug
