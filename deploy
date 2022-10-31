#!/bin/bash

TENANT_NAME=${1:? you should input the team name.}
TENANT_PORT=${2:? you should input the tenant port.}
echo "Start deploy pact broker service for "${TENANT_NAME}" on port "${TENANT_PORT}"..."
TENANT_PORT=${TENANT_PORT} docker-compose -p ${TENANT_NAME} up -d
echo "Successful!"