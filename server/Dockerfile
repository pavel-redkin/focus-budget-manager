FROM node:10.13-alpine

RUN apk add --no-cache make gcc g++ python

# Create app directory
RUN mkdir -p /srv/src/server
WORKDIR /srv/src/server

# Install app dependencies
COPY package.json /srv/src/server
COPY package-lock.json /srv/src/server

RUN npm install

# Bundle app source
COPY . /srv/src/server

CMD ["npm", "run", "start"]
