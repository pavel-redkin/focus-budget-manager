FROM node:10.13-alpine

RUN apk add --no-cache make gcc g++ python

# Create app directory
RUN mkdir -p /srv/src/client
WORKDIR /srv/src/client

# Install app dependencies
COPY package.json /srv/src/client
COPY package-lock.json /srv/src/client

RUN npm install

# Bundle app source
COPY . /srv/src/client

CMD ["npm", "run", "start"]
