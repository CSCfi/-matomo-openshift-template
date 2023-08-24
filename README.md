# Matomo OpenShift Template

This repository contains a template for deploying Matomo on OpenShift. Matomo, previously known as Piwik, is a web analytics tool that can be used as an alternative to Google Analytics. It offers statistics on website visitors, including search engines and keywords they used, popular pages,and more. You can learn more about Matomo by visiting the 
[Matomo website](https://matomo.org/).

## Usage Instructions

This template creates an OpenShift Deployment. Install the template `matomo-template.yaml`
file either by importing it to your OpenShift project via web console or CLI. CLI
instructions are provided below.

### 1. Creating a new project on OpenShift

1. `oc login <openshift-address> --token=<token>`
2. `oc new-project <project-name> --description='csc_project: <project-id>'`


### 2. Deploying the Matomo template

When you deploy the Matomo template, you're not only setting up Matomo itself but also deploying a MariaDB deployment, which is included in the template to serve as the backend database.

In order to successfully create the Matomo service, it is necessary to provide all the required variables. When using CSC Rahti,
consult the [Rahti documentation](https://rahtiapp.fi/) for details on the computing environment and project quotas.

#### Variables

Ensure that you have the following variables ready and properly configured before initiating the deployment of the Matomo template:

* **Database username**, `MYSQL_USER=<db-username>`: This variable holds the chosen username for the database.
* **Database password**, `MYSQL_PASSWORD=<db-password>`: The password for database user.
* **Database root password** `MYSQL_ROOT_PASSWORD=<db-root-password>`: The root password for the MariaDB instance.
* **Database name** `MYSQL_DATABASE=<db-name>`: The name of the database.
* **Application name**, `MATOMO_APP_NAME=<app-name>`:  Unique identifier for your application. 
  It is advisable to use your username as the recommended value.
* **Username**, `MATOMO_USERNAME=<app-username>`: Create a new username for logging into Matomo
* **Password**, `MATOMO_PASSWORD=<app-password>`: Create a new password for logging into Matomo
* **Matomo database username**, `MATOMO_DATABASE_USER=<db-username>`: Same sername that was
  specified for MariaDB.
* **Matomo database password**, `MATOMO_DATABASE_PASSWORD=<db-password>`: Same password
  (non-root) that was specified for MariaDB.
* **Matomo database name**, `MATOMO_DATABASE_NAME=<db-name>`: Same database name that was
  specified MariaDB.


### Command-line deployment of Matomo

1. Download the `matomo-template.yaml` from from this repo.
2. Process and apply template using default values from template and passing you application
   specific parameters, sample example shown below:-

```bash
oc process -f matomo-template.yaml -p MATOMO_APP_NAME="matomo-app-johndoe" -p MYSQL_USER="matomouser" -p MYSQL_PASSWORD="matomo" -p MYSQL_ROOT_PASSWORD="letmeinroot" -p MYSQL_DATABASE="matomodb" -p MATOMO_USERNAME="matomo" -p MATOMO_PASSWORD="matomopass" -p MATOMO_DATABASE_USER="matomouser"  -p  MATOMO_DATABASE_PASSWORD="matomo" -p  MATOMO_DATABASE_NAME="matomodb" | oc apply -f 
```

### Deleting the application and project

```bash
oc process -f matomo-template.yaml -p MATOMO_APP_NAME="matomo-app-johndoe" -p MYSQL_USER="matomouser" -p MYSQL_PASSWORD="matomo" -p MYSQL_ROOT_PASSWORD="letmeinroot" -p MYSQL_DATABASE="matomodb" -p MATOMO_USERNAME="matomo" -p MATOMO_PASSWORD="matomopass" -p MATOMO_DATABASE_USER="matomouser"  -p  MATOMO_DATABASE_PASSWORD="matomo" -p  MATOMO_DATABASE_NAME="matomodb" | oc delete -f -
```

or 

* `oc delete all -l app=<app-name>`
* `oc delete secret -l app=<app-name>,deploymentconfig=<app-name>,template=<app-name>`
  
You might also want to delete the secret and project

* `oc delete secret <secret-name>`
* `oc delete project <project-name>`
