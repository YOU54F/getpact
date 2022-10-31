# Pact Broker SafS

Pact Broker SafS is a Pact contract file middleware service that supports multi-tenancy.

I would recommend [Pactflow](https://pactflow.io/) if you are looking for a secure Hosted SaaS platform, this offers a really neat way to spin up and backup local brokers for testing purposes.

This is **NOT** suitable for production, and doesn't offer enterprise grade features such as OAuth login mechanisms.

With thanks to the original author

> **Source**: https://github.com/tongzh/pact-broker-saas

I use it like this

```sh
pactsafs () {
  <absolute_path>/pact-broker-safs/$1
}
```

```sh
___Pact Broker SafS___
______________________
__Available Commands__
______________________
pactsafs help
pactsafs list
pactsafs deploy <tenant_name> <tenant_port>
pactsafs stop <tenant_name>
pactsafs remove <tenant_name>
pactsafs backup <tenant_name>
pactsafs restore <tenant_name>
______________________
```


## characteristic

### multi-tenancy

Physical multi-tenancy is achieved through container isolation. Pull up PactBroker and dependent postgresql database services for each tenant through docker-compose, and the services of each tenant are distinguished by ports.


## Operation and maintenance


### Add tenant


When selecting a port number for a new tenant, it should avoid conflict with the existing port number. You can `docker ps | grep pact_broker` view the occupied port number by running commands.

Add tenant run command

`./deploy <tenant_name> <tenant_port>`

For example, for a new tenant named tenant serving on port 12999, run the following command:

`./deploy tenant 12999`

The corresponding pact_broker container and pact_db container can be created, and the service can be accessed through port 12999.

### Stop the tenant container from running

Stop the tenant containers (including the two containers pact_broker and pact_db) and run the command:


`./stop <tenant_name>`

For example, to stop the tenant's container, run the following command:


`./stop tenant`

### Delete tenant containers and data volumes

Delete the stopped tenant containers (including the two containers pact_broker and pact_db), delete the data volume, and run the command:

`./remove <tenant_name>`

For example, to delete the container and data volume of the tenant, run the following command:

`./remove tenant`

### Backup tenant data

Back up the tenant data to a file in the current folder `<tenant_name>-data.tar`, run the command:

`./backup <tenant_name>`

For example, to back up the tenant's data, run the following command:

`./backup tenant`

The tenant's database data will be backed up to files in the current folder `tenant-data.tar`.

### Restoring tenant data

First prepare the data source file in the current directory, the file name must be in the `<tenant_name>-data.tar` form, run the command:

`./restore <tenant_name>`

For example, to restore the tenant's data, run the following command:

`./restore tenant`

The tenant data will be restored from the files in the current folder `tenant-data.tar`, and the postgresql container will be restarted (due to the mechanism of postgresql itself, the container must be restarted to read the restored data).