# Focus Budget Manager

This application is being developed as part of a tutorial series.

You can access it [here](https://medium.com/netscape/building-a-budget-manager-with-vue-js-and-node-js-part-i-f3d7311822a8).

## Build Setup

```bash
# To install the RESTful API dependencies go to
# server folder
npm i

# Go to server/BudgetManagerAPI/config/database
# and setup a MongoDB

# Go back to your server folder
npm run start

# In your client folder
npm i

# Serve with hot reload at localhost:8080
npm run start

# For more details about npm scripts on application
# like how to build for production, read the README
# located inside applications folder
```

## Build with Docker

```bash

# to run server/client apps locally
docker-compose up --build

# to watch container's logs 
docker logs -f ${CONTAINER_NAME}

# to go to Mongo's shell
docker exec -it ${MONGO_CONTAINER_NAME} mongo

```
