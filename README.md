# matomo-openshift-template

Matomo template for OpenShift. Matomo, previously known as Piwik, is a web analytics
tool that can be used as an alternative to Google Analytics. It offers statistics on
website visitors, including search engines and keywords they used, popular pages, etc.
[Overview of Matomo](https://matomo.org/).

## How to use this template

This template creates an OpenShift Deployment. Install the template `matomo-template.yaml`
file either by importing it to your OpenShift project via web console or CLI. CLI
instructions are provided below.

## 1. Creating a new project on OpenShift

1. `oc login <openshift-address> --token=<token>`
2. `oc new-project <project-name> --description='csc_project: <project-id>'`

## 2. Deploying the database (MariaDB)

To deploy the required database, in our case the MariaDB, it needs certain
variables such as username, (root) password and the database name. [Overview of
MariaDB](https://mariadb.org/). These are stored in the following environment
variables:

* `MYSQL_USER=<db-username>`
* `MYSQL_PASSWORD=<db-password>`
* `MYSQL_ROOT_PASSWORD=<db-root-password>`
* `MYSQL_DATABASE=<db-name>`

For example, you can deploy the existing OpenShift template on Rahti, `mariadb-persistent`,
as follows:

```bash
oc new-app --template=mariadb-persistent -p MYSQL_USER=<db-username> -p MYSQL_PASSWORD=<db-password> -p MYSQL_ROOT_PASSWORD=<db-root-password> -p MYSQL_DATABASE=<db-name>
```

## 3. Deploying the Matomo template

All variables are mandatory for the Matomo service to be created. When using CSC Rahti,
consult the [Rahti documentation](https://rahtiapp.fi/) for details on the computing
environment and project quotas.

### Variables

* **Application name**, `NAME=<app-name>`: Unique identifier for your application,
  recommended value - your username
* **Username**, `USERNAME=<app-username>`: Create a new username for logging into Matomo
* **Password**, `PASSWORD=<app-password>`: Create a new password for logging into Matomo
* **Application hostname suffix**, `APPLICATION_DOMAIN_SUFFIX=<app-suffix>`: The exposed
  hostname suffix that will be used to create routes for the application. Default when
  using Rahti is`rahtiapp.fi`
* **Matomo database username**, `MATOMO_DATABASE_USER=<db-username>`: Username that was
  specified when deploying MariaDB in the previous step
* **Matomo database password**, `MATOMO_DATABASE_PASSWORD=<db-password>`: Password
  (non-root) that was specified when deploying MariaDB in the previous step
* **Matomo database name**, `MATOMO_DATABASE_NAME=<db-name>`: Database name that was
  specified when deploying MariaDB in the previous step

### Command-line deployment of Matomo

1. Download the `matomo-template.yaml` from from this repo.
2. Process and apply template using default values from template and passing you application
   specific parameters.

```bash
oc process -f matomo-template.yaml -p NAME=<app-name>  -p USER=<app-username> PASSWORD=<app-password>  -p MATOMO_DATABASE_USER=<db-username> -p MATOMO_DATABASE_PASSWORD=<db-password> -p MATOMO_DATABASE_NAME=<db-name> | oc apply -f -
```

### Deleting the application and project

```bash
oc process -f matomo-template.yaml -p NAME=<app-name>  -p USER=<app-username> PASSWORD=<app-password>  -p MATOMO_DATABASE_USER=<db-username> -p MATOMO_DATABASE_PASSWORD=<db-password> -p MATOMO_DATABASE_NAME=<db-name> | oc delete -f -
```

* `oc delete all -l app=<app-name>`
* `oc delete secret -l app=<app-name>,deploymentconfig=<app-name>,template=<app-name>`
  
You might also want to delete the secret and project

* `oc delete secret <secret-name>`
* `oc delete project <project-name>`
