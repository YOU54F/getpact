# Get Pact

Get Pact is a helper for Pact.io to get you started

You can run it directly from the Interwebs

`curl pact.saf.dev -Lso - | bash -s -- help`

The source is [here](https://gist.github.com/YOU54F/642ee8f54f1da813b0ee6e2a29628016) 

I set it up like the following in my `.zshrc` file

```sh
alias getpact_run="curl pact.saf.dev -Lso - | bash -s -- help"
alias getpact_dl="curl pact.saf.dev -Ls -o"
getpact_location="/Users/saf/dev/you54f/getpact/getpact"
getpact () {
  first_arg="$1"
  shift
  $getpact_location ${first_arg:-'help'} $@
}


```

## Options

```sh
______________________
  Get Pact
______________________
  Available Commands 
______________________
getpact help
getpact broker
getpact broker list
getpact broker deploy <tenant_name> <port>
getpact broker stop <tenant_name>
getpact broker remove <tenant_name>
getpact broker backup <tenant_name>
getpact broker restore <tenant_name>
getpact broker restore <tenant_name>
getpact doctor
getpact doctor-env
getpact install-cli
getpact install-cli ruby
getpact install-cli docker
getpact install-cli standalone
______________________

```

## Broker on demand

Pact Broker SafS is a Pact contract file middleware service that supports multi-tenancy.

I would recommend [Pactflow](https://pactflow.io/) if you are looking for a secure Hosted SaaS platform, this offers a really neat way to spin up and backup local brokers for testing purposes.

This is **NOT** suitable for production, and doesn't offer enterprise grade features such as OAuth login mechanisms.

With thanks to the original author

> **Source**: https://github.com/tongzh/pact-broker-saas


### characteristic

#### multi-tenancy

Physical multi-tenancy is achieved through container isolation. Pull up PactBroker and dependent postgresql database services for each tenant through docker-compose, and the services of each tenant are distinguished by ports.


### Operation and maintenance


#### Add tenant


When selecting a port number for a new tenant, it should avoid conflict with the existing port number. You can `docker ps | grep pact_broker` view the occupied port number by running commands.

Add tenant run command

`getpact deploy <tenant_name> <tenant_port>`

For example, for a new tenant named tenant serving on port 12999, run the following command:

`getpact deploy tenant 12999`

The corresponding pact_broker container and pact_db container can be created, and the service can be accessed through port 12999.

#### Stop the tenant container from running

Stop the tenant containers (including the two containers pact_broker and pact_db) and run the command:


`getpact stop <tenant_name>`

For example, to stop the tenant's container, run the following command:


`getpact stop tenant`

#### Delete tenant containers and data volumes

Delete the stopped tenant containers (including the two containers pact_broker and pact_db), delete the data volume, and run the command:

`getpact remove <tenant_name>`

For example, to delete the container and data volume of the tenant, run the following command:

`getpact remove tenant`

#### Backup tenant data

Back up the tenant data to a file in the current folder `<tenant_name>-data.tar`, run the command:

`getpact backup <tenant_name>`

For example, to back up the tenant's data, run the following command:

`getpact backup tenant`

The tenant's database data will be backed up to files in the current folder `tenant-data.tar`.

#### Restoring tenant data

First prepare the data source file in the current directory, the file name must be in the `<tenant_name>-data.tar` form, run the command:

`getpact restore <tenant_name>`

For example, to restore the tenant's data, run the following command:

`getpact restore tenant`

The tenant data will be restored from the files in the current folder `tenant-data.tar`, and the postgresql container will be restarted (due to the mechanism of postgresql itself, the container must be restarted to read the restored data).
