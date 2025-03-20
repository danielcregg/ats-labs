# How To Install Matomo Analytics Server

## Introduction

Matomo is an open-source web analytics application. Please note, it was formerly called Piwik, so you may see some references to the old name. It rivals Google Analytics and includes even more features, allowing you to send out custom daily, weekly, and monthly reports to your clients. Here is a video to explain: *Your introduction to Matomo Analytics!*  

<div align="center">
  <a href="https://www.youtube.com/watch?v=Qc2kooLNDiU">
    <img src="https://img.youtube.com/vi/Qc2kooLNDiU/maxresdefault.jpg" alt="Video" style="width:50%;">
  </a>
</div>

## Downloading and Unzipping Matomo

First, update your system packages to ensure you have the latest versions:

```bash
sudo dnf update
```

Install the required tools for downloading and extracting Matomo:

```bash
sudo dnf install -y wget unzip
```

Download the latest version of Matomo:

```bash
sudo wget https://builds.matomo.org/matomo.zip -P ~
```

Extract the downloaded Matomo zip file to your web server directory:

```bash
sudo unzip -o ~/matomo.zip -d /var/www/html
```

## Assigning Permissions

Set the appropriate ownership for the Matomo directory:

```bash
sudo chown -R apache:apache /var/www/html/matomo
```

Set the required permissions for the temporary directory:

```bash
sudo chmod -R 0755 /var/www/html/matomo/tmp
```

Restart the Apache web server to apply the changes:

```bash
sudo systemctl restart httpd
```

## Creating the Database

Now we need to create a database for our analytics server. We will call the database `matomodb`. Run the following commands exactly, **one command at a time**:

First, access the MySQL command line interface:

```bash
sudo mysql
```

Create a new database for Matomo:

```bash
CREATE DATABASE matomodb;
```

Create a database user for Matomo:

```bash
CREATE USER 'matomoadmin'@'localhost' IDENTIFIED BY 'password';
```

Grant all privileges on the Matomo database to the user:

```bash
GRANT ALL PRIVILEGES ON matomodb.* TO 'matomoadmin'@'localhost';
```

Apply the privilege changes:

```bash
FLUSH PRIVILEGES;
```

Exit the MySQL command line:

```bash
exit;
```

## Configuration via GUI

Browse to your matomo server dashboard by going to:

```
https://yourwebsiteaddress/matomo
```

Click *Next* and you will run through the system check. Everything should pass; if not, make sure LAMP is properly installed and configured.
![alt text](img/matomo-system-check.png)
Now you will configure the database you already set up. Just use the appropriate credentials and then click *Next*. For password, use `password`.
![alt text](img/matomo-database-setup.png)
Upon successful database setup, you should see a confirmation screen.
![alt text](img/matomo-db-setup-success.png)
Next, create an admin user. Call the **Super user login** `admin` and take note of your password. Put your email address in the email box.
![alt text](img/matomo-admin-creation.png)
Now we need to add our website. Fill out the following fields.
![alt text](img/matomo-website-setup.png)
Finally, read through the setup summary, untick the unnecessary boxes, and hit *Continue to Matomo*.
![alt text](img/matomo-setup-summary.png)
Read the below page untick the boxes and hit "Continue to Matomo". 
![alt text](img/matomo-final-setup.png)
## Logging In

Log in with your super user account using username as **admin** and your password.
![alt text](img/matomo-login.png)
Next Click on the **INSTALL WITH WORDPRESS** button.
![alt text](img/matomo-wp-install-button.png)
Follow the instructions that appear:
![alt text](img/matomo-wp-instructions.png)
## WordPress Plugin Installation

Next, go to the *Plugins* section on your WordPress dashboard and install the **WP-Matomo** plugin. Click *Install Now*, wait for the *Install Now* text to change to *Activate*, then click *Activate*.
![alt text](img/wp-matomo-plugin-install.png)
Once you have activated the Matomo Plugin, head over to **Settings > WP-Piwik** and fill out your server information. We will get the Auth Token in the next step.
![alt text](img/wp-matomo-settings.png)
Login to Matomo:

```
http://YOUR_IP_ADDRESS/matomo/index.php
```

Use the **Admin username** and the **Admin password** you set earlier. Select the **cogwheel icon** on the top right of the toolbar.
![alt text](img/matomo-cogwheel-menu.png)
Then select **Security** on the left-hand menu under the **Personal** section. Click on the **CREATE NEW TOKEN** button under the *Auth tokens* section.
![alt text](img/matomo-security-page.png)
Enter your admin password and click *Confirm*.
![alt text](img/matomo-password-confirm.png)
Fill in the description, then hit *CREATE NEW TOKEN*.
![alt text](img/matomo-token-create.png)
Your new token should now appear. Copy it and hit the big green *Confirm* button.
![alt text](img/matomo-token-copy.png)
Copy that token over to your WordPress site and click *Save Changes*.
![alt text](img/wp-matomo-token-save.png)
## Configuring WordPress Dashboard Settings

Configure the settings you would like to show on your dashboard and click *Save Changes*.
![alt text](img/wp-matomo-dashboard-settings.png)
Now, you need to enable the tracking. Select **Default tracking**, change any settings you'd like, and hit *Save*.
![alt text](img/wp-matomo-tracking-settings.png)
Go to your WordPress dashboard, and you will notice extra Matomo panels.
![alt text](img/wp-matomo-dashboard-panels.png)
If you go to your Matomo dashboard by opening a browser and navigating to:

```
http://YourServerIPaddress/matomo/
```

You will see information about your website. Initially, it will be blank, waiting for new stats to be generated. Wait for an hour and then check the Matomo dashboard again.
![alt text](img/matomo-dashboard.png)
### Congratulations! 🎉

You have installed Matomo on your Amazon Linux 2023 web server and configured the Matomo plugin on your WordPress site. Now, get three hits on your site—use your phone, desktop, and ask a friend to connect. Search the results and see what information you can find.

## Troubleshooting

If you are having issues, delete the database and database users and start again. **DO NOT RUN THE FOLLOWING COMMANDS IF EVERYTHING IS WORKING**:

Access the MySQL command line as the root user:

```bash
sudo mysql
```

View all MySQL users:

```bash
SELECT User FROM mysql.user;
```

Delete the Matomo database user:

```bash
DROP USER 'matomoadmin'@'localhost';
```

List all databases:

```bash
SHOW DATABASES;
```

Delete the Matomo database:

```bash
DROP DATABASE matomodb;
```

Apply the privilege changes:

```bash
FLUSH PRIVILEGES;
```

Exit the MySQL command line:

```bash
exit;
```

## Installation Workflow Diagram

```mermaid
graph TD;
    A[Start] -->|Download Matomo| B(Extract Files);
    B -->|Set Permissions| C(Restart Apache);
    C -->|Create Database| D(Configure Matomo GUI);
    D -->|Create Admin Account| E(Connect Website);
    E -->|Install WP-Matomo Plugin| F(Configure Plugin);
    F -->|Enable Tracking| G[Matomo Dashboard Ready];
    G -->|Analyze Traffic| H[Success!];
```

## Resources

- [Matomo Official Documentation](https://matomo.org/docs/installation/)
- [Matomo Website](https://matomo.org/)
- [PHP Manual](http://php.net/manual/en/intro.mbstring.php)

