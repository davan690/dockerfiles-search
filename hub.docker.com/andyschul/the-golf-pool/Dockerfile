FROM node:11-alpine

WORKDIR /client

COPY package.json ./
COPY package-lock.json ./

RUN npm install

EXPOSE 3000

COPY . .
CMD [ "npm", "start" ]
