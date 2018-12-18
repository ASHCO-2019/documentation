# Server Configuration for EspoCRM

EspoCRM can be installed on the Apache ([instructions](apache-server-configuration.md)), Nginx ([instructions](nginx-server-configuration.md)), or IIS server with support PHP version 5.6 or greater and MySQL version 5.5.3 or greater.

## Configuration Recommendations

### PHP Requirements

EspoCRM requires PHP 5.6 or greater, with the following extensions enabled:

* [pdo](http://php.net/manual/en/book.pdo.php) – to access MySQL in PHP;
* [json](http://php.net/manual/en/book.json.php) – resources use this format (metadata, layout, languages, and others);
* [gd](http://php.net/manual/en/book.image.php) – to manipulate images;
* [openssl](http://php.net/manual/en/book.openssl.php) – to ensure the highest protection;
* [zip](http://php.net/manual/en/book.zip.php) – to be able to upgrade EspoCRM and install extensions;
* [imap](http://php.net/manual/en/book.imap.php) – to monitore mailboxes in EspoCRM;
* [mbstring](http://php.net/manual/en/book.mbstring.php);
* [iconv](http://php.net/manual/en/book.iconv.php);
* [curl](http://php.net/manual/en/book.curl.php) – for integrations;
* [xml](http://php.net/manual/en/book.xml.php) - for excel export;
* [xmlwriter](http://php.net/manual/en/book.xmlwriter.php) - for excel export;
* [exif](http://php.net/manual/en/book.exif.php) – for a proper oriantion of uploaded images.

php.ini settings:

```
max_execution_time = 180
max_input_time = 180
memory_limit = 256M
post_max_size = 50M
upload_max_filesize = 50M
```

### Database Requirements

EspoCRM supports MySQL version 5.5.3 or greater. MariaDB is supported as well. These are no special peculiarities. All default settings are fine for EspoCRM.

For better work it's recommended to use MySQL 5.6 of greater.

### MySQL 8 Support

MySQL 8.0.4 has changed a default authentication method to caching_sha2_password which is not supported by PHP (at the time of writing). For MySQL 8 it should be changed to mysql_native_password method. For a user it can be done with the query:

```
CREATE USER username@localhost identified with mysql_native_password by 'password';
```
where username is your MySQL user, password is your MySQL user password.

## Required Permissions for Unix-based Systems

The files and directories should have the following permissions:

* `/data`, `/custom`, `/client/custom` – should be writable all files, directories and subdirectories (664 for files, 775 for directories, including all subdirectories and files);
* `/application/Espo/Modules`, `/client/modules` – should be writable the current directory (775 for the current directory, 644 for files, 755 for directories and subdirectories);
* All other files and directories should be readable (644 for files, 755 for directories).

To set the permissions, execute these commands in the terminal:

```
cd <PATH-TO-ESPOCRM-DIRECTORY>
find . -type d -exec chmod 755 {} + && find . -type f -exec chmod 644 {} +;
find data custom client/custom -type d -exec chmod 775 {} + && find data custom client/custom -type f -exec chmod 664 {} +;
chmod 775 application/Espo/Modules client/modules;
```

All files should be owned and group-owned by the webserver process. It can be “www-data”, “daemon”, “apache”, “www”, etc.  

Note: On Bitnami Stack, files should be owned and group-owned by “daemon” user.  

Note: On shared hosts, files should be owned and group-owned by your user account.

To set the owner and group-owner, execute these commands in the terminal:

```
cd <PATH-TO-ESPOCRM-DIRECTORY>
chown -R <OWNER>:<GROUP-OWNER> .;
```

## Setup a crontab

To setup a crontab on a UNIX system, take the following steps:

1. Login as administrator into your EspoCRM instance.
2. Go to the Scheduled Jobs section in the administrator panel (Menu > Administration > Scheduled Jobs) and copy the string for the crontab. It looks like this one:
```
* * * * * /usr/bin/php -f /var/www/html/espocrm/cron.php > /dev/null 2>&1
```
3. Open a terminal and run this command:
```
crontab -e -u WEBSERVER_USER
```
WEBSERVER_USER can be one of the following “www”, “www-data”, “apache”, etc (depends on your webserver).
4. Paste the copied string (from step 2) and save the crontab file (Ctrl+O, then Ctrl+X for nano editor).

## Configuration instructions based on your server

* [Apache server configuration](apache-server-configuration.md).
* [Nginx server configuration](nginx-server-configuration.md).
