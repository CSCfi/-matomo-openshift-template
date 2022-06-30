# matomo-openshift-template

Matomo template for openshift. Matomo, previously known as Piwik, is an analytics tools that can be used as an alternative to Google analytics. It offers statistics on website visitors, including search engines and keywords they used, popular pages, etc.
[Overview of Matomo](https://matomo.org/)

## How to use this template:

This template creates an openshift Deployment. Install the template matomo-template.yaml file either by importing it to your OpenShift project via web console or CLI. CLI instructions are provided below.

## Creating the project on OpenShift

* *oc login "openshift-address" --token="token"*
* *oc new-project "project-name"*

## Deploying the databases (MariaDB)
To deploy the databse, in ourcase the MariaDB, it needs certain variables such as the Username, the Password and the Database-Name.
[Overview of Mariadb](https://mariadb.org/).
For the Mariadb environment variables that are needed are: 

- MYSQL_USER = "username"
- MYSQL_PASSWORD = "password"
- MYSQL_ROOT_PASSWORD= "rootpassword"
- MYSQL_DATABASE = "database_name"



For example you can deploy from the existing Openshift template for mariadb-persistent as follows:
```
oc new-app --template=mariadb-persistent -p MYSQL_USER=<username> -p MYSQL_PASSWORD=<password> -p MYSQL_ROOT_PASSWORD=<rootpassword> -p MYSQL_DATABASE=<database_name>
```

## Deploying Matomo template:

All variables are mandatory for Matomo application to be created. When using CSC Rahti service, consult [Rahti Documentation](https://rahtiapp.fi/) for the details of computing environment and project quotas.

### Variables:
- **Application Name**: (NAME) Unique identifier for your application. Recommended value - your username
- **Username**: (USERNAME) Create a new username for logging into Matomo
- **Password**: (PASSWORD) Create a new password for logging into Matomo
- **Application Hostname Suffix**: (APPLICATION_DOMAIN_SUFFIX) The exposed hostname suffix that will be used to create routes for app (Default when using CSC Rahti service: rahtiapp.fi)
- ** Matomo database user**: (MATOMO_DATABASE_USER)
- ** Matomo databae password user**: (MATOMO_DATABASE_PASSWORD)
- ** Matomo database name**: (MATOMO_DATABASE_NAME)

### Command line Deployment for Matomo:

Download the matomo-template.yaml from Gitlab.


Process and apply template using default values from template and passing you application specific parameters
```
oc process -f matomo-template.yaml -p NAME=<name>  -p USER=<username> PASSWORD=<password>  -p MATOMO_DATABASE_USER=<username> -p MATOMO_DATABASE_PASSWORD=<dbpassword> -p MARIADB_ROOT_PASSWORD=<rootpassword> -p MATOMO_DATABASE_NAME=<database_name> | oc apply -f -

```


### Deleting application and project
```
oc process -f matomo-template.yaml -p NAME=<name>  -p USER=<username> PASSWORD=<password>  -p MATOMO_DATABASE_USER=<username> -p MATOMO_DATABASE_PASSWORD=<dbpassword> -p MARIADB_ROOT_PASSWORD=<rootpassword> -p MATOMO_DATABASE_NAME=<database_name> | oc delete -f -

```
* *oc delete all -l app=rstudio*
* *oc delete secret -l app=matomo,deploymentconfig=matomo,template=matomo*
You might also want to delete the secret 
* *oc delete secret <secret-name>*
* *oc delete project "project-name"*


