# generatedata.com OpenShift installation

Steps to deploy the website code from benkeen/generatedata as microservices in Red Hat OpenShift Container Platform.  Some modifcations are required, because unlike Docker where container deployments run as root by default, OpenShift securely runs containers using non-root credentials.

## Microservice design considerations
- httpd container (website)
- php-fpm container (php)
- mysql container (database)

Containers for the website and php-fpm require shared access to html and php code files on a persistent volume

Mysql database uses persistent storage for the database

## Components
- Persistent Volume/Persistent Volume Claim for mysql database
- Persistent Volume/Persistent Volume Claim for shared access to php/web files
- webserver Pod (image: nginx:alpine)
- php-fpm Pod (image: bitnami/php-fpm:7.4)
- mysql Pod (image: mysql:8.0)
- configMap containing the following data:
  - nginx.conf (replaces image-supplied config)
  - default.conf (replaces image-supplied config)
  - settings.php (pre-configured with connectivity settings for mysql)
- php-fpm Service exposing port 9000
- webserver Service exposing ports 8081 (http) and 3000 (api)
- mysql Service exposing port 3306
- webserver Route for http
- webserver Route for api (optional)


## Requirements
- OpenShift Container Platform 4.6+
- git 

Read/Execute required for all files/directories in generatedata/
  chown 101:101 -R generatedata/
	chmod 755 -R generatedata/

Read/Write/Execute required for web/cache/
	chmod 777 -R generatedata/cache/

-- there may be a more restrictive setting as only generatedata/cache requires write, but this worked to get functional

semanage fcontext -a -t httpd_sys_rw_content_t "{sharedpath}/generatedata(/.*)?"
restorecon -Rv {sharedpath}/generatedata



