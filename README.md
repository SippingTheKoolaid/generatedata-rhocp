#generatedata.com OpenShift installation

Steps to deploy the website code from benkeen/generatedata as microservices in Red Hat OpenShift Container Platform.  Some modifcations are required because, unlike Docker where container deployments run as root by default, OpenShift securely runs containers using non-root credentials.

##Design considerations
- httpd container (website)
-- requires persistent storage shared with php service to run code and render pages
- php-fpm container (php)
-- requires presistent storage shared with website to read code and execute as necessary
- mysql container (database)


##Basic 
- Persistent Volume for mysql database (read/write many)
- Persistent Volume for php/web files (read/write many)
