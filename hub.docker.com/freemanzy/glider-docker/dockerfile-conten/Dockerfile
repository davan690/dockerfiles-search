FROM node:latest

WORKDIR /usr/src/app

COPY . .

ENV NODE_ENV=production

RUN npm install

CMD ["npm", "run", "start"]
